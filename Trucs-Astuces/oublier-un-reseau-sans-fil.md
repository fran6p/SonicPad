# Comment « oublier » un réseau sans fil avec votre Sonic Pad :

***Préalable:*** obtenir un accès root

1. Se connectee en SSH et modifier le fichier avec un éditeur de texte (vi): /overlay/upper/etc/wifi/wpa_supplicant.conf
2. Supprimer le réseau sans fil du fichier (la section débutant par network={ … }
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
3. enregistrer le fichier modifié ( ESC :wq ).

Redémarrer la tablette via un `reboot`

Toujours conserver des sauvegardes de tous les fichiers modifiés afin de pouvoir revenir en arrière si nécessaire.
`cp /overlay/upper/etc/wifi/wpa_supplicant.conf /overlay/upper/etc/wifi/wpa_supplicant.conf.bkup`
