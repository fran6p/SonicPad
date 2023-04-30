Lors d'une mise à jour OTA, il est fait appel à plusieurs scripts.

Au démarrage de la tablette, un «daemon» vérifie si mise à jour il y aurait via `/etc/init.d/S99swupdate_autorun`

```
#!/bin/sh /etc/rc.common
#Copyright (c) 2018-2020 Allwinner Technology Co. Ltd.

START=99
DEPEND=done

PROG=/sbin/swupdate_cmd.sh
PROG_PROGRESS=/sbin/swupdate-progress

start(){
    $PROG_PROGRESS -w > /dev/console 2>&1 &
    $PROG
}

Le script `/sbin/swupdate_cmd.sh`

```
#!/bin/sh
#Copyright (c) 2018-2020 Allwinner Technology Co. Ltd.


# ============================================================================
# GLOBAL FUNCTIONS
# ============================================================================

. /usr/share/libubox/jshn.sh

JSON_FILE=/tmp/upgrade.json
save_update_status()
{
    json_init
    if [ "x$1" == "x1" ]; then
        json_add_boolean "upgradeStatus" 1
        json_add_string "msg" "success"
    else
        json_add_boolean "upgradeStatus" 0
        json_add_string "msg" "upgrade fail"
    fi

    json_dump > $JSON_FILE
}

swupdate_cmd()
{
    while true
    do
        swu_param=$(fw_printenv -n swu_param 2>/dev/null)
        swu_software=$(fw_printenv -n swu_software 2>/dev/null)
        swu_mode=$(fw_printenv -n swu_mode 2>/dev/null)
        swu_version=$(fw_printenv -n swu_version 2>/dev/null)
        echo "swu_param: ##$swu_param##"
        echo "swu_software: ##$swu_software##"
        echo "swu_mode: ##$swu_mode##"

        check_version_para=""
        [ x"$swu_version" != x"" ] && {
            echo "now version is $swu_version"
            #check_version_para="-N $swu_version"
        }

        [ x"$swu_mode" = x"" ] && {
            echo "no swupdate_cmd to run, wait for next swupdate"
            return
        }

        echo "###now do swupdate###"
        echo "###log in $swupdate_log_file###"

        echo "## swupdate -v $check_version_para $swu_param -e $swu_software,$swu_mode ##"
        swupdate -v $check_version_para $swu_param -e "$swu_software,$swu_mode" >> "$swupdate_log_file" 2>&1

        swu_next=$(fw_printenv -n swu_next 2>/dev/null)
        echo "swu_next: ##$swu_next##"
        if [ x"$swu_next" = "xreboot" ]; then
            fw_setenv swu_next
            save_update_status 1
            sleep 7
            reboot -f
        fi

        sleep 1
    done
}

# ============================================================================
# GLOBAL VARIABLES
# ============================================================================
swupdate_log_file=/mnt/UDISK/swupdate.log

# ============================================================================
# MAIN
# ============================================================================
mkdir -p /var/lock

[ $# -ne 0 ] && {
    echo "config new swupdate"
    rm -f $swupdate_log_file
    swu_input=$*
    echo "swu_input: ##$swu_input##"

    swu_param=$(echo " $swu_input" | sed -E 's/ -e +[^ ]*//')
#    echo "swu_param: ##$swu_param##"
    echo "swu_param $swu_param" > /tmp/swupdate_param_file
    swu_param_e=$(echo " $swu_input" | awk -F ' -e ' '{print $2}')
    swu_param_e=$(echo "$swu_param_e" | awk -F ' ' '{print $1}')
    swu_software=$(echo "$swu_param_e" | awk -F ',' '{print $1}')
#    echo "swu_software: ##$swu_software##"
    echo "swu_software $swu_software" >> /tmp/swupdate_param_file
    swu_mode=$(echo "$swu_param_e" | awk -F ',' '{print $2}')
#    echo "swu_mode: ##$swu_mode##"
    echo "swu_mode $swu_mode" >> /tmp/swupdate_param_file
    fw_setenv -s /tmp/swupdate_param_file
    sync

    echo "## set swupdate_param done ##"
}

swupdate_cmd
```

Une partie des fichiers plus ou moins offusqués en utilisant des bibliothèques (libraries), extension `.so` du répertoire `/usr/share/script`
est utilisée :
```
root@spad-1168:/usr/share/klipper# ls -al ../script/
drwxr-x--x    1 root     root          1024 Jan 14 16:44 .
drwxr-xr-x    1 root     root          1024 Feb 17 14:34 ..
-rwxr-x--x    1 root     root        104656 Apr 26 11:14 mudp.cpython-37.so
-rw-r--r--    1 root     root         18142 Apr 30 10:22 mudp.log
-rwxr-x--x    1 root     root          3497 Apr 26 11:14 recovery.sh
-rwxr-x--x    1 root     root        156216 Apr 26 11:14 system_upgrade.cpython-37.so
-rw-r--r--    1 root     root             0 Jan  1  2020 system_upgrade.log
-rwxr-x--x    1 root     root            38 Apr 26 11:14 system_upgrade.py
-rw-r--r--    1 root     root          2983 Apr 30 10:22 system_upgrade_command.yaml
-rwxr-x--x    1 root     root       6658284 Apr 26 11:14 test.stl
-rwxr-x--x    1 root     root        128704 Apr 26 11:14 watchdog.cpython-37.so
-rw-r--r--    1 root     root         12251 Jan  1  2020 watchdog.log
-rwxr-x--x    1 root     root          7218 Apr 26 11:14 watchdog.py
```

Le fichier `system_upgrade_command.yaml` contient les directives :
```

create_time: '0001-01-01T00:00:00Z'
current_no: V1.0.6.48.57
current_version: 100064857
debug_version_upgrade_change_to_the_official_version: false
download: http://upgrade-pkg-yjy.creality.com/04/whole_machine_202304280829.tar.gz
download_url: http://upgrade-pkg-yjy.creality.com/04/whole_machine_202304280829.tar.gz
hash_key: d9bdd27f72d9a87ab7b28ca88d4a78e5
is_differential_upgrade_packet: 0
is_must_upgrade: 0
is_publish: 0
last_version_no: ''
top_status: 0
type: 0
update_time: '0001-01-01T00:00:00Z'
upgrade_command: false
upgrade_domain: https://c-smart.cxswyjy.com
upgrade_limit: 0
upgrade_new_domain: https://c-smart.cxswyjy.com
version_comment: "1.Add pre-configuration support for Ender-2 pro & CR-M4 & Flsun\
  \ SR/Q5\r\n2.Added laser engraving function for Ender-3 S1 & Ender-3 S1 Pro\r\n\
  3.Add serial port control connection mode for some pre-configured models;\r\n4.Add\
  \ the function of customer service real-time online communication\r\n5.Added support\
  \ for USB peripherals (mouse/keyboard)\r\n6.Modify the net bed points directly on\
  \ the PAD UI\r\n7.The extruder can be directly set as MK8 or Sprite extruder\r\n\
  8.Added model cloning function for online slicing\r\n9.Added 1080P camera resolution\
  \ adjustment function\r\n10.Launched the Creality cloud APP control function, supporting\
  \ 6 models: Ender-3 S1 & Ender-3 V2 & Ender-3 S1 Pro & CR-10 SMART PRO & Ender-6\
  \ & Ender-5 S1\r\n11.Optimize the adaptation logic of vibration compensation, the\
  \ pre-configuration creality model will automatically select the type of vibration\
  \ compensation\r\n12.Optimize configuration file saving logic, automatic leveling\
  \ and other operations can be saved without restarting\r\n13.Optimize the UI interaction\
  \ experience and fix other known bugs"
version_explain: "1.Fixed some known issues\r\n2.Several features have been updated"
version_explain_japanese: null
version_explain_zh: "1. 预置机型增加Ender-2 pro & CR-M4 & Flsun SR/Q5  四款机型\r\n2. 针对Ender-3\
  \ S1 & Ender-3 S1 Pro 两款预置机型增加激光雕刻功能\r\n3. 部分预置机型增加串口控制连接方式\r\n4. 增加客户服务实时在线沟通功能\r\
  \n5. 新增USB外设（鼠标/键盘）支持\r\n6. 在PAD UI上可直接修改网床点数\r\n7. 挤出机可以直接设置为MK8或精灵挤出机\r\n8. 新增在线切片的模型克隆功能\r\
  \n9. 新增1080P摄像头分辨率调节功能\r\n10. 上线创想云APP控制功能，已支持6款机型：Ender-3 S1 & Ender-3 V2  & Ender-3\
  \ S1 Pro &  CR-10 SMART PRO  &  Ender-6 & Ender-5 S1\r\n11. 优化振动补偿的适配逻辑，内置的creality机型会自动选择振动补偿的类型\r\
  \n12. 优化配置文件保存逻辑，自动调平等一系列操作无须重启即可保存\r\n13. 优化UI交互体验，并修复其它已知BUG"
version_id: 202304280829
version_no: V1.0.6.48.57
version_zh: "1.修复了一些已知问题\r\n2.更新了若干功能"
```

Les modifications, ajouts, corrections sont succinctement listées. Elles apparaitront dans la fenêtre sur le SonicPad, pour prendre
connaissance de la totalité, il faudra faire défiler le contenu.
```
1 Add pre-configuration support for Ender-2 pro & CR-M4 & Flsun SR/Q5
2 Added laser engraving function for Ender-3 S1 & Ender-3 S1 Pro
3 Add serial port control connection mode for some pre-configured models;
4 Add the function of customer service real-time online communication\
5 Added support for USB peripherals (mouse/keyboard)
6 Modify the net bed points directly on the PAD UI
7 The extruder can be directly set as MK8 or Sprite extruder
8 Added model cloning function for online slicing
9 Added 1080P camera resolution adjustment function
10 Launched the Creality cloud APP control function, supporting 6 models:
     - Ender-3 S1
     - Ender-3 V2
     - Ender-3 S1 Pro
     - CR-10 SMART PRO
     - Ender-6
     - Ender-5 S1
11 Optimize the adaptation logic of vibration compensation, the pre-configuration creality model will automatically select the type of vibration compensation
12 Optimize configuration file saving logic, automatic leveling and other operations can be saved without restarting
13 Optimize the UI interaction experience and fix other known bugs
```
