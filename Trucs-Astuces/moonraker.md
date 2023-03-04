## Permettre l'arrêt, le redémarrage de la tablette via les interfaces Web

Creality utilise le systéme d'init pour lancer et arrêter les services en lieu et place de systemd. De plus `sudo` n'est pas implémenté avec le système d'exploitation choisi par Creality 5tinaLinux, dérivé d'OpenWRT).

Les commandes Host et Services (Reboot, Shutdown, Restart Moonraker, Restart Klipper etc.) dans Fluidd/Mainsail ne fonctionnent pas à cause de Moonraker 
qui lui, utilise la syntaxe Debian (systemd).

Pour résoudre ce problème, il faut modifier le fichier `/usr/share/moonraker/moonraker/components/machine.py`. 

Pour que les boutons de commande du client soient fonctionnels, il faut modifier l'appel de ces commandes pour la mise hors tension de l'hôte, le reboot et le redémarrage des services respectivement:

   Rechercher dans le fichier `machine.py` les lignes contenant `self._execute_cmd( … )`
   
   Remplacer `"sudo shutdown now"` par `"poweroff"`, 
   
   Remplacer `"sudo shutdown -r now"` par `"reboot"`, 
   
   Remplacer `f'sudo systemctl {action} {service_name}'` par `f'/etc/init.d/{service_name} {action}'`


Avant modifications:
```
 async def shutdown_machine(self) -> None:
        await self._execute_cmd("sudo shutdown now")

    async def reboot_machine(self) -> None:
        await self._execute_cmd("sudo shutdown -r now")

    async def do_service_action(self,
                                action: str,
                                service_name: str
                                ) -> None:
        await self._execute_cmd(
            f'sudo systemctl {action} {service_name}')
```
Après modifications:
```
 async def shutdown_machine(self) -> None:
        await self._execute_cmd("poweroff")

    async def reboot_machine(self) -> None:
        await self._execute_cmd("reboot")

    async def do_service_action(self,
                                action: str,
                                service_name: str
                                ) -> None:
        await self._execute_cmd(
            f'/etc/init.d/{service_name} {action}')
```

Modifier également ce qui suit pour permettre la vérification des services autorisés:

```
async def _find_active_services(self):
        services = os.listdir("/etc/init.d/")
        for svc in services:
            for allowed in ALLOWED_SERVICES:
                if svc.startswith(allowed):
                    self.available_services.append(svc)
        self.system_info['available_services'] = self.available_services

```

--------------------------------------

## Redémarrer Klipper à l'allumage de l'imprimante

Extinction de l'imprimante alors que le Sonic Pad reste allumé ?

Trop fatiguant de devoir appuyer sur 'Redémarrer Klipper' à chaque fois qque l'imprimante est allumée ?

Un accès root est onligatoire pour pouvoir réaliser cette manipulation.

Suivre ce qui suit pour que klipper redémarre automatiquement lorsque l'imprimante est rallumée.

**[Besoin d'un accès root]**

Créer `/etc/udev/rules.d/98-klipper.rules` avec le contenu:

```
SUBSYSTEM=="usb", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="7523", ACTION=="add", RUN+="/bin/sh -c 'echo RESTART > /tmp/printer'"
```

puis :

```
chmod +x /etc/udev/rules.d/98-klipper.rules
udevadm control --reload (ou redémarrer le Sonic Pad)
```

--------------------------------------

