## Permettre l'arrêt, le redémarrage de la tablette via les interfaces Web

Creality utilise le «vieux» systéme d'init pour lancer et arrêter les services en lieu et place du «plus moderne» systemd. De plus `sudo` n'est pas implémenté avec le système d'exploitation choisi par Creality (TinaLinux (Linux Sunxi Allwinner), dérivé d'OpenWRT).

Les commandes Host et Services (Reboot, Shutdown, Restart Moonraker, Restart Klipper etc.) dans Fluidd/Mainsail ne fonctionnent pas à cause de Moonraker 
qui lui, utilise la syntaxe Debian (systemd + sudo).

Pour résoudre ce problème, il faut modifier le fichier `/usr/share/moonraker/moonraker/components/machine.py`. 

Pour que les boutons de commandes du client soient fonctionnels, il faut modifier l'appel de celles-ci pour la mise hors tension de l'hôte, le reboot et le redémarrage des services respectivement:

   Rechercher dans le fichier `machine.py` les lignes contenant `self._execute_cmd( … )`
   
   Remplacer `"sudo shutdown now"` par `"poweroff"`, 
   
   Remplacer `"sudo shutdown -r now"` par `"reboot"`, 
   
   Remplacer `f'sudo systemctl {action} {service_name}'` par `f'/etc/init.d/{service_name} {action}'`


Avant modifications du fichier `machine.py`:
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
Après modifications du fichier `machine.py` :
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

En gros, un «diff» :
```
@@ -125,17 +125,17 @@
         return "ok"

     async def shutdown_machine(self) -> None:
-        await self._execute_cmd("poweroff")
+        await self._execute_cmd("sudo shutdown now")

     async def reboot_machine(self) -> None:
-        await self._execute_cmd("reboot")
+        await self._execute_cmd("sudo shutdown -r now")

     async def do_service_action(self,
                                 action: str,
                                 service_name: str
                                 ) -> None:
         await self._execute_cmd(
-            f'/etc/init.d/{service_name} {action}')
+            f'sudo systemctl {action} {service_name}')
```

On peut modifier également la liste des services «à surveiller» pour permettre la vérification de ceux autorisés:

```
async def _find_active_services(self):
        services = os.listdir("/etc/init.d/")
        for svc in services:
            for allowed in ALLOWED_SERVICES:
                if svc.startswith(allowed):
                    self.available_services.append(svc)
        self.system_info['available_services'] = self.available_services

```

Le «diff» :
```
@@ -288,7 +288,8 @@
         try:
             resp = await scmd.run_with_response()
             lines = resp.split('\n')
-            services = os.listdir("/etc/init.d/")
+            services = [line.split()[0].strip() for line in lines
+                        if ".service" in line.strip()]
             for svc in services:

```

--------------------------------------

## Redémarrer Klipper à l'allumage de l'imprimante

L'imprimante a été éteinte mais le Sonic Pad est resté allumé.

Vous trouvez qu'il est trop fatiguant de devoir appuyer sur 'Redémarrer Klipper' à chaque allumage de l'imprimante ?

Un accès root est onligatoire pour pouvoir réaliser cette manipulation.

Suivre ce qui suit pour que klipper redémarre automatiquement lorsque l'imprimante est rallumée.

**[Besoin d'un accès root]**

Créer une régle UDEV `/etc/udev/rules.d/98-klipper.rules` avec le contenu:

```
SUBSYSTEM=="usb", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="7523", ACTION=="add", RUN+="/bin/sh -c 'echo RESTART > /tmp/printer'"
```

puis modifier les droits (exécution) de cette règle et mettre à jour la base UDEV :

```
chmod +x /etc/udev/rules.d/98-klipper.rules
udevadm control --reload (ou redémarrer le Sonic Pad)
```

--------------------------------------

