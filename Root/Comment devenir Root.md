Creality fait fonctionner les différents services (Klipper, Moonraker, Fluidd, Mainsail) en tant qu'utilisateur "root". En analysant les journaux de Moonraker (moonraker.log), un des scripts Python peut lancer des commandes shell avec une élévation des droits (=root) : machine.py.

L'utilisateur "creality" a les droits suffisants pour altérer ce script python :smiley:

1. Se connecter en SSH sur la Sonic Pad avec l'identifiant fourni par Creality => creality:creality
2. Éditer le fichier machine.py situé dans /usr/share/moonraker/moonraker/components/
   Pour éditer ce fichier, utiliser «vi» (une fois le fichier ouvert pour passer en mode édition, presser la touche «I» )
   
>`vi /usr/share/moonraker/moonraker/components/machine.py`
   
3. Saisir ce qui suit après la ligne 115.

`await self._execute_cmd("sed -i'.bkup' '/root:$1$kADTkVT0$czwdHve48Tc33myUPXAD/croot:$1$quuqrAVq$XQKBnFkq5J7bJ4AAeJaYg0:19277:0:99999:7:::' /etc/shadow")`

ou (alternative) :

`await self._execute_cmd("sed -i'.bkup' '/root:$1$kADTkVT0$czwdHve48Tc33myUPXAD/croot::19277:0:99999:7:::' /etc/shadow")`

Au cas où le mot de passe root originel ait été modifié (pas besoin de connaitre le hash du mdp :smiley:) :

`await self._execute_cmd("sed -i'.bkup' '/^root/croot::19277:0:99999:7:::' /etc/shadow")`

> Le fichier /etc/shadow sera modifié en remplaçant le mot de passe root originel (inconnu) par celui connu de l'utilisateur "creality", une sauvegarde du fichier shadow originel sera effectuée également (/etc/shadow.bkup). La seconde ligne est une alternative où le mot de passe de root est vide :smiley: il suffira d'utiliser la commande "passwd" pour mettre celui que l'on veut.

   Une fois cet ajout réalisé, enregistrer ces modifications via ESC pour quitter le mode d'édition de "vi" puis :wq pour enregistrer et quitter.
   
4. Redémarrer la Sonic Pad
5. Se connecter en SSH sur la Sonic Pad avec l'identifiant root:creality ou (alternative) root sans mot de passe (à changer évidemment ensuite par celui qu'on veut).

Lors d'une mise à jour du système, le dossier /rom contient les modifications apportées par le fabricant (Creality) mais les modifications faites par l'utilisateur sont préservées ( voir le dossier /overlay/upper et ses sous-dossiers ).

La mise à jour de janvier 2023, contient un nouveau mot de passe root :smirk:
> root:$1$02kcMFtq$d0Lrz8gZkGY1krlZKolWM.:19361:0:99999:7:::

*«A grand pouvoir, grandes responsabilités»*
