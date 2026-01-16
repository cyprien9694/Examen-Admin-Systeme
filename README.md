# Examen Administration Système - 16 janvier 2026

Ce document reprend les questions de l’examen et y apporte des réponses synthétiques et pratiques.

---

## Les deux familles

### Principales distributions

- **Famille Debian** : Debian, Ubuntu, Linux Mint, Pop!_OS  
- **Famille Red Hat** : Red Hat Enterprise Linux (RHEL), Fedora, CentOS, Rocky Linux, AlmaLinux  
- **Autres** : Arch Linux, openSUSE, Gentoo, Slackware  

### Positionnements et outils d’administration

| Famille | Positionnement | Outils principaux |
|---------|----------------|-----------------|
| Debian  | Stabilité, open source, polyvalente | apt, dpkg, /etc/apt, systemd |
| Red Hat | Entreprise, support commercial | yum/dnf, rpm, /etc/yum.repos.d, systemd |
| Autres  | Rolling release (Arch), personnalisation (Gentoo) | pacman (Arch), zypper (openSUSE), emerge (Gentoo) |

---

## Back to basics

**Mode kernel/superviseur vs mode utilisateur**  

- Kernel : Accès complet au matériel et aux ressources critiques, exécution privilégiée  
- Utilisateur : Accès limité, ne peut pas exécuter certaines instructions sensibles, isolation pour sécurité  

---

## Qui est où ?

| Répertoire | Contenu |
|------------|---------|
| /etc       | Fichiers de configuration système |
| /bin et /usr/bin | Binaries essentiels pour tous les utilisateurs |
| /sbin et /usr/sbin | Binaries pour l’administration système (root) |
| /home      | Répertoires personnels des utilisateurs |
| /var       | Fichiers variables : logs, bases de données, spool |
| /var/log   | Logs système et applicatifs |
| /var/lib   | Données persistantes des services |

---

## Où est le pilote ?

- Liste des pilotes chargés : `lsmod`  
- Fichiers binaires des modules : `modinfo <module>`  
- Charger/décharger : `modprobe <module>` / `rmmod <module>`  
- Messages : `dmesg | grep <module>` ou `/var/log/kern.log`  

---

## MS Windows (WSL)

- Permet d’exécuter GNU/Linux sur Windows sans VM  
- Accès aux fichiers Windows (/mnt/c)  
- Support Bash et outils GNU/Linux  
- Protocoles utilisés : NT Kernel, pipes UNIX  

---

## Sécurisation SSH après installation

1. Désactiver root login : `/etc/ssh/sshd_config` → `PermitRootLogin no`  
2. Changer le port SSH  
3. Utiliser clés publiques/privées  
4. Activer firewall et fail2ban  

---

## Problème de clef publique d’hôte

- Vérifier la clef publique de l’hôte  
- Supprimer la ligne correspondante dans `~/.ssh/known_hosts`  
- Permet d’éviter attaques Man-in-the-Middle  

---

## Utilisateurs qui consomment le plus d’espace

```bash
du -sh /home/* | sort -hr | head -n 10
