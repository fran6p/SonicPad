LED bleue:

Cette LED clignote à une période de 500ms. Elle indique que tous les services ont bien été
démarrés.

Le service qui est lancé se situe dans `/etc/init.d/`  et se nomme `sys_led`

Son contenu :
```
root@spad-1168:/etc/init.d# cat sys_led
#!/bin/sh /etc/rc.common

START=99
STOP=99

USE_PROCD=1

start_service() {
    [ -e /sys/class/leds/sys-led/trigger ] && echo timer > /sys/class/leds/sys-led/trigger
}

stop_service(){
    [ -e /sys/class/leds/sys-led/trigger ] && echo none > /sys/class/leds/sys-led/trigger
}

```
Les parmètres pilotables de la LED bleue sont modifiables dans `/sys/class/leds/sys-led/`
```
-rw-r--r--    1 root     root          4096 Jan 26 14:52 brightness
-rw-r--r--    1 root     root          4096 Jan 26 14:52 delay_off
-rw-r--r--    1 root     root          4096 Jan 26 14:52 delay_on
lrwxrwxrwx    1 root     root             0 Jan 26 14:52 device -> ../../../leds
-r--r--r--    1 root     root          4096 Jan 26 14:52 max_brightness
drwxr-xr-x    2 root     root             0 Jan 26 14:52 power
lrwxrwxrwx    1 root     root             0 Jan 26 14:52 subsystem -> ../../../../../class/leds
-rw-r--r--    1 root     root          4096 Jan  1  2020 trigger
-rw-r--r--    1 root     root          4096 Jan  1  2020 uevent

```
max_brightness et brightness : 255
delay_on et delay_off : 500
