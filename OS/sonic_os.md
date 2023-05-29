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
*Avec une version autre de Ubuntu (20.04, 22.04), les paquets `libc6:i386`et `libstdc++6:i386` ne sont pas trouvés*

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
    *Le fichier [sonic_pad_os_dl.txt](https://raw.githubusercontent.com/fran6p/SonicPad/main/Fichiers/sonic_pad_os_dl.txt) contient la liste des quarante et un (41) fichiers sources à télécharger ( ≃ 1,5 Go)*
    
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
