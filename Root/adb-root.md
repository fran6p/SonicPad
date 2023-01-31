Nécessite qu'adb soit installé sur le PC

SonicPad connecté en USB sur le PC

Lancer la commande suivante pour voir si le SP s'affiche (est reconnu) :

`adb devices`

Ensuite: 
```
adb pull /etc/shadow
# Faire une sauvegarde du fichier originel
cp shadow shadow_bkup
vi shadow
# Remplacer le mot de passe root (la liste de caractères après le root: jusqu'au : à la fin de  cette liste)
ex: root:$1ß…Ygo:19277:0:99999:7::: par
root::19277:0:99999:7:::
# Enregistrer cette modification ( ESC puis :wq ) puis pousser celle-ci sur le Pad
adb push shadow /etc/
# Tester le login root (vide) en lançant le shell adb
adb shell

```

Cette méthode modifie le mot de passe root de manière permanente, comme il est «vide», il est tout de même conseillé
 d'en mettre un à votre convenance (passwd).

Une sauvegarde ayant été réalisée, on peut toujours rmettre en l'état au cas où (mise à jour, etc.). Toutefois, j'ai déjà
 eu plusieurs mises à jour avec cette modification présente sans que le mot de passe root ne soit remodifié :smirk:
