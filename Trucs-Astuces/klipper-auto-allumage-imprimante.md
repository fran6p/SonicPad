## Démarrer / redémarrer Klipper à l'allumage de l'imprimante

L'imprimante a été éteinte mais le Sonic Pad est resté allumé.

Procrastinateur né, vous trouvez trop fatiguant de devoir appuyer sur 'Redémarrer Klipper' à chaque allumage de l'imprimante ?

>  Un accès root est obligatoire pour pouvoir réaliser cette manipulation.

Suivre ce qui suit pour que klipper redémarre automatiquement lorsque l'imprimante est rallumée.

**[RAPPEL] manipulations à réaliser en tant qu'utilisateur «root»**

Créer une régle UDEV `/etc/udev/rules.d/98-klipper.rules` avec le contenu:

```
SUBSYSTEM=="usb", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="7523", ACTION=="add", RUN+="/bin/sh -c 'echo RESTART > /tmp/printer'"
```

puis modifier les droits (exécution) de cette règle (pas craiment nécessaire) et mettre à jour la base UDEV :

`chmod +x /etc/udev/rules.d/98-klipper.rules`

puis
`udevadm control --reload`

ou
`udevadm control --reload-rules`

ou encore, redémarrer le Sonic Pad


### NOTE

L'idVendor (1a86) et l'idProduct(7523) correspondent aux ID xxxx:yyyy retournées par la commande `lsusb` pour le port USB où est connectée l'imprimante, elles dépendent fortement de la carte contrôleur qui implémente la connexion USB (là ça correspond à bon nombre d'imprimantes Creality utilisant le pilote CH341) :

>  ```
>  $ lsusb
>  Bus 001 Device 013: ID **1a86:7523 QinHeng Electronics CH340 serial converter**
>  Bus 001 Device 004: ID 1bcf:2285 Sunplus Innovation Technology Inc. papalook FHD Camera
>  Bus 001 Device 003: ID 0bda:8179 Realtek Semiconductor Corp. RTL8188EUS 802.11n Wireless Network Adapter
>  Bus 001 Device 002: ID 1a40:0101 Terminus Technology Inc. Hub
>  Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
>  ```

Sources:
- [Github Klipper](https://github.com/Klipper3d/klipper/issues/835)
