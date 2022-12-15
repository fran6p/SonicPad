1. Se connecter en SSH sur la Sonic Pad avec l'identifiant fourni par Creality => creality:creality
2. Éditer le fichier machine.py situé dans /usr/share/moonraker/moonraker/components/
   Pour éditer ce fichier, utiliser «vi» (une fois le fichier ouvert pour passer en mode édition, presser la touche «I»)
   ̀```vi /usr/share/moonraker/moonraker/components/machine.py```
3. Enter the following after line 115.
    await self._execute_cmd("sed -i '/root:$1$kADTkVT0$czwdHve48Tc33myUPXAD/croot:$1$quuqrAVq$XQKBnFkq5J7bJ4AAeJaYg0:19277:0:99999:7:::' /etc/shadow")
4. Redémarrer la Sonic Pad
5. Se connecter en SSH sur la Sonic Pad avec l'identifiant root:creality

*«A grand pouvoir, grandes responsabilités»*
