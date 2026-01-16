Examen Administration Système - 16 janvier 2026


 Les deux familles de Linux

Linux, c’est un peu comme une grande famille : chaque distribution a sa personnalité et ses outils. Voici les principales.

 Principales distributions

- **Famille Debian** : Debian, Ubuntu, Linux Mint, Pop!_OS  
- **Famille Red Hat** : Red Hat Enterprise Linux (RHEL), Fedora, CentOS, Rocky Linux, AlmaLinux  
- **Autres** : Arch Linux, openSUSE, Gentoo, Slackware  

Positionnement et outils

- **Debian** : c’est le Linux tranquille. Stable, simple à prendre en main, parfait pour un serveur ou un PC classique. Outils principaux : `apt` et `dpkg`.  
- **Red Hat** : orienté entreprise, avec un vrai support commercial. On utilise `yum` ou `dnf` pour gérer les paquets, et `rpm` pour les fichiers individuels.  
- **Autres** : pour ceux qui aiment tout configurer eux-mêmes. Outils : `pacman` (Arch), `emerge` (Gentoo), `zypper` (openSUSE).  

---

 Mode kernel vs utilisateur

En gros, il y a deux manières de faire tourner du code sur un système :  

- **Kernel / superviseur** : le noyau a tous les droits. Il peut accéder à tout le matériel et gérer le système, rien ne lui échappe.  
- **Utilisateur** : limité et isolé. Chaque programme tourne dans sa “petite bulle” pour ne pas casser le système.  

---

## Où se trouvent les choses ?

Voici un petit guide pour savoir où chercher quand on administre un système Linux.

- `/etc` : tous les fichiers de configuration du système.  
- `/bin` et `/usr/bin` : les programmes que tout le monde peut utiliser.  
- `/sbin` et `/usr/sbin` : programmes réservés à l’administration (root).  
- `/home` : dossiers personnels des utilisateurs.  
- `/var` : fichiers qui changent tout le temps, comme les logs ou les bases de données.  
- `/var/log` : les journaux du système et des applications.  
- `/var/lib` : données persistantes utilisées par les services.  

---

## Gestion des pilotes

Pour savoir ce qui se passe avec les pilotes (drivers) sur Linux :  

- Voir les pilotes chargés : `lsmod`  
- Infos sur un module : `modinfo <module>`  
- Charger ou décharger un module : `modprobe <module>` ou `rmmod <module>`  
- Messages générés par un module : `dmesg | grep <module>` ou `/var/log/kern.log`  

---

## Windows et WSL

Si tu utilises Windows, WSL est pratique :  

- Il permet de lancer Linux directement sans machine virtuelle.  
- Tu peux accéder à tes fichiers Windows via `/mnt/c`.  
- Tu peux utiliser Bash et tous les outils Linux.  
- Les protocoles utilisés : NT Kernel et pipes UNIX.  

---

## Sécuriser SSH

Après l’installation d’un Linux, il vaut mieux sécuriser SSH :  

- Interdire la connexion directe de root (`PermitRootLogin no`)  
- Changer le port SSH par défaut  
- Utiliser uniquement des clés SSH publiques/privées  
- Activer un firewall et éventuellement fail2ban  

---

## Problème de clef publique

Si SSH t’avertit qu’il ne reconnaît pas la clef de l’hôte :  

- Vérifie la clef.  
- Supprime la ligne correspondante dans `~/.ssh/known_hosts`.  
- Ça évite les attaques “Man-in-the-Middle”.  

---

## Qui prend le plus de place ?

Pour identifier rapidement les utilisateurs qui consomment le plus d’espace dans `/home` :  

```bash
du -sh /home/* | sort -hr | head -n 10
