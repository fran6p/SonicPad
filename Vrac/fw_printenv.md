## Boot

```
root@spad-1168:/usr/share/klipper# fw_printenv
bootdelay=0
bootcmd=run setargs_nand boot_normal
earlyprintk=sunxi-uart,0x05000000
initcall_debug=0
console=ttyS0,115200
nand_root=/dev/nandd
mmc_root=/dev/mmcblk0p7
logo_rotate=0
init=/sbin/init
rdinit=/rdinit
loglevel=8
cma=32M
snum=
dn=
mac=
wifi_mac=
bt_mac=
specialstr=
keybox_list=rpmb_key,dm_crypt_key
setargs_nand=setenv bootargs console=${console} root=${nand_root} rootwait init=${init} rdinit=${rdinit} loglevel=${loglevel}  earlyprintk=${earlyprintk} initcall_debug=${initcall_debug}  loglevel=${loglevel} partitions=${partitions} cma=${cma} gpt=1
setargs_mmc=setenv bootargs console=${console} root=${mmc_root} rootwait init=${init} rdinit=${rdinit} loglevel=${loglevel} earlyprintk=${earlyprintk} initcall_debug=${initcall_debug} loglevel=${loglevel} partitions=${partitions} cma=${cma} snum=${snum} dn=${dn} mac_addr=${mac} wifi_mac=${wifi_mac} gpt=1
boot_normal=sunxi_flash read 45000000 ${boot_partition};bootm 45000000
boot_recovery=sunxi_flash read 45000000 recovery;bootm 45000000
boot_fastboot=fastboot
swu_param=-i /mnt/UDISK/.crealityprint/tmp/t800-sonic_lcd-ab_1.0.6.46.25.swu
swu_software=stable
boot_partition=bootA
root_partition=rootfsA
systemAB_next=A
swu_version=1.0.6.46.25
build_type=release
```
