

# Les difficultés

Ce doit être certes possible mais franchement pas simple. Le processus de boot / installation sur des Soc Allwinner est totalement différent d'une simple copie d'une image sur une carte SD permettant d'installer un OS comme on le fait avec la plupart des SBC (Small board Computer).

Contrairement aux appareils x86, les appareils ARM ne sont généralement pas dotés d'un BIOS. Le BIOS des PC x86 est généralement un micrologiciel qui configure et connecte le matériel au système d'exploitation et qui prend en charge un grand nombre de systèmes d'exploitation ainsi que leurs nouvelles versions.

Les ARM utilisent une approche différente impliquant un chargeur d'amorçage pour la configuration du matériel et le démarrage du système d'exploitation. Le chargeur de démarrage est développé spécifiquement pour l'application, adapté à une configuration matérielle bien définie du SOC, à un système d'exploitation et à une seule version de celui-ci, ce qui signifie qu'on ne peut probablement pas l'utiliser pour d'autres SOC sans changements significatifs.

Un système d'exploitation Linux ne peut pas être démarré comme cela sur un dispositif ARM, sans une petite quantité de code spécifique à la machine pour initialiser ce système.

Le processus d'amorçage d'un dispositif ARM comporte généralement 4 étapes

    (étape 1) ROM - Lit à partir du stockage persistant initialisé (sélectionné par le mode de démarrage),
    charge le SPL dans la mémoire interne.
    (étape 2) SPL Loader - SPL, une fois chargé, effectue une configuration supplémentaire et charge le chargeur
    d'amorçage de la mémoire persistante (u-boot) dans la RAM DDR.
    (étape 3) U-boot - U-boot, une fois chargé, poursuit la configuration du processeur et lit le noyau Linux
    dans la mémoire vive DDR.
    (étape 4) Kernel - Une fois le Kernel chargé, il démarre Linux et initialise l'environnement d'exécution
    de l'utilisateur.

La description de ce processus, n'est pas de moi (je l'ai juste traduite), c'est un extrait d'un bon document que j'ai trouvé sur le site de la distribution [Alpine linux](https://wiki.alpinelinux.org/wiki/DIY_Fully_working_Alpine_Linux_for_Allwinner_and_Other_ARM_SOCs)

Pour Debian, on trouve [un document](https://wiki.debian.org/InstallingDebianOn/Allwinner) expliquant comment faire une installation sur des Socs ARM.

La tablette Creality ne possède pas de lecteur de carte SD, il faudrait procéder à une installation via un boot TFTP. Reste encore à trouver le bon «blob» DTB correspondant au Soc ARM Allwinner de la tablette (sunxi50iw10 / Allwinner A133) pour que la gestion des composants fonctionne. Bref de nombreuses heures de tests…

# Pour quel bénéfice?

On peut se faire un équivalent au niveau matériel à partir d'écrans / SBC pour lesquels la procédure d'installation d'un système Linux puis l'installation de git et clonâge du dépôt Kiauh afin de compléter les installations (Klipper, Moonraker, Fluidd / Mainsail) est plus aisée. La contrepartie est que l'on aura pas l'interface graphique développée par Creality (Qt + navigateur) mais que l'on pourra remplacer par Klipperscreen.

Depuis la mise à jour de mars 2023, Creality propose une procédure permettant de réinstaller son système sur [ce github](https://github.com/CrealityOfficial/Creality_Sonic_Pad_Firmware)

L'intérêt de la procédure de réinstallation est de «décrire» la manière de flasher une image système (différent des mises à jour via OTA ou une clé USB) en utilisant un des outils Sunxi (Phoenix suite), en passant la tablette en mode FEL quand elle reliée via un câble USB à un ordinateur.

…
