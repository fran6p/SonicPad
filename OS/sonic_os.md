# Sonic OS

Creality a mis en ligne les sources de son système d'exploitation.

Ce [dépôt Github](https://github.com/CrealityTech/sonic_pad_os) contient normalement tout le nécessaire pour compiler une version identique de l'OS Creality basé sur Tina Linux, fork d'OpenWRT pour des microcontrôleurs Allwinner.

La première fois que j'ai tenté de récupérer les fichiers de [ce lien](https://klipper.cxswyjy.com/download/sonic_dl/) la connexion n'aboutissait pas (timeout), un échange avec celui gérant le projet via Discord (@luo52) a permis de tout remttre en ordre. Le lien est, à ce jour, 29 mai 2023, fonctionnel. Sans ces sources complémentaires, la compilation se termine par un échec.

En gros, les étapes à suivre pour permettre une compilation :

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
3. h


`test`


