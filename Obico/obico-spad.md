# Obico et Klipper (Creality Sonic Pad)
J'ai réussi à faire fonctionner Obico sur le Sonic Pad. Vous trouverez ci-dessous les instructions pour le faire fonctionner. Vous devrez obtenir un compte root pour effectuer ces étapes. Soyez prudent, car chaque fois que vous utilisez le root, il y a une chance que vous puissiez bloquer l'appareil, si vous ne pouvez pas le démarrer. Creality n'a pas fourni de processus pour flasher complètement l'appareil.


**Ce guide a été écrit pour les personnes ayant une expérience préalable de Linux. Ne suivez ce guide que si vous êtes à l'aise avec l'édition de fichiers, la navigation dans les systèmes de fichiers linux, et si vous pouvez faire votre propre dépannage. Cela a fonctionné pour moi, mais je ne peux pas garantir que cela fonctionnera pour vous. Veuillez vous assurer que vous êtes sur le même firmware avant de l'exécuter. Ce guide suppose que vous avez une imprimante. Je ne l'ai pas testé sur un Pad connecté à plusieurs imprimantes. J'utilise une Ender 5 S1, donc je ne peux pas garantir que cela fonctionnera pour vous et d'autres imprimantes.**


>Si vous rencontrez des difficultés, vous pouvez restaurer le dispositif en exécutant la commande suivante :
>>`/usr/share/script/recovery.sh all`

Ce guide a été configuré pour le firmware du Sonic Pad "V1.0.6.35.154 02 Dec. 2022".

MAJ 29/01/2023:
- Obico fonctionne encore avec la mise à jour de janvier proposée par Creality (juste avant le nouvel an chinois :smirk:)
- La section concernant Procd (5) a été modifiée pour fonctionner avec cron. Pas besoin de délai pour démarrer Obico après Moonraker 

A un moment donné, je chercherai à automatiser ceci dans un script. Tout ceci est codé en dur pour le moment.
Voici les étapes de haut niveau :

1. SSH et obtenir root
2. Télécharger et configurer Obico
3. Installer les dépendances
4. Lier l'imprimante
5. ~~Créer un service~~ Exécuter au démarrage

C'est parti !

## 1. SSH et passer root

  ### 1.1 Se connecter sur le Sonic Pad
  
  ```ssh creality@<adresse-ip/nom-hôte>```
  >Mot de passe: creality
  
  ### 1.2 Passer root
  crédit à [smwoodward](https://github.com/smwoodward/Sonic-Pad-Updates/blob/main/root_access/Root) pour la méthode d'accès

  Editer le script Python machine.py de Moonraker pour remplacer le mot de passe root que Creality n'a pas fourni par le même que celui de l'utilisateur creality (connu).
  
  `vi /usr/share/moonraker/moonraker/components/machine.py`

  Après la ligne 115, entrez ce qui suit (ligne 116 donc), presser la touche "I" pour passer vi en mode édition :
  
  ```await self._execute_cmd("sed -i '/root:$1$kADTkVT0$czwdHve48Tc33myUPXAD/croot:$1$quuqrAVq$XQKBnFkq5J7bJ4AAeJaYg0:19277:0:99999:7:::' /etc/shadow")```

  Enregistrer cette modification ( ESC, :wq ).
  
 ### 1.3 Redémarer le Sonic Pad

  ### 1.4 SSH en utilisateur root
  
  ```ssh root@<adresse-ip/nom-hôte>```
  >Mot de passe: creality

## 2. Télécharger et configurer Obico

  ### 2.1 Téléchargez le client Obico sur github.
  ```
  cd /usr/share
  git clone https://github.com/TheSpaghettiDetective/moonraker-obico.git
  ```

  ### 2.2 Créer moonraker-obico.cfg
  ```
  cp moonraker-obico.cfg.sample moonraker-obico.cfg
  ```
  >Editer la configuration que vous avez copiée et modifier le chemin du journal en : `/mnt/UDISK/printer_logs/moonraker-obico.log`
  
  >Si vous utilisez une version locale d'obico, modifiez l'url du serveur.



## 3. Installer les dépendances

  ### 3.1 Configuration de l'environnement virtuel de python pour éviter les collisions de versions
  ```
     pip3 install virtualenv
     cd /usr/share/moonraker-obico
     virtualenv env
     source env/bin/activate
  ```
  
  Vérifiez que vous êtes dans l'environnement virtuel.
  
  ```which python3```
          
  >La sortie devrait être /usr/share/moonraker-obico/env/bin/python3.


  ### 3.2 Installer les modules requis du kit d'installation obico
 **Ignorez l'échec de la construction de psutil. Cela sera corrigé plus tard.**
 
 
 ```pip3 install --require-virtualenv -r requirements.txt```

  Pour une raison quelconque, il y a encore des erreurs d'inclusion de module malgré ce qui précède, exécutez alors ce qui suit :
  ```
  pip3 install --require-virtualenv requests
  pip3 install --require-virtualenv backoff
  pip3 install --require-virtualenv sentry_sdk
  pip3 install --require-virtualenv bson
  pip3 install --require-virtualenv pathvalidate
  ```


  ### 3.3 python3-psutil est déjà installé depuis opkg. Copiez le module psutil dans l'environnement local (virtualenv).
  
  ```cp -R /usr/lib/python3.7/site-packages/psutil* /usr/share/moonraker-obico/env/lib/python3.7/site-packages/```
 
 
  À ce stade, obico devrait être prêt à fonctionner. Connecter maintenant une imprimante.


## 4. Lier l'imprimante

  ### 4.1 Récupérer le code de liaison du site Web d'obico (ou de votre propre serveur)
  
  Aller à [obico.io](https://app.obico.io/printers/wizard/setup/) pour obtenir un code permettant de connecter l'imprimante.
 
  ### 4.2 Exécuter le client de liaison et entrer le code de 4.1
  Exécutez l'application de liaison 
  
  ```python3 -m moonraker_obico.link -c /usr/share/moonraker-obico/moonraker-obico.cfg```


  ### 4.3 Exécuter obico
  Avant de lancer obico en tant que service, s'assurer qu'il s'exécute correctement. Surveiller les éventuelles erreurs.
  
  Ignorer les erreurs de limites du taux d'accès de l'API quand vous utilisez le service cloud obico.io.
  
  Pour terminer obico, utilisez Ctrl-c
  
  ```/usr/share/moonraker-obico/env/bin/python3 -m moonraker_obico.app -c /usr/share/moonraker-obico/moonraker-obico.cfg```


## 5. Créer un service

  Maintenant que tout est fonctionnel, nous allons configurer Obico pour qu'il fonctionne comme un service système au démarrage (daemon).

### 5.1 Création du service procd
  Editez et mettez ce qui suit `/etc/init.d/moonraker_obico_service`

```
  #!/bin/sh /etc/rc.common

  START=95
  STOP=1
  DEPEND=fstab
  USE_PROCD=1

  start_service() {
      procd_open_instance
      procd_set_param stdout 1
      procd_set_param stderr 1
      procd_set_param env PYTHONPATH=/usr/share/moonraker-obico
      procd_set_param command /usr/share/moonraker-obico/env/bin/python3 -m moonraker_obico.app -c /usr/share/moonraker-obico/moonraker-obico.cfg
      procd_close_instance
  }
  ```
  
  ### 5.2 Activer et exécuter le service
 ```
  /etc/init.d/moonraker_obico_service enable
  /etc/init.d/moonraker_obico_service start
  ```

  ou
  ```
  update-rc.d moonraker_obico_service defaults
  ```

## 5. Créer une tâche cron

La méthode utilisant procd pour démarrer en ntant que service peut fonctionner mais il faut qu'Obico attende que Moonraker soit complètement opérationnel avant de démarrer sinon il échoue et il faut alors le démarrer manuellement. On pourrait ajouter un délai (sleep) de 30 secondes pour s'assurer que cela se produise, mais cela retarderait le démarrage du service, une exécution en tant que tâche cron semble une meilleure solution.

Exécuter: `crontab -e -u root`

Cela lancera l'éditeur crontab en utilisant vi. Utiliser les commandes vi pour insérer (I), puis ESC et ensuite :wq.
 Vérifier que la tâche cron a bien été créée  `crontab -l`.
 
> `@reboot sleep 30s && /usr/share/moonraker-obico/obico-start.sh`

Redémarrer le Pad et vérifier que Obico démarrae correctement. Utiliser `ps`pour vérifier que le processus `obico` a bien été lancé:

> `ps | grep obico`

voilà, vous devriez être opérationnel avec Obico (anciennement Spaghetti Detective) !
