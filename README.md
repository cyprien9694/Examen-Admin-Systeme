# Examen Administration Système - 16 janvier 2026

---

## Les deux familles

### Les distributions principales

- **Famille Debian** : Debian, Ubuntu, Linux Mint, Pop!_OS. Perso, j’aime bien Ubuntu pour bosser vite.  
- **Famille Red Hat** : Red Hat Enterprise Linux (RHEL), Fedora, CentOS, Rocky Linux, AlmaLinux. Plus orienté entreprise et serveur.  
- **Autres** : Arch Linux, openSUSE, Gentoo, Slackware. Là c’est plus pour bidouiller et apprendre vraiment le système.  

### Positionnements et outils

| Famille | Positionnement | Outils qu’on utilise souvent |
|---------|----------------|-----------------------------|
| Debian  | Stable, open source, polyvalente | apt, dpkg, /etc/apt, systemd |
| Red Hat | Entreprise, support commercial | yum/dnf, rpm, /etc/yum.repos.d, systemd |
| Autres  | Rolling release, perso/custom | pacman (Arch), zypper (openSUSE), emerge (Gentoo) |

En gros, Debian c’est tranquille pour débuter, Red Hat c’est pro, les autres c’est pour les curieux qui veulent tout configurer eux-mêmes.  

---

## Back to basics

**Kernel vs utilisateur**  

- Le **kernel** peut tout faire, accès total au matos et aux instructions sensibles.  
- L’utilisateur ne peut pas toucher au système directement, sinon le kernel l’arrête.  
- En gros, kernel = boss, utilisateur = employé qui suit les règles.  

---

## Qui est où ?

| Répertoire | Contenu |
|------------|---------|
| /etc       | Tous les fichiers de config du système |
| /bin et /usr/bin | Programmes que tout le monde peut lancer |
| /sbin et /usr/sbin | Programmes pour l’admin (root) |
| /home      | Répertoires perso des utilisateurs |
| /var       | Fichiers variables : logs, bases, spool |
| /var/log   | Logs du système et des applis |
| /var/lib   | Données persistantes des services |

---

## Les pilotes

- Voir ce qui est chargé : `lsmod`  
- Info sur un module : `modinfo <module>`  
- Charger/décharger : `modprobe <module>` / `rmmod <module>`  
- Messages qu’ils ont générés : `dmesg | grep <module>` ou regarder `/var/log/kern.log`  

---

## Windows et WSL

- WSL permet de lancer Linux **dans Windows**, pas besoin de VM.  
- Tu peux accéder à tes fichiers Windows via `/mnt/c`.  
- On peut utiliser bash et tous les outils Linux.  
- Les communications passent par le kernel Windows et des pipes UNIX.  

---

## Sécurisation SSH

Après avoir installé Linux, je fais toujours ça :  

1. Interdire la connexion root : `/etc/ssh/sshd_config` → `PermitRootLogin no`  
2. Changer le port par défaut  
3. Utiliser des clés SSH plutôt que mot de passe  
4. Activer firewall et fail2ban pour limiter les attaques  

---

## Clé publique inconnue

- Si SSH dit que la clé de l’hôte est inconnue :  
  - Vérifier la clé  
  - Supprimer l’ancienne dans `~/.ssh/known_hosts`  
  - C’est pour éviter les attaques Man-in-the-Middle  

---

## Les utilisateurs gourmands

Pour savoir qui prend le plus de place :  

```bash
du -sh /home/* | sort -hr | head -n 10
