Il est possible d'afficher les températures de la tablette et du microcontrôleur (MCU) de la carte de l'imprimante (uniquement pour des 
cartes 32 bits équipées de à base de μcontrôleurs: ATSAM, ATSAMD, et STM32 
=> voir [ici](https://www.klipper3d.org/fr/Config_Reference.html#capteur-de-temperature-integre-au-microcontroleur)

Ajouter dans le printer.cfg :
```
#==================  Temperatures host + μcontroler =================
[temperature_sensor SonicPad]
sensor_type: temperature_host
min_temp: 10
max_temp: 75

[temperature_sensor STM32F103]
sensor_type: temperature_mcu
min_temp: 10
max_temp: 75
```

