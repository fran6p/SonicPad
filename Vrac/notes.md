## Réflexions diverses

La mise à jour de début novembre permet de préparer un firmware directement sur la tablette sans passer par une machine virtuelle ou plus simple 
pour les Windowsiens par WSL 😉 Le manuel de Creality a d'ailleurs été mis à jour pour en tenir compte (pages 22 et au-delà).

L'accès root n'apporterait pas grand chose aux utilisateurs sans connaissances et pratique de Linux, à part mettre le boxon en cas
de mauvaises manipulations (un rm -rf / est si vite arrivé 😃 ).

Le système d'exploitation n'est pas un dérivé Debian (Ubuntu ou autre) mais est basé sur OpenWRT (Tina linux, version Sunxi Allwinner).

=> pas de /home donc difficile pour l'utilisateur «creality» d'y faire des installations (opkg est d'ailleurs bloqué, les dépôts ne sont pas déclarés 🙁 )

L'interface graphique de cette tablette n'est pas basée sur Klipperscreen mais utilise QT (un peu comme leurs écrans tactiles).

Bref, c'est une jolie tablette pas si mal réalisée que ça d'ailleurs capable de mettre le pied à l'étrier à quelques utilisateurs souhaitant
goûter à Klipper mais pour aller plus loin, ces utilisateurs ne pourront se dispenser de la lecture et relecture des nombreuses documentations.
Ce n'est à peu près «plug and play» que pour les modèles d'imprimantes Creality fournis à condition que l'imprimante soit restée «stock» 
(pas «d'améliorations» matérielles ;-) ).

### Table des partitions

```
root@spad-1168:~# ls -l /dev/by-name/
lrwxrwxrwx    1 root     root            15 Jan  1  2020 UDISK -> /dev/mmcblk0p12
lrwxrwxrwx    1 root     root            14 Jan  1  2020 boot-resource -> /dev/mmcblk0p1
lrwxrwxrwx    1 root     root            14 Jan  1  2020 boot-resource1 -> /dev/mmcblk0p2
lrwxrwxrwx    1 root     root            14 Jan  1  2020 bootA -> /dev/mmcblk0p5
lrwxrwxrwx    1 root     root            14 Jan  1  2020 bootB -> /dev/mmcblk0p6
lrwxrwxrwx    1 root     root            14 Jan  1  2020 env -> /dev/mmcblk0p3
lrwxrwxrwx    1 root     root            14 Jan  1  2020 env-redund -> /dev/mmcblk0p4
lrwxrwxrwx    1 root     root            15 Jan  1  2020 private -> /dev/mmcblk0p10
lrwxrwxrwx    1 root     root            15 Jan  1  2020 pstore -> /dev/mmcblk0p11
lrwxrwxrwx    1 root     root            14 Jan  1  2020 rootfsA -> /dev/mmcblk0p7
lrwxrwxrwx    1 root     root            14 Jan  1  2020 rootfsB -> /dev/mmcblk0p8
lrwxrwxrwx    1 root     root            14 Jan  1  2020 rootfs_data -> /dev/mmcblk0p9
```

### J'ai reçu la tablette SonicPad, et après ?

Vous avez donc acheté une tablette Creality Sonic Pad mais vous ne comprenez pas vraiment ce qu'elle fait ni comment elle vous aide à imprimer plus vite.
Creality a fait un excellent travail de marketing en présentant le SonicPad (SP) comme un appareil «magique» qui rendra instantanément votre imprimante plus
rapide. Sans vous dire, expliquer tout ce que vous devez faire pour obtenir des impressions plus rapides et de qualité.
La première étape, une fois que vous avez installé le SP et que vous l'avez connecté à votre imprimante, est d'essayer d'imprimer un fichier que vous avez
déjà imprimé avec succès. Cela vous permettra de vous assurer que tout fonctionne comme il se doit et que vous n'avez pas de problème qui pourrait vous
retarder.

Ensuite, assurez-vous que vos rétractions sont calibrées, l'extrudeuse également (Esteps => rotation_distance) via la méthode de 100 mm demandés à extruder =
 100 mm extrudés.
 
Une fois cela fait, suivez le guide de Klipper pour le réglage de l'avance à la pression.
Vous commencez à comprendre que le Sonic Pad n'est qu'un erzatz de Raspberry Pi avec un écran tactile, Klipper et sa suite de logiciels associés 
(Moonraker, Fluidd, Mainsail) ayant été préinstallés.
. 
Une fois l'avance à la pression réglée et votre imprimante configurée, vous devrez faire la mise en forme de l'entrée (input shaping) avec l'accéléromètre 
fourni. Cela fonctionne à partir d'une sous-routine que vous pouvez trouver dans l'interface utilisateur du Sonic Pad. 
Suivez les instructions pour mesurer la résonance de votre imprimante. 
Une fois tout ce qui précède fait, vous voudrez déterminer l'accélération maximale de votre machine. 

Tout ceci n'est qu'un survol rapide et basique pour débloquer le potentiel du SonicPad. Il y a plus de réglages et de choses que vous pouvez faire mais 
cela devrait vous permettre de démarrer et d'imprimer déjà plus rapidement.

Pour le reste, le [guide de Ellis](https://ellis3dp.com/Print-Tuning-Guide/) est une lecture à mon avis indispensable pour celui qui veut aller plus loin
dans ses réglages, sans oublier évidemment l'abondante [documentation](https://www.klipper3d.org/Overview.html) de Klipper disponible désormais en plusieurs langues :smirk: 

La lecture de [cette page](https://ellis3dp.com/Print-Tuning-Guide/articles/misconceptions.html) permet d'éclairer quelques idées fausses et mauvais conseils.

:smiley:
https://ellis3dp.com/Print.../articles/misconceptions.html
