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
2. Téléchargez les paquets complémentaires de sources, les sauvegarder dans un réperoire «dl» (sonic_pad_os/dl) :
    ```
    cd sonic_pad_os
    mkdir dl
    cd dl
    wget https://raw.githubusercontent.com/fran6p/SonicPad/main/Fichiers/sonic_pad_os_dl.txt
    wget -i sonic_pad_os_dl.txt
    ```
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
     lunch    ; choisir dans la liste d'options, l'option 6 
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
