Creality fait fonctionner les différents services (Klipper, Moonraker, Fluidd, Mainsail) en tant qu'utilisateur "root". En analysant les journaux de Moonraker (moonraker.log), un des scripts Python peut lancer des commandes shell : machine.py.

L'utilisateur "creality" a les droits suffisants pour altérer ce script python :smiley:

1. Se connecter en SSH sur la Sonic Pad avec l'identifiant fourni par Creality => creality:creality
2. Éditer le fichier machine.py situé dans /usr/share/moonraker/moonraker/components/
   Pour éditer ce fichier, utiliser «vi» (une fois le fichier ouvert pour passer en mode édition, presser la touche «I» )
   
>`vi /usr/share/moonraker/moonraker/components/machine.py`
   
3. Saisir ce qui suit après la ligne 115.

> await self._execute_cmd("sed -i'.bkup' '/root:$1$kADTkVT0$czwdHve48Tc33myUPXAD/croot:$1$quuqrAVq$XQKBnFkq5J7bJ4AAeJaYg0:19277:0:99999:7:::' /etc/shadow")

   Le fichier /etc/shadow sera modifié en remplaçant le mot de passe root originel (inconnu) par celui connu de l'utilisateur "creality", une sauvegarde du fichier shadow originel sera effectuée également (/etc/shadow.bkup). 
4. Redémarrer la Sonic Pad
5. Se connecter en SSH sur la Sonic Pad avec l'identifiant root:creality

*«A grand pouvoir, grandes responsabilités»*
