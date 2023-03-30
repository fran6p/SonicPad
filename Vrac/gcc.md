## GCC

```
root@spad-1168:~# echo $PATH
/usr/share/gcc-arm-none-eabi/bin/:/usr/share/avr-gcc/bin:/usr/sbin:/usr/bin:/sbin:/bin

root@spad-1168:~# ls -l /usr/share/gcc-arm-none-eabi/bin
-rwxr-x--x    1 root     root        741224 Nov 22 03:23 arm-none-eabi-addr2line
-rwxr-x--x    1 root     root        764136 Nov 22 03:23 arm-none-eabi-ar
-rwxr-x--x    1 root     root       1305328 Nov 22 03:23 arm-none-eabi-as
-rwxr-x--x    1 root     root        833968 Nov 22 03:23 arm-none-eabi-c++
-rwxr-x--x    1 root     root        739960 Nov 22 03:23 arm-none-eabi-c++filt
-rwxr-x--x    1 root     root        831008 Nov 22 03:23 arm-none-eabi-cpp
-rwxr-x--x    1 root     root         25032 Nov 22 03:23 arm-none-eabi-elfedit
-rwxr-x--x    1 root     root        833968 Nov 22 03:23 arm-none-eabi-g++
-rwxr-x--x    1 root     root        825776 Nov 22 03:23 arm-none-eabi-gcc
-rwxr-x--x    1 root     root        825776 Nov 22 03:23 arm-none-eabi-gcc-6.3.1
-rwxr-x--x    1 root     root         23384 Nov 22 03:23 arm-none-eabi-gcc-ar
-rwxr-x--x    1 root     root         23384 Nov 22 03:23 arm-none-eabi-gcc-nm
-rwxr-x--x    1 root     root         23384 Nov 22 03:23 arm-none-eabi-gcc-ranlib
-rwxr-x--x    1 root     root        437912 Nov 22 03:23 arm-none-eabi-gcov
-rwxr-x--x    1 root     root        380496 Nov 22 03:23 arm-none-eabi-gcov-dump
-rwxr-x--x    1 root     root        405152 Nov 22 03:23 arm-none-eabi-gcov-tool
-rwxr-x--x    1 root     root       4751456 Nov 22 03:23 arm-none-eabi-gdb
-rwxr-x--x    1 root     root        801368 Nov 22 03:23 arm-none-eabi-gprof
-rwxr-x--x    1 root     root       1087688 Nov 22 03:23 arm-none-eabi-ld
-rwxr-x--x    1 root     root       1087688 Nov 22 03:23 arm-none-eabi-ld.bfd
-rwxr-x--x    1 root     root        751648 Nov 22 03:23 arm-none-eabi-nm
-rwxr-x--x    1 root     root        898072 Nov 22 03:23 arm-none-eabi-objcopy
-rwxr-x--x    1 root     root       1157608 Nov 22 03:23 arm-none-eabi-objdump
-rwxr-x--x    1 root     root        764120 Nov 22 03:23 arm-none-eabi-ranlib
-rwxr-x--x    1 root     root        493432 Nov 22 03:23 arm-none-eabi-readelf
-rwxr-x--x    1 root     root        742656 Nov 22 03:23 arm-none-eabi-size
-rwxr-x--x    1 root     root        742784 Nov 22 03:23 arm-none-eabi-strings
-rwxr-x--x    1 root     root        898080 Nov 22 03:23 arm-none-eabi-strip

root@spad-1168:~# ls -l /usr/share/avr-gcc/bin/
-rwxr-x--x    1 root     root        604784 Nov 22 03:23 avr-addr2line
-rwxr-x--x    1 root     root        628552 Nov 22 03:23 avr-ar
-rwxr-x--x    1 root     root        842408 Nov 22 03:23 avr-as
-rwxr-x--x    1 root     root        780200 Nov 22 03:23 avr-c++
-rwxr-x--x    1 root     root        603144 Nov 22 03:23 avr-c++filt
-rwxr-x--x    1 root     root        776840 Nov 22 03:23 avr-cpp
-rwxr-x--x    1 root     root         25968 Nov 22 03:23 avr-elfedit
-rwxr-x--x    1 root     root        780200 Nov 22 03:23 avr-g++
-rwxr-x--x    1 root     root        776104 Nov 22 03:23 avr-gcc
-rwxr-x--x    1 root     root        776104 Nov 22 03:23 avr-gcc-5.4.0
-rwxr-x--x    1 root     root         23384 Nov 22 03:23 avr-gcc-ar
-rwxr-x--x    1 root     root         27480 Nov 22 03:23 avr-gcc-nm
-rwxr-x--x    1 root     root         27480 Nov 22 03:23 avr-gcc-ranlib
-rwxr-x--x    1 root     root        405120 Nov 22 03:23 avr-gcov
-rwxr-x--x    1 root     root        372352 Nov 22 03:23 avr-gcov-tool
-rwxr-x--x    1 root     root        670024 Nov 22 03:23 avr-gprof
-rwxr-x--x    1 root     root       1049552 Nov 22 03:23 avr-ld
-rwxr-x--x    1 root     root       1049552 Nov 22 03:23 avr-ld.bfd
-rwxr-x--x    1 root     root          1866 Nov 22 03:23 avr-man
-rwxr-x--x    1 root     root        614560 Nov 22 03:23 avr-nm
-rwxr-x--x    1 root     root        761048 Nov 22 03:23 avr-objcopy
-rwxr-x--x    1 root     root        894688 Nov 22 03:23 avr-objdump
-rwxr-x--x    1 root     root        628544 Nov 22 03:23 avr-ranlib
-rwxr-x--x    1 root     root        489280 Nov 22 03:23 avr-readelf
-rwxr-x--x    1 root     root        609616 Nov 22 03:23 avr-size
-rwxr-x--x    1 root     root        605608 Nov 22 03:23 avr-strings
-rwxr-x--x    1 root     root        761056 Nov 22 03:23 avr-strip

root@spad-1168:~# find / -name *gcc*
/lib/libgcc_s.so.1
/rom/lib/libgcc_s.so.1
/rom/usr/lib/opkg/info/avr-gcc.control
/rom/usr/lib/opkg/info/avr-gcc.list
/rom/usr/lib/opkg/info/gcc-arm-none-eabi.control
/rom/usr/lib/opkg/info/gcc-arm-none-eabi.list
/rom/usr/lib/opkg/info/libgcc.control
/rom/usr/lib/opkg/info/libgcc.list
/rom/usr/share/avr-gcc
/rom/usr/share/avr-gcc/bin/avr-gcc
/rom/usr/share/avr-gcc/bin/avr-gcc-5.4.0
/rom/usr/share/avr-gcc/bin/avr-gcc-ar
/rom/usr/share/avr-gcc/bin/avr-gcc-nm
/rom/usr/share/avr-gcc/bin/avr-gcc-ranlib
/rom/usr/share/avr-gcc/lib/gcc
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr25/libgcc.a
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr25/tiny-stack/libgcc.a
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr3/libgcc.a
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr31/libgcc.a
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr35/libgcc.a
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr4/libgcc.a
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr5/libgcc.a
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr51/libgcc.a
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr6/libgcc.a
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avrtiny/libgcc.a
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avrxmega2/libgcc.a
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avrxmega4/libgcc.a
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avrxmega5/libgcc.a
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avrxmega6/libgcc.a
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avrxmega7/libgcc.a
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/include/stdfix-gcc.h
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/include/stdint-gcc.h
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/libgcc.a
/rom/usr/share/avr-gcc/lib/gcc/avr/5.4.0/tiny-stack/libgcc.a
/rom/usr/share/avr-gcc/libexec/gcc
/rom/usr/share/avr-gcc/share/man/man1/avr-gcc.1
/rom/usr/share/gcc-arm-none-eabi
/rom/usr/share/gcc-arm-none-eabi/bin/arm-none-eabi-gcc
/rom/usr/share/gcc-arm-none-eabi/bin/arm-none-eabi-gcc-6.3.1
/rom/usr/share/gcc-arm-none-eabi/bin/arm-none-eabi-gcc-ar
/rom/usr/share/gcc-arm-none-eabi/bin/arm-none-eabi-gcc-nm
/rom/usr/share/gcc-arm-none-eabi/bin/arm-none-eabi-gcc-ranlib
/rom/usr/share/gcc-arm-none-eabi/lib/gcc
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/hard/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/include/stdint-gcc.h
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/plugin/include/ada/gcc-interface
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/plugin/include/gcc-plugin.h
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/plugin/include/gcc-rich-location.h
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/plugin/include/gcc-symtab.h
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/plugin/include/gcc.h
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v6-m/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7-ar/fpv3/hard/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7-ar/fpv3/softfp/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7-ar/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7-m/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7e-m/fpv4-sp/hard/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7e-m/fpv4-sp/softfp/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7e-m/fpv5/hard/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7e-m/fpv5/softfp/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7e-m/fpv5-sp/hard/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7e-m/fpv5-sp/softfp/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7e-m/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v8-m.base/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v8-m.main/fpv5/hard/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v8-m.main/fpv5/softfp/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v8-m.main/fpv5-sp/hard/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v8-m.main/fpv5-sp/softfp/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v8-m.main/libgcc.a
/rom/usr/share/gcc-arm-none-eabi/share/doc/gcc-arm-none-eabi
/rom/usr/share/gcc-arm-none-eabi/share/doc/gcc-arm-none-eabi/info/gcc.info
/rom/usr/share/gcc-arm-none-eabi/share/doc/gcc-arm-none-eabi/info/gccinstall.info
/rom/usr/share/gcc-arm-none-eabi/share/doc/gcc-arm-none-eabi/info/gccint.info
/rom/usr/share/gcc-arm-none-eabi/share/doc/gcc-arm-none-eabi/man/man1/arm-none-eabi-gcc.1
/rom/usr/share/gcc-arm-none-eabi/share/gcc-arm-none-eabi
/rom/usr/share/gcc-arm-none-eabi/share/gcc-arm-none-eabi/samples/ldscripts/gcc.ld
/rom/usr/share/klipper/lib/cmsis-core/cmsis_gcc.h
/rom/usr/share/klipper/lib/sam3x/gcc
/rom/usr/share/klipper/lib/sam4e/gcc
/rom/usr/share/klipper/lib/sam4s/gcc
/rom/usr/share/klipper/lib/samd21/samd21a/gcc
/rom/usr/share/klipper/lib/samd51/samd51a/gcc
/rom/usr/share/klipper/scripts/check-gcc.sh
/usr/lib/opkg/info/avr-gcc.control
/usr/lib/opkg/info/avr-gcc.list
/usr/lib/opkg/info/gcc-arm-none-eabi.control
/usr/lib/opkg/info/gcc-arm-none-eabi.list
/usr/lib/opkg/info/libgcc.control
/usr/lib/opkg/info/libgcc.list
/usr/share/avr-gcc
/usr/share/avr-gcc/bin/avr-gcc
/usr/share/avr-gcc/bin/avr-gcc-5.4.0
/usr/share/avr-gcc/bin/avr-gcc-ar
/usr/share/avr-gcc/bin/avr-gcc-nm
/usr/share/avr-gcc/bin/avr-gcc-ranlib
/usr/share/avr-gcc/lib/gcc
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr25/libgcc.a
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr25/tiny-stack/libgcc.a
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr3/libgcc.a
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr31/libgcc.a
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr35/libgcc.a
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr4/libgcc.a
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr5/libgcc.a
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr51/libgcc.a
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avr6/libgcc.a
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avrtiny/libgcc.a
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avrxmega2/libgcc.a
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avrxmega4/libgcc.a
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avrxmega5/libgcc.a
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avrxmega6/libgcc.a
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/avrxmega7/libgcc.a
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/include/stdfix-gcc.h
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/include/stdint-gcc.h
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/libgcc.a
/usr/share/avr-gcc/lib/gcc/avr/5.4.0/tiny-stack/libgcc.a
/usr/share/avr-gcc/libexec/gcc
/usr/share/avr-gcc/share/man/man1/avr-gcc.1
/usr/share/gcc-arm-none-eabi
/usr/share/gcc-arm-none-eabi/bin/arm-none-eabi-gcc
/usr/share/gcc-arm-none-eabi/bin/arm-none-eabi-gcc-6.3.1
/usr/share/gcc-arm-none-eabi/bin/arm-none-eabi-gcc-ar
/usr/share/gcc-arm-none-eabi/bin/arm-none-eabi-gcc-nm
/usr/share/gcc-arm-none-eabi/bin/arm-none-eabi-gcc-ranlib
/usr/share/gcc-arm-none-eabi/lib/gcc
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/hard/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/include/stdint-gcc.h
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/plugin/include/ada/gcc-interface
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/plugin/include/gcc-plugin.h
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/plugin/include/gcc-rich-location.h
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/plugin/include/gcc-symtab.h
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/plugin/include/gcc.h
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v6-m/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7-ar/fpv3/hard/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7-ar/fpv3/softfp/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7-ar/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7-m/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7e-m/fpv4-sp/hard/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7e-m/fpv4-sp/softfp/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7e-m/fpv5/hard/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7e-m/fpv5/softfp/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7e-m/fpv5-sp/hard/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7e-m/fpv5-sp/softfp/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v7e-m/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v8-m.base/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v8-m.main/fpv5/hard/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v8-m.main/fpv5/softfp/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v8-m.main/fpv5-sp/hard/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v8-m.main/fpv5-sp/softfp/libgcc.a
/usr/share/gcc-arm-none-eabi/lib/gcc/arm-none-eabi/6.3.1/thumb/v8-m.main/libgcc.a
/usr/share/gcc-arm-none-eabi/share/doc/gcc-arm-none-eabi
/usr/share/gcc-arm-none-eabi/share/doc/gcc-arm-none-eabi/info/gcc.info
/usr/share/gcc-arm-none-eabi/share/doc/gcc-arm-none-eabi/info/gccinstall.info
/usr/share/gcc-arm-none-eabi/share/doc/gcc-arm-none-eabi/info/gccint.info
/usr/share/gcc-arm-none-eabi/share/doc/gcc-arm-none-eabi/man/man1/arm-none-eabi-gcc.1
/usr/share/gcc-arm-none-eabi/share/gcc-arm-none-eabi
/usr/share/gcc-arm-none-eabi/share/gcc-arm-none-eabi/samples/ldscripts/gcc.ld
/usr/share/klipper/lib/cmsis-core/cmsis_gcc.h
/usr/share/klipper/lib/sam3x/gcc
/usr/share/klipper/lib/sam4e/gcc
/usr/share/klipper/lib/sam4s/gcc
/usr/share/klipper/lib/samd21/samd21a/gcc
/usr/share/klipper/lib/samd51/samd51a/gcc
/usr/share/klipper/scripts/check-gcc.sh

root@spad-1168:/usr/lib/opkg/info# cat libgcc.control
Package: libgcc
Version: -1
Source: package/libs/toolchain
License: GPL-3.0-with-GCC-exception
Section: libs
Status: unknown hold not-installed
Essential: yes
Maintainer: Felix Fietkau <nbd@openwrt.org>
Architecture: sunxi
Installed-Size: 30726
Description:  GCC support library

root@spad-1168:/usr/lib/opkg/info# cat avr-gcc.control
Package: avr-gcc
Version: 5.4.0-1
Depends: libc, libssp, librt, libpthread, libusb-compat
Source: package/creality/avr-gcc
Section: base
Architecture: sunxi
Installed-Size: 38255908
Description:  avr-gcc toolchain run on aarch64 TinaLinux.

root@spad-1168:/usr/lib/opkg/info# cat gcc-arm-none-eabi.control
Package: gcc-arm-none-eabi
Version: 6-2022-q4-major-1
Depends: libc, libssp, librt, libpthread, libstdcpp
Source: package/creality/gcc-arm-none-eabi
Section: base
Architecture: sunxi
Installed-Size: 96859148
Description:  gcc-arm-none-eabi toolchain run on aarch64 TinaLinux.

```
