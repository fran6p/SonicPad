Creality regroupe l'ensemble des firmwares à flasher sur la carte contrôleur ainsi que les fichiers «printer.cfg» des imprimantes gérées par la tablette
dans le répertoire `/usr/share/klipper-brain`

Les firmwares sont dans `/usr/share/klipper-brain/printer_config/firmware`. le contenu à la date de la dernière mise à jour (mars 2023) :

<details>
  <summary>(Cliquez pour agrandir!)</summary>


```
root@spad-1168:~# ls -al /usr/share/klipper-brain/printer_config/firmware/
drwxr-x--x    3 root     root           381 Mar  6 07:05 .
drwxr-x--x    6 root     root           376 Mar  6 07:05 ..
drwxr-x--x    2 root     root            94 Mar  6 07:05 STM32F4_UPDATE
-rwxr-x--x    1 root     root         73024 Mar  6 07:05 klipper1284P_v02.elf.hex
-rwxr-x--x    1 root     root         76546 Mar  6 07:05 klipper2560_v02.elf.hex
-rwxr-x--x    1 root     root         21744 Mar  6 07:05 klipper_103_28KBBootloader_CR10Smart_v02.bin
-rwxr-x--x    1 root     root         21744 Mar  6 07:05 klipper_103_64KBBootloader_CR10Smart_v02.bin
-rwxr-x--x    1 root     root         21720 Mar  6 07:05 klipper_103_v02.bin
-rwxr-x--x    1 root     root         21452 Mar  6 07:05 klipper_401_64KBbootloader_CR10Smart_v02.bin
-rwxr-x--x    1 root     root         21440 Mar  6 07:05 klipper_401_v02.bin
-rwxr-x--x    1 root     root         23916 Mar  6 07:05 klipper_407_prusamini_v02.bin
-rwxr-x--x    1 root     root         21752 Mar  6 07:05 klipper_ender7_103_v02.bin
root@spad-1168:~# ls -al /usr/share/klipper-brain/printer_config/firmware/STM32F4_UPDATE/
drwxr-x--x    2 root     root            94 Mar  6 07:05 .
drwxr-x--x    3 root     root           381 Mar  6 07:05 ..
-rwxr-x--x    1 root     root         21452 Mar  6 07:05 klipper_401_64KBbootloader_CR10Smart_v02.bin
-rwxr-x--x    1 root     root         21440 Mar  6 07:05 klipper_401_v02.bin 
```

 </details>
  
Les configurations des imprimantes sont dans `/usr/share/klipper-brain/printer_config/config`, la liste actuelle (mars 2023) :

<details>
  <summary>(Cliquez pour agrandir!)</summary>


```
root@spad-1168:~# ls -al /usr/share/klipper-brain/printer_config/config/
drwxr-x--x    2 root     root          1830 Mar  6 07:05 .
drwxr-x--x    6 root     root           376 Mar  6 07:05 ..
-rwxr-x--x    1 root     root          6524 Mar  6 07:05 printer--Cr30-103.cfg
-rwxr-x--x    1 root     root          5793 Mar  6 07:05 printer--Cr6max-103.cfg
-rwxr-x--x    1 root     root          5792 Mar  6 07:05 printer--Cr6se-103.cfg
-rwxr-x--x    1 root     root          6305 Mar  6 07:05 printer-Cr10-103.cfg
-rwxr-x--x    1 root     root          4713 Mar  6 07:05 printer-Cr10-AVR1284p.cfg
-rwxr-x--x    1 root     root          6093 Mar  6 07:05 printer-Cr10Smart-CR-FDM-v2.5.S1-103-64Kib.cfg
-rwxr-x--x    1 root     root          6050 Mar  6 07:05 printer-Cr10Smart-CR-FDM-v2.5.S1-401-64Kib.cfg
-rwxr-x--x    1 root     root          6247 Mar  6 07:05 printer-Cr10Smart-CRC-2405V1.2-103-28Kib.cfg
-rwxr-x--x    1 root     root          5635 Mar  6 07:05 printer-Cr10Smartpro-103.cfg
-rwxr-x--x    1 root     root          5787 Mar  6 07:05 printer-Cr10Smartpro-401.cfg
-rwxr-x--x    1 root     root          4312 Mar  6 07:05 printer-Cr10s4-AVR2560.cfg
-rwxr-x--x    1 root     root          4320 Mar  6 07:05 printer-Cr10s5-AVR2560.cfg
-rwxr-x--x    1 root     root          5260 Mar  6 07:05 printer-Cr10sprov2-AVR2560.cfg
-rwxr-x--x    1 root     root          5001 Mar  6 07:05 printer-Cr10v3-AVR2560.cfg
-rwxr-x--x    1 root     root          5505 Mar  6 07:05 printer-Cr200B-v2.4.S4.101-103.cfg
-rwxr-x--x    1 root     root          5509 Mar  6 07:05 printer-Cr200B-v4.2.5-v4.2.10-103.cfg
-rwxr-x--x    1 root     root          6130 Mar  6 07:05 printer-Cr30-103.cfg
-rwxr-x--x    1 root     root          5619 Mar  6 07:05 printer-Cr6se-V4.5.2-103.cfg
-rwxr-x--x    1 root     root          5828 Mar  6 07:05 printer-Ender3S1-401.cfg
-rwxr-x--x    1 root     root         10966 Mar  6 07:05 printer-Ender3V2-CRtouch-V4.2.2-V4.3.1.cfg
-rwxr-x--x    1 root     root          8737 Mar  6 07:05 printer-Ender3V2-CRtouch-V4.2.7.cfg
-rwxr-x--x    1 root     root         10884 Mar  6 07:05 printer-Ender3V2-Neo-CRtouch-V4.2.2.cfg
-rwxr-x--x    1 root     root          8430 Mar  6 07:05 printer-Ender3V2-V4.2.7.cfg
-rwxr-x--x    1 root     root          6602 Mar  6 07:05 printer-Ender3max-103.cfg
-rwxr-x--x    1 root     root          6108 Mar  6 07:05 printer-Ender3maxNeo-103.cfg
-rwxr-x--x    1 root     root          5531 Mar  6 07:05 printer-Ender3pro-103.cfg
-rwxr-x--x    1 root     root          5150 Mar  6 07:05 printer-Ender3pro-AVR1284p.cfg
-rwxr-x--x    1 root     root          8316 Mar  6 07:05 printer-Ender3pro-CRtouch-V4.2.7.cfg
-rwxr-x--x    1 root     root          8107 Mar  6 07:05 printer-Ender3pro-V4.2.7.cfg
-rwxr-x--x    1 root     root          5555 Mar  6 07:05 printer-Ender3s1-103.cfg
-rwxr-x--x    1 root     root          5834 Mar  6 07:05 printer-Ender3s1-401-changce.cfg
-rwxr-x--x    1 root     root          5828 Mar  6 07:05 printer-Ender3s1-401.cfg
-rwxr-x--x    1 root     root          6489 Mar  6 07:05 printer-Ender3s1plus-401.cfg
-rwxr-x--x    1 root     root          6141 Mar  6 07:05 printer-Ender3s1pro-103.cfg
-rwxr-x--x    1 root     root          6158 Mar  6 07:05 printer-Ender3s1pro-401.cfg
-rwxr-x--x    1 root     root         10752 Mar  6 07:05 printer-Ender3v2-V4.2.2-V4.3.1.cfg
-rwxr-x--x    1 root     root          4821 Mar  6 07:05 printer-Ender5plus-AVR2560.cfg
-rwxr-x--x    1 root     root          5164 Mar  6 07:05 printer-Ender5pro-103.cfg
-rwxr-x--x    1 root     root          5559 Mar  6 07:05 printer-Ender5pro-CRtouch-103.cfg
-rwxr-x--x    1 root     root          6115 Mar  6 07:05 printer-Ender5s1-401.cfg
-rwxr-x--x    1 root     root          5780 Mar  6 07:05 printer-Ender5s1-new-401.cfg
-rwxr-x--x    1 root     root          6078 Mar  6 07:05 printer-Ender6-103.cfg
-rwxr-x--x    1 root     root          4836 Mar  6 07:05 printer-Ender7-103.cfg
-rwxr-x--x    1 root     root          6639 Mar  6 07:05 printer-Prusamini-407.cfg
-rwxr-x--x    1 root     root          5454 Mar  6 07:05 printer-Prusamini.cfg
-rwxr-x--x    1 root     root          5430 Mar  6 07:05 printer-SermoonD1-103.cfg
-rwxr-x--x    1 root     root          5150 Mar  6 07:05 printer-ender3pro-AVR1284p.cfg
-rwxr-x--x    1 root     root          6125 Mar  6 07:05 printer-ender3pro-CRtouch-103.cfg
-rwxr-x--x    1 root     root          6712 Mar  6 07:05 printer_test.cfg
-rwxr-x--x    1 root     root          6554 Mar  6 07:05 printer_user.cfg 
```
  
</details>

Le répertoire `/usr/share/klipper-brain/printer_config/`contient également les fichiers de configuration de Moonraker (moonraker.conf),
le reverse proxy (nginx.conf), la prise en charge des timelapse (timelapse.cfg) et la prise en charge de caméras (webcam.txt, webcam1.txt) :

<details>
  <summary>(Cliquez pour agrandir!)</summary>


```
root@spad-1168:~# ls -al /usr/share/klipper-brain/printer_config/
drwxr-x--x    6 root     root           376 Mar  6 07:05 .
drwxr-x--x    7 root     root           139 Mar  6 07:05 ..
drwxr-x--x    6 root     root            64 Mar  6 07:05 Position_Calibration_Img
drwxr-x--x    2 root     root          1830 Mar  6 07:05 config
-rwxr-x--x    1 root     root         40960 Mar  6 07:05 default_faq.cxfaq
drwxr-x--x    3 root     root           381 Mar  6 07:05 firmware
-rwxr-x--x    1 root     root         56822 Mar  6 07:05 front_demo_1.jpg
-rwxr-x--x    1 root     root         59445 Mar  6 07:05 left_demo_1.jpg
-rwxr-x--x    1 root     root         28052 Mar  6 07:05 mcu_params.json
-rwxr-x--x    1 root     root          1322 Mar  6 07:05 moonraker.conf
-rwxr-x--x    1 root     root         40375 Mar  6 07:05 nginx.conf
drwxr-x--x    2 root     root           849 Mar  6 07:05 png
-rwxr-x--x    1 root     root             0 Mar  6 07:05 printer.cfg
-rwxr-x--x    1 root     root          2887 Mar  6 07:05 readme.txt
-rwxr-x--x    1 root     root        151193 Mar  6 07:05 right_demo_1.jpg
-rwxr-x--x    1 root     root         21947 Mar  6 07:05 timelapse.cfg
-rwxr-x--x    1 root     root         58035 Mar  6 07:05 top_demo_1.jpg
-rwxr-x--x    1 root     root          2586 Mar  6 07:05 webcam.txt
-rwxr-x--x    1 root     root          2593 Mar  6 07:05 webcam1.txt

```

</details>

Le fichier readme.txt reprend les modifications apportées par Creality (traduction automatique Chinois-Français ci-dessous) :

<details>
  <summary>(Cliquez pour agrandir!)</summary>

   V1.1 Changements de version
   1. ajuster la vitesse de [safe_z_home] à 200
   2. ajuster [printer], max_accel limit to 5000, enable max_z_velocity:5,max_z_accel : 100
   3. ajuster [extruder], désactiver pressure_advance
   4. ajuster [extruder], la température maximale de la buse est de 305 pour ender3S1 PRO, la température maximale est de 265 pour ender3S1/ender3V2
   5. ajuster [bed_mesh], la limite de vitesse de ender3V2 est 120, la limite de vitesse de ender3S1 PRO/ender3S1 est 150
   6. ajouter la configuration time lapse [include /mnt/UDISK/printer_config/timelapse.cfg]

   Version V1.2 changements
   1. suppression du code de configuration redondant
   2) Modifier la valeur de [virtual_sdcard] pour le chemin : ~/gcode_files
   3. modifier Ender 3V2 [extruder] [heater_bed] valeur min_temp à 0
   4. nouvelles instructions pour la compilation et l'installation du firmware de la partie inférieure du klipper

   Changements dans la version V1.3
   1.Correction d'urgence pour le champ [display] causant le problème de chauffage de la buse de la CR6SE. Ce champ s'applique à l'écran matriciel, l'écran sonique est inutile, supprimez ce champ.
   2) Correction Ender3 V2-CRtouch [stepper_z], enable_pin : !PC3, non activé, ce qui fait que l'axe Z ne se soulève pas.
   3. changer le nom du modèle Ender 3V2-CRtouch en Ender 3V2 ABL

   Modifications de la V1.4
   1. modifier Ender3-S1 [safe_z_home] home_xy_position:155,155
   2) Modifier [stepper_x] homing_speed : 80
   3) Modifier [stepper_y] homing_speed : 80
   4) Ender-3S1/S1 Pro modifié [stepper_y] position_max : 230
   5. nouveau modèle de taille de moulage # printer_size : 220x220x270
   6. nouveau [adxl345] spi_speed : 2000000

   Modification de la version V1.5
   1. correction du nivellement automatique, l'erreur signalée est hors plage

   Modifications V1.6
   1. suppression de la configuration de la détection des matériaux cassés de la série Ender3-V2
   2. ajout du fichier de configuration de la carte mère Ender3-V2-V4.2.7
   3. nouveau fichier de configuration de la carte mère Ender3V2-CRtouch-V4.2.7
   4. modification du nom du fichier de configuration dans un style unifié

   Changements dans la version V1.7
   1. nivellement adaptatif, ajout d'un algorithme de double interpolation
   2. modification de la spécification du nom du modèle

   Modification de la version V1.8
   1. ajouter [verify_heater extruder] 
      check_gain_time : 200 
      hystérésis : 5
   2. modifier les valeurs PID par défaut de [extruder] et [heater_bed].

   Changements dans la V1.9
   1) Modifier [safe_z_home] home_xy_position : 145,155 dans Ender3-S1/S1Pro
   2) Modifier [bltouch] x_offset : -30.0 dans Ender3-S1/S1Pro

   Version V2.0 Modifications
   1) Modifier [printer] max_z_velocity : 10 max_z_accel : 1000 dans Ender3-S1/S1 Pro
   2. ajouter [printer] square_corner_velocity : 5.0 dans Ender3-S1/S1 Pro/V2

   V2.1 Modifications
   1) Modification de [stepper_z] position_max : 275 dans Ender3-S1/S1 Pro
   2. modifier [stepper_z] position_max : 255 dans Ender3-V2

   Changements dans la version V2.2
   1) Modifier la définition de la macro [gcode_macro CANCEL_PRINT].
   2. modifier [stepper_x] position_max : 240 dans Ender3-V2-CRtouch
   3. mise à jour du firmware pour ajouter la logique sous-jacente du chauffage des exceptions.

   V2.3 Modifications
   1) Modifier Ender3-S1/S1 Pro [stepper_x] position_min : -6 position_endstop : -6 
   2. suppression de la macro-commande Ender3-V2 [delayed_gcode AUTOSTART].
 
   V2.4 Modifications
   1. modifier Ender3-S1/S1 Pro [stepper_x] position_min : -3 position_endstop : -3 

   V2.5 Modifications
   1. modifier Ender3-S1/S1 Pro [stepper_x] position_min : -5 position_endstop : -5 
   2. modifier Ender3-S1/S1 Pro plage d'impression

   Modifications de la version V2.6
   1. nouveau modèle Ender3-V2 Neo
   2. nouveau modèle Ender3-s1pro-103
   2. nouvelle définition de macro [gcode_macro G29].

   V2.7 Modifications
   1.modifié Ender3-V2-CRtouch-V4.2.7 [stepper_z] Configuration tactile modifiée
  
</details>

L'archive [printer_config.zip](https://github.com/fran6p/SonicPad/raw/main/Fichiers/printer_config.zip) contient l'ensemble des fichiers du répertoire printer_config (à la date de la mise à jour de mars 2023).

Pour ne récupérer que :
-  les [firmwares](https://github.com/fran6p/SonicPad/raw/main/Fichiers/firmware.zip),
-  les [fichiers de configuration imprimantes](https://github.com/fran6p/SonicPad/raw/main/Fichiers/config.zip) .

…
