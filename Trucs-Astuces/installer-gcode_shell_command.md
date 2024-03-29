L'utilisateur th33xitus, auteur de [KIAUH](https://github.com/th33xitus/kiauh) a créé un module python permettant d'exécuter des scripts shell via des macros Klipper.

La première étape est d'installer le script python au bon endroit pour que Klipper le gère.
Toutes les manipulations sont effectuées en tant qu'utilisateur «root» :

1- Se placer dans le répertoire souhaité :

`cd /usr/share/klipper/klippy/extras`

2- Récupérer le script et le rendre exécutable :

```
wget "https://raw.githubusercontent.com/th33xitus/kiauh/master/resources/gcode_shell_command.py"
chmod +x gcode_shell_command.py
```

Ce script ajoute un GCode étendu: RUN_SHELL_COMMAND utilisable dans des macros Gcode. Un fichier shell_command.cfg doit être ajouté à la configuration via une directive «include» dans le printer.cfg. Celui contiendra des sections à l'identique de [gcode_macro] pour l'appel des scripts, via des macros «shell_command» [gcode_shell_command …]. Il suffit de créer les macros Gcode, les macros Shell_command et les scripts shell voulus.

Exemple extrait du fichier shell_command.cfg :
```
[gcode_shell_command adxl_x]
command: sh /mnt/UDISK/printer_config/shell_commands/adxl_x.sh 
timeout: 300.
verbose: True

[gcode_shell_command adxl_y]
command: sh /mnt/UDISK/printer_config/shell_commands/adxl_y.sh 
timeout: 300.
verbose: True
```

Exemple pour le test de résonance :

Macros Gcode :
```
################################### INPUT SHAPER #####################################
# Manually via ssh to obtain the images (PNG) of the resonances for each axe (X/Y).
# Example for the Creality Sonic Pad (OS=OpenWRT, use /usr/share as 'home' and 'root' as user !!!)
# Axe X:
# /usr/share/klipper/scripts/calibrate_shaper.py /tmp/calibration_data_x_*.csv -o /mnt/UDISK/printer_config/shaper_calibrate_x.png
# Axe Y:
# /usr/share/klipper/scripts/calibrate_shaper.py /tmp/calibration_data_y_*.csv -o /mnt/UDISK/printer_config/shaper_calibrate_y.png
#
# If root on the Sonic Pad, test with 'shell_command', the shell is 'ash' from busybox so use with caution.
# Read more about measuring resonances, smoothing, offline processing of shaper data etc.
# https://www.klipper3d.org/Measuring_Resonances.html
#
# Input shaper auto-calibration (run tests then generate csv output)
# Don't forget SAVE_CONFIG to save and restart Klipper
# The value 'max_accel' won't be automatically modified, you have to do it in the [printer] section, according to the results
# of the auto-calibration.
# With 'bed-slinger' use the lowest max_accel of X/Y axis.
#
[gcode_macro ADXL_TEST]
description: ADXL Test
gcode:
  ACCELEROMETER_QUERY

[gcode_macro ADXL_NOISE]
description: Measure Accelerometer Noise
gcode:
  MEASURE_AXES_NOISE

[gcode_macro HOTEND_INPUT_SHAPER]
description: test resonances in x direction for the hotend
gcode:
  M118 DO NOT TOUCH THE PRINTER UNTIL DONE!!!
  _HOME_CHECK
  SHAPER_CALIBRATE AXIS=X
  RUN_SHELL_COMMAND CMD=adxl_x
  M118 Test done
  SAVE_CONFIG
  
[gcode_macro BED_INPUT_SHAPER]
description: test resonances in y direction for the heated bed
gcode:
  M118 DO NOT TOUCH THE PRINTER UNTIL DONE!!!
  _HOME_CHECK
  SHAPER_CALIBRATE AXIS=Y
  RUN_SHELL_COMMAND CMD=adxl_y
  M118 Test done
  SAVE_CONFIG

[gcode_macro ADXL_SHAPE_ALL]
description: Test resonances for both axis
gcode:
    M118 DO NOT TOUCH THE PRINTER UNTIL DONE!!!
    LAZY_HOME
    SHAPER_CALIBRATE
    RUN_SHELL_COMMAND CMD=adxl_x
    RUN_SHELL_COMMAND CMD=adxl_y
    M118 Test done
    SAVE_CONFIG
```

Scripts shell :
```
#!/bin/sh
#
# Create PNG from csv file issued after INPUT_SHAPING, X axis
#

# Paths
# Creality use OpenWRT as OS so the shell is from busybox: ash
#
DATE=$(date +"%Y%m%d")
SCRIPTS="/usr/share/klipper/scripts/calibrate_shaper.py"
CSV_FILE="/tmp/calibration_data_x_*.csv"
PNG_FILE="/mnt/UDISK/printer_config/shaper_calibrate_x_$DATE.png"

echo "Paths :"
echo "-------"
echo .
echo $DATE
echo $SCRIPTS
echo $CSV_FILE
echo $PNG_FILE
echo .

$SCRIPTS $CSV_FILE -o $PNG_FILE
```
Le script shell pour l'axe Y peut être déduit du précédent :smirk:

A l'extinction de la tablette le répertoire /tmp est vidé, les fichiers CSV issus des tests de résonances seront perdus. Si on veut pouvoir les réutiliser , il faut les transférer dans un endroit persistant (/mnt/UDISK/printer_config par exemple).
Là encore un script shell permet d'automatiser la copie :
```
#!/bin/sh
#
# Backup csv file as they are deleted when SonicPad is powered off (/tmp directory emptied at poweroff)
#

# Paths
# Creality use OpenWRT as OS, the shell is from busybox: ash
#
CSV_FILE="/tmp/calibration_data_*.csv"
DIR_CONF="/mnt/UDISK/printer_config"

cp $CSV_FILE $DIR_CONF
```
Il suffit de créer une macro Gcode et son pendant shell_command pour pouvoir effectuer la sauvegarde directement via le terminal de Klipper :
```
[gcode_macro BACKUP_CSV]
description: Backup csv files registered in /tmp directory emptied on poweroff
gcode:
    M118 Backup all csv files !
    RUN_SHELL_COMMAND CMD=bkup_csv
    M118 Backup done
    
…
# Sauvegarde des fichiers csv issus de tests de résonance
[gcode_shell_command bkup_csv]
command: sh /mnt/UDISK/printer_config/shell_commands/backup_csv_files.sh 
timeout: 300.
verbose: True
```

:smiley:
