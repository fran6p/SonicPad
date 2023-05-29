# Sonic OS

Creality a mis en ligne les sources de son système d'exploitation.

Ce [dépôt Github](https://github.com/CrealityTech/sonic_pad_os) contient normalement tout le nécessaire pour compiler une version identique de l'OS Creality basé sur Tina Linux, fork d'OpenWRT pour des microcontrôleurs Allwinner.

La première fois que j'ai tenté de récupérer les fichiers de [ce lien](https://klipper.cxswyjy.com/download/sonic_dl/) la connexion n'aboutissait pas (timeout), un échange avec celui gérant le projet via Discord (@luo52) a permis de tout remttre en ordre. Le lien est, à ce jour, 29 mai 2023, fonctionnel. Sans ces sources complémentaires, la compilation se termine par un échec.

En gros, les étapes à suivre pour permettre une compilation :

## Prérequis

Il est recommandé d'utiliser Ubuntu18.04 pour compiler le système d'exploitation sonic_pad_os.

Installer les dépendances nécessaires :
```
sudo apt update && sudo apt upgrade
sudo apt install git gcc gawk flex libc6:i386 libstdc++6:i386 lib32z1 libncurses5 libncurses5-dev python g++ libz-dev libssl-dev make p7zip-full
```

## Compiler

1. Télécharger le dépôt : 
    `git clone https://github.com/CrealityTech/sonic_pad_os.git`
2. Téléchargez les paquets complémentaires de sources, les sauvegarder dans un réperoire «dl» (sonic_pad_os/dl) :
    ```
    cd sonic_pad_os
    mkdir dl
    cd dl
    wget -i sonic_pad_os_dl.txt
    ```
    
    Le fichier [sonic_pad_os_dl.txt](https://raw.githubusercontent.com/fran6p/SonicPad/main/Fichiers/sonic_pad_os_dl.txt) contient les quarante et un (41) fichiers sources à télécharger ( ≃ 1,5 Go)
3. Entrer dans le répertoire racine de sonic_pad_os et exécuter le script «prepare.sh» du répertoire «script/»  :
    ```
    cd ~/sonic_pad_os
    ./scripts/prepare.sh
    ```
 4. Etapes de la compilation :
     ```
     source build/envsetup.sh
     lunch    choisir l'option 6
     make -j2 && pack 4.4 swupdate_pack_swu -ab
     ```
5. Une fois la compilation terminée (sans échec évidemment :smirk:) :
    1. Flashage via une clé USB :
    Copiez les fichiers config.ini et t800-sonic_lcd-ab_1.0.6.48.57.swu du répertoire sonic_pad_os/out/r818-sonic_lcd/ dans le répertoire racine de la clé USB.
    Le numéro de version du micrologiciel doit être supérieur au numéro de version actuel de l'appareil, sinon la fenêtre de mise à niveau ne s'affichera pas.
    2. Flashage via mise à jour à partir d'un ordinateur relié en USB au SonicPad et le programme Phoenix Suite :
    Le chemin de l'image générée après la compilation est sonic_pad_os/out/r818-sonic_lcd/t800-sonic_lcd_uart0.img.
    Se référer au lien pour l'outil de flashage et la méthode de mise à jour indiquée [ici](https://github.com/CrealityOfficial/Creality_Sonic_Pad_Firmware)



