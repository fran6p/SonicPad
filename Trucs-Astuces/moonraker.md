La version Creality de Moonraker utilise le systéme d'init pour lancer et arrêter des services en lieu et place de systemd.
 Sudo n'est pas implémenté avec OpenWRT (OS choisi par Creality).

Les commandes Host et Services (Reboot, Shutdown, Restart Moonraker, Restart Klipper etc.) dans fluidd/mainsail ne fonctionnent pas à cause de moonraker 
qui utilise la syntaxe debian (systemd).

Pour résoudre ce problème, il faut modifier le fichier `/usr/share/moonraker/moonraker/components/machine.py`. 

Utiliser ces commandes pour la mise hors tension de l'hôte, le reboot et le redémarrage des services respectivement:
```
self._execute_cmd("command") : 
"poweroff", 
"reboot", 
f'/etc/init.d/{service_name} {action}'
```

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

Modifier ce qui suit pour permettre la vérification des services autorisés:

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
## Redémaarer Klipper à l'allumage de l'imprimante

Vous éteignez votre imprimante mais laissez le sonic pad allumé ? Fatigué de devoir appuyer sur 'Redémarrer Klipper' à chaque fois que vous rallumez votre imprimante?

Vous aurez besoin d'un accès root, pour pouvoir réaliser cette manipulation.

Suivez ce qui suit pour que klipper redémarre automatiquement lorsque votre imprimante est rallumée.

**[Besoin d'un accès root]**

Créez `/etc/udev/rules.d/98-klipper.rules` avec le contenu:

```
SUBSYSTEM=="usb", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="7523", ACTION=="add", RUN+="/bin/sh -c 'echo RESTART > /tmp/printer'"
```

puis :

```
chmod +x /etc/udev/rules.d/98-klipper.rules
udevadm control --reload (ou redémarrer le Sonic Pad)
```

--------------------------------------
## Comment « oublier » un réseau sans fil avec votre Sonic Pad

Nécessite un accès root

1. Connectez-vous en SSH et modifiez ce fichier avec un éditeur de texte: `/overlay/upper/etc/wifi/wpa_supplicant.conf`
2. Supprimez votre réseau sans fil du fichier et remplacez-le par le nouveau puis enregistrer le fichier.
3. Redémarrez votre tablette.

Rappel: toujours conserver des sauvegardes de tous les fichiers modifiés afin de pouvoir revenir en arrière si nécessaire.

--------------------------------------

