Est-il possible d'outrepasser les étapes de première instalallation de la tablette ?

Cela permettrait à un utilisateur plus aguerri de copier les fichiers de configurations de son imprimante dans le dossier `printer_config` du répertoire
prévu pour le stockage permanent `/mnt/UDISK` sans devoir passer par les étapes de choix de l'imprimante, flashage du firmware, tests de vérifications matériels.

A la racine du répertoire `/mnt/UDISK` on trouve un fichier `QtStorage.json`. Son contenu possède peut-être un indice ?
```
{
    "isFirstBoot": false,
    "language": "en"
}
```

La "variable" booléenne `isFirst-Boot` ne passe à `false` qu'une fois les étapes de première installation faites. Au premier allumage après déballage du SonicPad, elle est positionnée à `true`

:smiley:
