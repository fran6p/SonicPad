# Comment « oublier » un réseau sans fil avec la tablette Sonic Pad :

***Préalable:*** obtenir un accès root

1. Se connecter en SSH.

2. Via un éditeur de texte, modifier le fichier : **/overlay/upper/etc/wifi/wpa_supplicant.conf**
`vi /overlay/upper/etc/wifi/wpa_supplicant.conf`

3. Supprimer le réseau sans fil du fichier (toute la section débutant par network={ … } ) :
```
ctrl_interface=/etc/wifi/sockets
disable_scan_offload=1
update_config=1
wowlan_triggers=any

network={
        ssid="Creality"
        scan_ssid=1
        psk="huawei123"
        key_mgmt=WPA-PSK
        priority=1
}

network={
        ssid="NOM-DU-RESEAU-SANS-FIL"
        scan_ssid=1
        psk="motdepassedureseauwifi"
        key_mgmt=WPA-PSK
        priority=2
}

```
4. Enregistrer le fichier modifié ( ESC :wq ).

5. Redémarrer la tablette via un `reboot`

***RAPPEL:***
**Toujours conserver des sauvegardes de tous les fichiers modifiés afin de pouvoir revenir en arrière si nécessaire.**

`cp /overlay/upper/etc/wifi/wpa_supplicant.conf /overlay/upper/etc/wifi/wpa_supplicant.conf.bkup`
