# Sonic OS

Creality a mis en ligne les sources de son système d'exploitation.

Ce [dépôt Github](https://github.com/CrealityTech/sonic_pad_os) contient normalement tout le nécessaire pour compiler une version de l'OS identique à celle fournie par Creality basé sur Tina Linux, fork d'OpenWRT pour des microcontrôleurs Allwinner.

La première fois que j'ai tenté de récupérer les fichiers de [ce lien](https://klipper.cxswyjy.com/download/sonic_dl/) la connexion n'aboutissait pas (timeout), un échange avec celui gérant le projet (@luo52) via Discord a permis que tout rentre en ordre. Le lien est, à ce jour, 29 mai 2023, fonctionnel. Sans ces sources complémentaires, la compilation se terminait par un échec (à l'issue du `make -j2`).

En gros, les étapes à suivre pour permettre une compilation :

## Prérequis

Il est recommandé d'utiliser Ubuntu18.04 pour compiler le système d'exploitation sonic_pad_os.

Installer les dépendances nécessaires :
```
sudo apt update && sudo apt upgrade
sudo apt install git gcc gawk flex libc6:i386 libstdc++6:i386 lib32z1 libncurses5 libncurses5-dev python g++ libz-dev libssl-dev make p7zip-full
```
*Avec les versions de Ubuntu (18.04.06, 20.04, 22.04), les paquets `libc6:i386`et `libstdc++6:i386` ne sont pas trouvés* :
    
    ```
    francis@ARRAKIS-DUNE:~$ sudo apt install libc6:i386 libstdc++6:i386
    Reading package lists... Done
    Building dependency tree
    Reading state information... Done
    Package libc6:i386 is not available, but is referred to by another package.
    This may mean that the package is missing, has been obsoleted, or
    is only available from another source
    However the following packages replace it:
      libdb1-compat tzdata
      
    E: Package 'libc6:i386' has no installation candidate
    E: Unable to locate package libstdc++6:i386
    E: Couldn't find any package by regex 'libstdc++6'
    ```

Pour résoudre ce problème d'installation de ces paquets pour une architecture «i386», :
```
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install libc6:i386 libstdc++6:i386
```
Maintenant, ces paquets sont bien installés :
```
francis@ARRAKIS-DUNE:~$ apt-cache policy libc6 libc6:i386
libc6:
  Installed: 2.27-3ubuntu1.6
  Candidate: 2.27-3ubuntu1.6
  Version table:
 *** 2.27-3ubuntu1.6 500
        500 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 Packages
        100 /var/lib/dpkg/status
     2.27-3ubuntu1.5 500
        500 http://security.ubuntu.com/ubuntu bionic-security/main amd64 Packages
     2.27-3ubuntu1 500
        500 http://archive.ubuntu.com/ubuntu bionic/main amd64 Packages
libc6:i386:
  Installed: 2.27-3ubuntu1.6
  Candidate: 2.27-3ubuntu1.6
  Version table:
 *** 2.27-3ubuntu1.6 500
        500 http://archive.ubuntu.com/ubuntu bionic-updates/main i386 Packages
        100 /var/lib/dpkg/status
     2.27-3ubuntu1.5 500
        500 http://security.ubuntu.com/ubuntu bionic-security/main i386 Packages
     2.27-3ubuntu1 500
        500 http://archive.ubuntu.com/ubuntu bionic/main i386 Packages
francis@ARRAKIS-DUNE:~$
```

## Compiler

1. Télécharger le dépôt : 
    `git clone https://github.com/CrealityTech/sonic_pad_os.git`
    ```
    francis@ARRAKIS-DUNE:~$ git clone https://github.com/CrealityTech/sonic_pad_os.git
    Cloning into 'sonic_pad_os'...
    remote: Enumerating objects: 123814, done.
    remote: Counting objects: 100% (70/70), done.
    remote: Compressing objects: 100% (38/38), done.
    remote: Total 123814 (delta 31), reused 49 (delta 25), pack-reused 123744
    Receiving objects: 100% (123814/123814), 1.62 GiB | 11.70 MiB/s, done.
    Resolving deltas: 100% (25556/25556), done.
    Checking out files: 100% (129562/129562), done.
    francis@ARRAKIS-DUNE:~$
    ```
2. Téléchargez les paquets complémentaires de sources, les sauvegarder dans un réperoire «dl» (sonic_pad_os/dl) :
    ```
    cd sonic_pad_os
    mkdir dl
    cd dl
    wget https://raw.githubusercontent.com/fran6p/SonicPad/main/Fichiers/sonic_pad_os_dl.txt
    wget -i sonic_pad_os_dl.txt
    ```
    
<details>
  <summary>(Cliquez pour agrandir!)</summary>
francis@ARRAKIS-DUNE:~/sonic_pad_os/dl$ wget -i ../../sonic_pad_os_dl.txt
--2023-06-07 18:26:06--  https://klipper.cxswyjy.com/download/sonic_dl/Cython-0.29.2.tar.gz
Resolving klipper.cxswyjy.com (klipper.cxswyjy.com)... 184.104.219.54
Connecting to klipper.cxswyjy.com (klipper.cxswyjy.com)|184.104.219.54|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2041500 (1.9M) [application/octet-stream]
Saving to: ‘Cython-0.29.2.tar.gz’

Cython-0.29.2.tar.gz          100%[=================================================>]   1.95M  1.31MB/s    in 1.5s

2023-06-07 18:26:10 (1.31 MB/s) - ‘Cython-0.29.2.tar.gz’ saved [2041500/2041500]

--2023-06-07 18:26:10--  https://klipper.cxswyjy.com/download/sonic_dl/ade-0.1.1d.zip
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 166008 (162K) [application/zip]
Saving to: ‘ade-0.1.1d.zip’

ade-0.1.1d.zip                100%[=================================================>] 162.12K  --.-KB/s    in 0.009s

2023-06-07 18:26:10 (16.7 MB/s) - ‘ade-0.1.1d.zip’ saved [166008/166008]

--2023-06-07 18:26:10--  https://klipper.cxswyjy.com/download/sonic_dl/ai-engine-prebuilt.tar.bz2
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 20715 (20K) [application/octet-stream]
Saving to: ‘ai-engine-prebuilt.tar.bz2’

ai-engine-prebuilt.tar.bz2    100%[=================================================>]  20.23K  --.-KB/s    in 0s

2023-06-07 18:26:11 (58.4 MB/s) - ‘ai-engine-prebuilt.tar.bz2’ saved [20715/20715]

--2023-06-07 18:26:11--  https://klipper.cxswyjy.com/download/sonic_dl/avr-gcc-5.4.0.tar.bz2
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 44812560 (43M) [application/octet-stream]
Saving to: ‘avr-gcc-5.4.0.tar.bz2’

avr-gcc-5.4.0.tar.bz2         100%[=================================================>]  42.74M  2.87MB/s    in 14s

2023-06-07 18:26:25 (3.01 MB/s) - ‘avr-gcc-5.4.0.tar.bz2’ saved [44812560/44812560]

--2023-06-07 18:26:25--  https://klipper.cxswyjy.com/download/sonic_dl/bluez-alsa-20180913.tar.gz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 88557 (86K) [application/octet-stream]
Saving to: ‘bluez-alsa-20180913.tar.gz’

bluez-alsa-20180913.tar.gz    100%[=================================================>]  86.48K  --.-KB/s    in 0.005s

2023-06-07 18:26:25 (18.1 MB/s) - ‘bluez-alsa-20180913.tar.gz’ saved [88557/88557]

--2023-06-07 18:26:25--  https://klipper.cxswyjy.com/download/sonic_dl/fluidd-1.0.14-prebuilt.tar.bz2
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 7451115 (7.1M) [application/octet-stream]
Saving to: ‘fluidd-1.0.14-prebuilt.tar.bz2’

fluidd-1.0.14-prebuilt.tar.bz 100%[=================================================>]   7.11M  2.65MB/s    in 2.7s

2023-06-07 18:26:28 (2.65 MB/s) - ‘fluidd-1.0.14-prebuilt.tar.bz2’ saved [7451115/7451115]

--2023-06-07 18:26:28--  https://klipper.cxswyjy.com/download/sonic_dl/fluidd-pad-1.2.56-prebuilt.tar.bz2
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 14986760 (14M) [application/octet-stream]
Saving to: ‘fluidd-pad-1.2.56-prebuilt.tar.bz2’

fluidd-pad-1.2.56-prebuilt.ta 100%[=================================================>]  14.29M  2.56MB/s    in 5.6s

2023-06-07 18:26:34 (2.56 MB/s) - ‘fluidd-pad-1.2.56-prebuilt.tar.bz2’ saved [14986760/14986760]

--2023-06-07 18:26:34--  https://klipper.cxswyjy.com/download/sonic_dl/gcc-arm-none-eabi-6-2022-q4-major-linux.tar.bz2
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 77713347 (74M) [application/octet-stream]
Saving to: ‘gcc-arm-none-eabi-6-2022-q4-major-linux.tar.bz2’

gcc-arm-none-eabi-6-2022-q4-m 100%[=================================================>]  74.11M  3.57MB/s    in 26s

2023-06-07 18:27:01 (2.83 MB/s) - ‘gcc-arm-none-eabi-6-2022-q4-major-linux.tar.bz2’ saved [77713347/77713347]

--2023-06-07 18:27:01--  https://klipper.cxswyjy.com/download/sonic_dl/glib-2.50.1.tar.xz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 7521832 (7.2M) [application/octet-stream]
Saving to: ‘glib-2.50.1.tar.xz’

glib-2.50.1.tar.xz            100%[=================================================>]   7.17M  2.40MB/s    in 3.0s

2023-06-07 18:27:04 (2.40 MB/s) - ‘glib-2.50.1.tar.xz’ saved [7521832/7521832]

--2023-06-07 18:27:04--  https://klipper.cxswyjy.com/download/sonic_dl/harfbuzz-1.7.4.tar.bz2
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 1718260 (1.6M) [application/octet-stream]
Saving to: ‘harfbuzz-1.7.4.tar.bz2’

harfbuzz-1.7.4.tar.bz2        100%[=================================================>]   1.64M  2.22MB/s    in 0.7s

2023-06-07 18:27:05 (2.22 MB/s) - ‘harfbuzz-1.7.4.tar.bz2’ saved [1718260/1718260]

--2023-06-07 18:27:05--  https://klipper.cxswyjy.com/download/sonic_dl/hostapd-2017-11-08.tar.bz2
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 2874867 (2.7M) [application/octet-stream]
Saving to: ‘hostapd-2017-11-08.tar.bz2’

hostapd-2017-11-08.tar.bz2    100%[=================================================>]   2.74M  2.27MB/s    in 1.2s

2023-06-07 18:27:07 (2.27 MB/s) - ‘hostapd-2017-11-08.tar.bz2’ saved [2874867/2874867]

--2023-06-07 18:27:07--  https://klipper.cxswyjy.com/download/sonic_dl/icu4c-55_1-src.tgz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 25600847 (24M) [application/octet-stream]
Saving to: ‘icu4c-55_1-src.tgz’

icu4c-55_1-src.tgz            100%[=================================================>]  24.41M  2.55MB/s    in 10s

2023-06-07 18:27:17 (2.39 MB/s) - ‘icu4c-55_1-src.tgz’ saved [25600847/25600847]

--2023-06-07 18:27:17--  https://klipper.cxswyjy.com/download/sonic_dl/iozone3_489.tgz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 830369 (811K) [application/octet-stream]
Saving to: ‘iozone3_489.tgz’

iozone3_489.tgz               100%[=================================================>] 810.91K  3.03MB/s    in 0.3s

2023-06-07 18:27:18 (3.03 MB/s) - ‘iozone3_489.tgz’ saved [830369/830369]

--2023-06-07 18:27:18--  https://klipper.cxswyjy.com/download/sonic_dl/klipper-0.1.98.tar.gz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 49105915 (47M) [application/octet-stream]
Saving to: ‘klipper-0.1.98.tar.gz’

klipper-0.1.98.tar.gz         100%[=================================================>]  46.83M  1.25MB/s    in 27s

2023-06-07 18:27:45 (1.74 MB/s) - ‘klipper-0.1.98.tar.gz’ saved [49105915/49105915]

--2023-06-07 18:27:45--  https://klipper.cxswyjy.com/download/sonic_dl/klipper-brain-1.9.1-prebuilt.tar.bz2
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 29376166 (28M) [application/octet-stream]
Saving to: ‘klipper-brain-1.9.1-prebuilt.tar.bz2’

klipper-brain-1.9.1-prebuilt. 100%[=================================================>]  28.01M  1.78MB/s    in 19s

2023-06-07 18:28:05 (1.46 MB/s) - ‘klipper-brain-1.9.1-prebuilt.tar.bz2’ saved [29376166/29376166]

--2023-06-07 18:28:05--  https://klipper.cxswyjy.com/download/sonic_dl/libinput-1.5.0.tar.xz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 910476 (889K) [application/octet-stream]
Saving to: ‘libinput-1.5.0.tar.xz’

libinput-1.5.0.tar.xz         100%[=================================================>] 889.14K  1.74MB/s    in 0.5s

2023-06-07 18:28:06 (1.74 MB/s) - ‘libinput-1.5.0.tar.xz’ saved [910476/910476]

--2023-06-07 18:28:06--  https://klipper.cxswyjy.com/download/sonic_dl/librsync-2.3.1.tar.gz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 194573 (190K) [application/octet-stream]
Saving to: ‘librsync-2.3.1.tar.gz’

librsync-2.3.1.tar.gz         100%[=================================================>] 190.01K  --.-KB/s    in 0.01s

2023-06-07 18:28:06 (16.7 MB/s) - ‘librsync-2.3.1.tar.gz’ saved [194573/194573]

--2023-06-07 18:28:06--  https://klipper.cxswyjy.com/download/sonic_dl/libump-ec0680628744f30b8fac35e41a7bd8e23e59c39f.tar.gz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 25572 (25K) [application/octet-stream]
Saving to: ‘libump-ec0680628744f30b8fac35e41a7bd8e23e59c39f.tar.gz’

libump-ec0680628744f30b8fac35 100%[=================================================>]  24.97K  --.-KB/s    in 0.001s

2023-06-07 18:28:06 (45.8 MB/s) - ‘libump-ec0680628744f30b8fac35e41a7bd8e23e59c39f.tar.gz’ saved [25572/25572]

--2023-06-07 18:28:06--  https://klipper.cxswyjy.com/download/sonic_dl/libvpx-1.6.0.tar.bz2
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 1943026 (1.9M) [application/octet-stream]
Saving to: ‘libvpx-1.6.0.tar.bz2’

libvpx-1.6.0.tar.bz2          100%[=================================================>]   1.85M  2.45MB/s    in 0.8s

2023-06-07 18:28:07 (2.45 MB/s) - ‘libvpx-1.6.0.tar.bz2’ saved [1943026/1943026]

--2023-06-07 18:28:07--  https://klipper.cxswyjy.com/download/sonic_dl/libwebp-0.4.3.tar.gz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 990904 (968K) [application/octet-stream]
Saving to: ‘libwebp-0.4.3.tar.gz’

libwebp-0.4.3.tar.gz          100%[=================================================>] 967.68K  1.88MB/s    in 0.5s

2023-06-07 18:28:08 (1.88 MB/s) - ‘libwebp-0.4.3.tar.gz’ saved [990904/990904]

--2023-06-07 18:28:08--  https://klipper.cxswyjy.com/download/sonic_dl/live.2019.02.27.tar.gz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 635377 (620K) [application/octet-stream]
Saving to: ‘live.2019.02.27.tar.gz’

live.2019.02.27.tar.gz        100%[=================================================>] 620.49K  2.45MB/s    in 0.2s

2023-06-07 18:28:09 (2.45 MB/s) - ‘live.2019.02.27.tar.gz’ saved [635377/635377]

--2023-06-07 18:28:09--  https://klipper.cxswyjy.com/download/sonic_dl/lz4-1.9.2.tar.gz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 305796 (299K) [application/octet-stream]
Saving to: ‘lz4-1.9.2.tar.gz’

lz4-1.9.2.tar.gz              100%[=================================================>] 298.63K  --.-KB/s    in 0.02s

2023-06-07 18:28:09 (15.7 MB/s) - ‘lz4-1.9.2.tar.gz’ saved [305796/305796]

--2023-06-07 18:28:09--  https://klipper.cxswyjy.com/download/sonic_dl/mainsail-1.0.5_mainsail.tar.gz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 5505794 (5.2M) [application/octet-stream]
Saving to: ‘mainsail-1.0.5_mainsail.tar.gz’

mainsail-1.0.5_mainsail.tar.g 100%[=================================================>]   5.25M  2.14MB/s    in 2.5s

2023-06-07 18:28:12 (2.14 MB/s) - ‘mainsail-1.0.5_mainsail.tar.gz’ saved [5505794/5505794]

--2023-06-07 18:28:12--  https://klipper.cxswyjy.com/download/sonic_dl/moonraker-0.0.71.tar.gz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 1975616 (1.9M) [application/octet-stream]
Saving to: ‘moonraker-0.0.71.tar.gz’

moonraker-0.0.71.tar.gz       100%[=================================================>]   1.88M  1.56MB/s    in 1.2s

2023-06-07 18:28:13 (1.56 MB/s) - ‘moonraker-0.0.71.tar.gz’ saved [1975616/1975616]

--2023-06-07 18:28:13--  https://klipper.cxswyjy.com/download/sonic_dl/ncnn-yolov5-prebuilt.tar.bz2
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 9552468 (9.1M) [application/octet-stream]
Saving to: ‘ncnn-yolov5-prebuilt.tar.bz2’

ncnn-yolov5-prebuilt.tar.bz2  100%[=================================================>]   9.11M  1.72MB/s    in 5.3s

2023-06-07 18:28:19 (1.71 MB/s) - ‘ncnn-yolov5-prebuilt.tar.bz2’ saved [9552468/9552468]

--2023-06-07 18:28:19--  https://klipper.cxswyjy.com/download/sonic_dl/netifaces-0.10.9.tar.gz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 28844 (28K) [application/octet-stream]
Saving to: ‘netifaces-0.10.9.tar.gz’

netifaces-0.10.9.tar.gz       100%[=================================================>]  28.17K  --.-KB/s    in 0.001s

2023-06-07 18:28:19 (35.5 MB/s) - ‘netifaces-0.10.9.tar.gz’ saved [28844/28844]

--2023-06-07 18:28:19--  https://klipper.cxswyjy.com/download/sonic_dl/nghttp2-1.24.0.tar.bz2
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 1800637 (1.7M) [application/octet-stream]
Saving to: ‘nghttp2-1.24.0.tar.bz2’

nghttp2-1.24.0.tar.bz2        100%[=================================================>]   1.72M  1.23MB/s    in 1.4s

2023-06-07 18:28:21 (1.23 MB/s) - ‘nghttp2-1.24.0.tar.bz2’ saved [1800637/1800637]

--2023-06-07 18:28:21--  https://klipper.cxswyjy.com/download/sonic_dl/opencv-4.1.0.zip
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 91806599 (88M) [application/zip]
Saving to: ‘opencv-4.1.0.zip’

opencv-4.1.0.zip              100%[=================================================>]  87.55M  3.90MB/s    in 30s

2023-06-07 18:28:52 (2.88 MB/s) - ‘opencv-4.1.0.zip’ saved [91806599/91806599]

--2023-06-07 18:28:52--  https://klipper.cxswyjy.com/download/sonic_dl/opencv_contrib-4.1.0.zip
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 61741612 (59M) [application/zip]
Saving to: ‘opencv_contrib-4.1.0.zip’

opencv_contrib-4.1.0.zip      100%[=================================================>]  58.88M  3.46MB/s    in 18s

2023-06-07 18:29:10 (3.36 MB/s) - ‘opencv_contrib-4.1.0.zip’ saved [61741612/61741612]

--2023-06-07 18:29:10--  https://klipper.cxswyjy.com/download/sonic_dl/pyparsing-pyparsing_2.3.0.zip
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 661068 (646K) [application/zip]
Saving to: ‘pyparsing-pyparsing_2.3.0.zip’

pyparsing-pyparsing_2.3.0.zip 100%[=================================================>] 645.57K  --.-KB/s    in 0.04s

2023-06-07 18:29:10 (14.9 MB/s) - ‘pyparsing-pyparsing_2.3.0.zip’ saved [661068/661068]

--2023-06-07 18:29:10--  https://klipper.cxswyjy.com/download/sonic_dl/python-dateutil-2.7.5.tar.gz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 316043 (309K) [application/octet-stream]
Saving to: ‘python-dateutil-2.7.5.tar.gz’

python-dateutil-2.7.5.tar.gz  100%[=================================================>] 308.64K  --.-KB/s    in 0.02s

2023-06-07 18:29:11 (15.9 MB/s) - ‘python-dateutil-2.7.5.tar.gz’ saved [316043/316043]

--2023-06-07 18:29:11--  https://klipper.cxswyjy.com/download/sonic_dl/pytz-2018.9.tar.gz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 310705 (303K) [application/octet-stream]
Saving to: ‘pytz-2018.9.tar.gz’

pytz-2018.9.tar.gz            100%[=================================================>] 303.42K  --.-KB/s    in 0.02s

2023-06-07 18:29:11 (16.0 MB/s) - ‘pytz-2018.9.tar.gz’ saved [310705/310705]

--2023-06-07 18:29:11--  https://klipper.cxswyjy.com/download/sonic_dl/qt-browser2-prebuilt.tar.bz2
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 113090 (110K) [application/octet-stream]
Saving to: ‘qt-browser2-prebuilt.tar.bz2’

qt-browser2-prebuilt.tar.bz2  100%[=================================================>] 110.44K  --.-KB/s    in 0.006s

2023-06-07 18:29:11 (17.5 MB/s) - ‘qt-browser2-prebuilt.tar.bz2’ saved [113090/113090]

--2023-06-07 18:29:11--  https://klipper.cxswyjy.com/download/sonic_dl/qt-everywhere-opensource-src-5.12.9-prebuilt_glibc_64bit.tar.gz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 497329799 (474M) [application/octet-stream]
Saving to: ‘qt-everywhere-opensource-src-5.12.9-prebuilt_glibc_64bit.tar.gz’

qt-everywhere-opensource-src- 100%[=================================================>] 474.29M  5.67MB/s    in 2m 1s

2023-06-07 18:31:12 (3.93 MB/s) - ‘qt-everywhere-opensource-src-5.12.9-prebuilt_glibc_64bit.tar.gz’ saved [497329799/497329799]

--2023-06-07 18:31:12--  https://klipper.cxswyjy.com/download/sonic_dl/qt-everywhere-opensource-src-5.12.9.tar.xz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 511048548 (487M) [application/octet-stream]
Saving to: ‘qt-everywhere-opensource-src-5.12.9.tar.xz’

qt-everywhere-opensource-src- 100%[=================================================>] 487.37M  2.92MB/s    in 1m 53s

2023-06-07 18:33:06 (4.30 MB/s) - ‘qt-everywhere-opensource-src-5.12.9.tar.xz’ saved [511048548/511048548]

--2023-06-07 18:33:06--  https://klipper.cxswyjy.com/download/sonic_dl/rtsp_demo-prebuilt.tar.bz2
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 58694 (57K) [application/octet-stream]
Saving to: ‘rtsp_demo-prebuilt.tar.bz2’

rtsp_demo-prebuilt.tar.bz2    100%[=================================================>]  57.32K  --.-KB/s    in 0.003s

2023-06-07 18:33:06 (19.0 MB/s) - ‘rtsp_demo-prebuilt.tar.bz2’ saved [58694/58694]

--2023-06-07 18:33:06--  https://klipper.cxswyjy.com/download/sonic_dl/setuptools-41.4.0.zip
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 855608 (836K) [application/zip]
Saving to: ‘setuptools-41.4.0.zip’

setuptools-41.4.0.zip         100%[=================================================>] 835.55K  1.14MB/s    in 0.7s

2023-06-07 18:33:07 (1.14 MB/s) - ‘setuptools-41.4.0.zip’ saved [855608/855608]

--2023-06-07 18:33:07--  https://klipper.cxswyjy.com/download/sonic_dl/six-1.12.0.tar.gz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 32725 (32K) [application/octet-stream]
Saving to: ‘six-1.12.0.tar.gz’

six-1.12.0.tar.gz             100%[=================================================>]  31.96K  --.-KB/s    in 0.001s

2023-06-07 18:33:07 (29.5 MB/s) - ‘six-1.12.0.tar.gz’ saved [32725/32725]

--2023-06-07 18:33:07--  https://klipper.cxswyjy.com/download/sonic_dl/swupdate-2019.11.tar.xz
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 5584324 (5.3M) [application/octet-stream]
Saving to: ‘swupdate-2019.11.tar.xz’

swupdate-2019.11.tar.xz       100%[=================================================>]   5.33M  2.64MB/s    in 2.0s

2023-06-07 18:33:09 (2.64 MB/s) - ‘swupdate-2019.11.tar.xz’ saved [5584324/5584324]

--2023-06-07 18:33:09--  https://klipper.cxswyjy.com/download/sonic_dl/tslib-1.15.tar.bz2
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 395481 (386K) [application/octet-stream]
Saving to: ‘tslib-1.15.tar.bz2’

tslib-1.15.tar.bz2            100%[=================================================>] 386.21K  --.-KB/s    in 0.02s

2023-06-07 18:33:10 (15.2 MB/s) - ‘tslib-1.15.tar.bz2’ saved [395481/395481]

--2023-06-07 18:33:10--  https://klipper.cxswyjy.com/download/sonic_dl/x264.zip
Reusing existing connection to klipper.cxswyjy.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 1083607 (1.0M) [application/zip]
Saving to: ‘x264.zip’

x264.zip                      100%[=================================================>]   1.03M  3.72MB/s    in 0.3s

2023-06-07 18:33:10 (3.72 MB/s) - ‘x264.zip’ saved [1083607/1083607]

FINISHED --2023-06-07 18:33:10--
Total wall clock time: 7m 4s
Downloaded: 41 files, 1.4G in 6m 49s (3.40 MB/s)
francis@ARRAKIS-DUNE:~/sonic_pad_os/dl$    
</details>

*Le fichier [sonic_pad_os_dl.txt](https://raw.githubusercontent.com/fran6p/SonicPad/main/Fichiers/sonic_pad_os_dl.txt) contient la liste des quarante et un (41) fichiers sources à télécharger ( ≃ 1,5 Go)* :smiley:
    
Le contenu du répertoire sonic_pad_os/dl :
    
![listing](https://github.com/fran6p/SonicPad/blob/main/Images/sonic_os_dl-listing.jpg)
    
3. Entrer dans le répertoire racine de sonic_pad_os et exécuter le script «prepare.sh» du répertoire «script/»  :
    ```
    cd ~/sonic_pad_os
    ./scripts/prepare.sh
    ```
 4. Etapes de la compilation ( 4 étapes) :
     ```
     source build/envsetup.sh
     lunch 6   ; ou lunch puis choix de l'option 6 dans la liste d'options proposées,  
     make -j2 && pack
     swupdate_pack_swu -ab
     ```
5. Une fois la compilation terminée (sans échec évidemment :smirk:) :
    - Flashage via une clé USB :
    
    Copiez les fichiers **config.ini** et **t800-sonic_lcd-ab_1.0.6.48.57.swu** du répertoire ***sonic_pad_os/out/r818-sonic_lcd/*** dans le répertoire racine de la clé USB.
    
    **Le numéro de version du micrologiciel doit être supérieur au numéro de version actuel de l'appareil**, sinon la fenêtre de mise à niveau ne s'affichera pas.
    
    - Flashage via mise à jour à partir d'un ordinateur relié en USB au SonicPad et le programme Phoenix Suite :
    
    Le chemin de l'image générée après la compilation est **sonic_pad_os/out/r818-sonic_lcd/t800-sonic_lcd_uart0.img**.
    
    Se référer au lien pour l'outil de flashage et la méthode de mise à jour indiquée **[ici](https://github.com/CrealityOfficial/Creality_Sonic_Pad_Firmware)**

## NOTES

### Concernant la compilation

Le `make -j2` bien que «rapide» à compiler, lors d'une erreur, ce n'est pas du tout explicite quant à cette erreur 🙃. Il faut
relancer la compilation via `make -j1` ou simplement  `make`, le processus est alors beaucoup, beaucoup plus lent mais en cas d'erreur,
cette fois-ci, l'erreur est clairement décrite.

