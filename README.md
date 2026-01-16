# Examen Administration Système - 16 janvier 2026

Bienvenue sur le dépôt pour mon examen d’administration système. Ce document reprend les questions posées et apporte des réponses simples et pratiques.

---

## Les deux familles

### Principales distributions

- Famille Debian : Debian, Ubuntu, Linux Mint, Pop!_OS  
- Famille Red Hat : Red Hat Enterprise Linux (RHEL), Fedora, CentOS, Rocky Linux, AlmaLinux  
- Autres : Arch Linux, openSUSE, Gentoo, Slackware  

### Positionnement et outils

- Debian : stable, simple à utiliser, adapté à la fois pour le serveur et le poste de travail. Outils principaux : `apt` et `dpkg`.  
- Red Hat : orienté entreprise, support commercial. Outils : `yum` ou `dnf`, `rpm`.  
- Autres : pour les curieux qui veulent tout configurer eux-mêmes. Outils : `pacman` (Arch), `emerge` (Gentoo), `zypper` (openSUSE).  

---

## Back to basics

- Mode kernel / superviseur : accès total au système et au matériel, peut tout faire.  
- Mode utilisateur : accès limité, isolé pour la sécurité, ne peut pas toucher aux fonctions critiques.  

---

## Qui est où ?

- `/etc` : fichiers de configuration du système  
- `/bin` et `/usr/bin` : programmes pour tous les utilisateurs  
- `/sbin` et `/usr/sbin` : programmes pour l’administration (root)  
- `/home` : dossiers personnels des utilisateurs  
- `/var` : fichiers variables comme les logs ou bases de données  
- `/var/log` : journaux du système et des applications  
- `/var/lib` : données persistantes des services  

---

## Où est le pilote ?

- Voir les pilotes chargés : `lsmod`  
- Infos sur un module : `modinfo <module>`  
- Charger / décharger : `modprobe <module>` ou `rmmod <module>`  
- Messages des modules : `dmesg | grep <module>` ou `/var/log/kern.log`  

---

## MS Windows et WSL

- WSL permet de lancer Linux dans Windows sans machine virtuelle.  
- On peut accéder aux fichiers Windows via `/mnt/c`.  
- Permet d’utiliser Bash et tous les outils Linux.  
- Protocoles utilisés : NT Kernel et pipes UNIX.  

---

## Sécurisation SSH

Après installation :  

- Interdire la connexion root (`PermitRootLogin no`)  
- Changer le port SSH par défaut  
- Utiliser uniquement des clés SSH publiques/privées  
- Activer un firewall et fail2ban  

---

## Problème de clef publique

Si SSH dit que la clef de l’hôte n’est pas reconnue :  

- Vérifier la clef  
- Supprimer l’ancienne clef dans `~/.ssh/known_hosts`  
- Cela permet d’éviter les attaques Man-in-the-Middle  

---

## Utilisateurs gourmands

Pour voir qui prend le plus de place dans `/home` :  

```bash
du -sh /home/* | sort -hr | head -n 10
