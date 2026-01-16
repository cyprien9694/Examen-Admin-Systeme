# Examen Administration système 16 janvier 2026

Attendu : une url menant à un fichier README.md (format _markdown_) sur une plateforme GIT publique _(codeberg, gitlab, github, ...)_.
reprenant ce document en le complétant. 

## Les deux familles

Quelles sont les principales distributions de la famille Debian, de la famille Red Hat, et les autres ?

Comment pourriez-vous décrire, succintement, leurs positionnements respectifs ? Les différences au niveau des outils d'admnistration ?

Linux, c’est un peu comme une grande famille : chaque distribution a sa personnalité et ses outils :   
 
- Famille Debian : Debian, Ubuntu, Linux Mint, Pop!_OS  
- Famille Red Hat : Red Hat Enterprise Linux (RHEL), Fedora, CentOS, Rocky Linux, AlmaLinux  
- Autres : Arch Linux, openSUSE, Gentoo, Slackware  

**Positionnement et outils :**  
- Debian : stable et simple, parfait pour serveur ou PC classique. Outils : `apt`, `dpkg`.  
- Red Hat : orienté entreprise, support commercial, outils : `yum`/`dnf`, `rpm`.  
- Autres : pour les utilisateurs expérimentés, outils : `pacman` (Arch), `emerge` (Gentoo), `zypper` (openSUSE).  

## Back to basics
Quelle est la différence fondamentale entre du code qui tourne en mode kernel/superviseur et en mode utilisateur ?
 
- **Mode kernel / superviseur** : le noyau a tous les droits, accès complet au matériel et à la mémoire, gère le système.  
- **Mode utilisateur** : limité et isolé, accès au système uniquement via appels systèmes, protège le noyau et les autres processus.  

## Qui est où ?

Indiquez, en quelques mots, pour ces répertoires, ce qu'ils sont supposés contenir :
 
- /etc : tous les fichiers de configuration du système.  
- /bin et /usr/bin : les programmes que tout le monde peut utiliser.  
- /sbin et /usr/sbin : programmes réservés à l’administration (root).  
- /home : dossiers personnels des utilisateurs.  
- /var : fichiers qui changent tout le temps, comme les logs ou les bases de données.  
- /var/log : les journaux du système et des applications.  
- /var/lib : données persistantes utilisées par les services.   

## Où est le pilote ?

Comment retrouver, sous GNU/Linux :

- La liste des pilotes chargées en mémoire ?  Voir les pilotes chargés : lsmod  
- La liste des fichiers binaires _(modules)_ qui les fournissent ? Infos sur un module : modinfo <module>  
- Forcer leur chargement et leur déchargement ? Charger ou décharger un module : modprobe <module> ou rmmod <module>  
- Les messages qu'ils ont émis ? Messages générés par un module : dmesg | grep <module> ou /var/log/kern.log  

## MS Windows

Comment fonctionne en pratique WSL _(Windows Subsystem for Linux)_ ? Que permet-il de faire ? Quels protocoles utilise-t-il ?
 
- Il permet de lancer Linux directement sans machine virtuelle.  
- Tu peux accéder à tes fichiers Windows via /mnt/c.  
- Tu peux utiliser Bash et tous les outils Linux.  
- Les protocoles utilisés : NT Kernel et pipes UNIX.   

## On commence

Vous venez d'installer un système GNU/Linux que faites-vous pour en sécuriser les accès via SSH ?

Après l’installation d’un Linux, il vaut mieux sécuriser SSH :  

- Interdire la connexion directe de root (PermitRootLogin no)  
- Changer le port SSH par défaut  
- Utiliser uniquement des clés SSH publiques/privées  
- Activer un firewall et éventuellement fail2ban  

## Alerte !

Vous vous connectez à un système GNU/Linux, le client ssh vous averti qu'il a un problème de reconnaissance de sa clef publique
d'hôte. Que faites-vous ? Pourquoi ?

Si SSH t’avertit qu’il ne reconnaît pas la clef de l’hôte :  

- Vérifie la clef  
- Supprime la ligne correspondante dans ~/.ssh/known_hosts  
- Ça évite les attaques “Man-in-the-Middle”  

## Oh les gourmands !

Comment identifier rapidement, avec une seule ligne de commande, les utilisateurs qui consomment le plus d'espace de stockage.
_N.B._ Vous pouvez supposer que les répertoires personnels sont tous des sous-répertoires de `/home` 

Pour identifier rapidement les utilisateurs qui consomment le plus d’espace dans /home :  
du -sh /home/* | sort -hr | head -n 10

## On va aider les devs...

Une équipe de développeur.se.s vous demande de préparer un système GNU/Linux pour du développent système (C, make, etc.)

Que faites-vous en pratique en tant qu'administrateur.trice pour commencer ?

- Sur une distribution de la famille Debian
- Sur une distribution de la famille Red Hat

Si une équipe de devs arrive et veut coder en C, make, etc. :  

- Sur Debian/Ubuntu :  
sudo apt update  
sudo apt install build-essential git

- Sur Red Hat/Fedora :  
sudo dnf groupinstall "Development Tools"  
sudo dnf install git

## Qui fait quoi ?

Comment pourriez-vous retrouver la liste des dernières connexions d'utilisateurs à un système GNU/Linux, autant à distance que localement ?

Comment identifier les sessions où un.e utilisateur.trice à utilisé _sudo_ pour avoir les droit d'administration ?

Pour savoir qui s’est connecté :  
- Liste des dernières connexions : last  
- Sessions sudo : grep sudo /var/log/auth.log (Debian) ou /var/log/secure (Red Hat)

## Post mortem

Le système que vous administrez est tombé cette nuit. Comment récupérer des informations sur ce qui s'est passé ?

Si le système tombe, pour comprendre ce qui s’est passé :  

- Vérifier les logs : /var/log/syslog ou /var/log/messages  
- Journaux récents : journalctl -xe  
- Logs d’un service précis : journalctl -u <service>  

## On surveille...

Sur un serveur GNU/Linux qui démarre via _systemd_, un service nommé _bidule_, comment :

Pour un service nommé bidule :

- Vérifier s’il est activé au démarrage : systemctl is-enabled bidule  
- Vérifier s’il tourne : systemctl status bidule  
- Activer au démarrage : systemctl enable bidule  
- Voir ses messages : journalctl -u bidule  
- Redémarrer : systemctl restart bidule  
- Trouver le paquet d’origine : dpkg -S $(which bidule) (Debian) ou rpm -qf $(which bidule) (Red Hat)  

## Zut

Vous devez reprendre la main, en tant qu'administrateur, d'un système GNU/Linux mais vous n'avez aucun mot de passe à votre disposition, ni _root_ ni utilisateur. Que pouvez-vous faire ?

Si tu n’as ni mot de passe root ni utilisateur :  

- Passer par un live CD ou mode recovery  
- Monter la partition et réinitialiser le mot de passe dans /etc/shadow 

## C'est la faute à Rémy

Un service réseau http(s) ne fonctionne plus. Vous avez accès à ce système comme administrateur.trice. Quels outils utilisez-vous pour diagnostiquer le problème, indiquez précisément ce que chaque outil permet de vérifier.

Outils pratiques :  

- systemctl status apache2 ou nginx → vérifier que le service tourne  
- journalctl -u apache2 → logs du service  
- netstat -tulnp ou ss -tulnp → quels ports sont ouverts  
- curl -I http://localhost → tester la réponse HTTP  
- ping / traceroute → vérifier le réseau  
- tcpdump → analyser le trafic  

## Au charbon !

On vous demande d'installer un serveur de base de données _Elastic_ (autrefois nommé _Elastic Search_) sur un système Debian (ou autre si vous préférez)

Décrivez votre recherche documentaire, la méthode que vous sélectionnez, les commandes que vous exécutez et les fichiers de configuration impliqués.

- Ajouter le dépôt officiel et la clé GPG  
- Installer : sudo apt update && sudo apt install elasticsearch  
- Configurer /etc/elasticsearch/elasticsearch.yml  
- Activer et démarrer : systemctl enable --now elasticsearch  
- Vérifier : curl -X GET "localhost:9200/"

## Le Web

Vous avez la charge de gérer un serveur Web sous Debian GNU/Linux, pour héberger plusieurs sites Web (hébergement mutualisé).

- Quels paquets installez-vous ?
- Comment organisez sous le stockage des différents contenus ?
- Certains sites ont besoin de PHP, comment l'installez-vous ?
- Comment pouvez-vous organiser l'enregistrement séparé des visites des différents sites ?
- Quel(s) outil(s) peuvent vous permettre de fournir une vison des visites de chaque site à chaque Webmaster ?

- Installer les paquets : apache2, php, libapache2-mod-php, mysql-server ou mariadb-server  
- Organiser les sites : /var/www/site1, /var/www/site2, etc.  
- Journaux séparés pour chaque site via VirtualHost (CustomLog et ErrorLog)  
- Statistiques : GoAccess ou AWStats pour chaque webmaster  




