# Examen Administration Système - 16 janvier 2026

Ce README reprend toutes les questions de l’examen et apporte des réponses complètes et pratiques, dans mon style.

---

## Les deux familles

### Principales distributions

- **Famille Debian** : Debian, Ubuntu, Linux Mint, Pop!_OS  
- **Famille Red Hat** : Red Hat Enterprise Linux (RHEL), Fedora, CentOS, Rocky Linux, AlmaLinux  
- **Autres** : Arch Linux, openSUSE, Gentoo, Slackware  

### Positionnements et outils d’administration

- Debian : stable, polyvalent, beaucoup utilisé pour les serveurs et postes de travail. On gère avec `apt` et `dpkg`.  
- Red Hat : plus orienté entreprise, support commercial, outils principaux : `yum` ou `dnf`, `rpm`.  
- Autres : Arch et Gentoo pour ceux qui veulent tout configurer à fond, rolling release, gestion avec `pacman` ou `emerge`.

En résumé, Debian = tranquille et stable, Red Hat = pro et entreprise, les autres = bidouille et apprentissage poussé.

---

## Back to basics

**Différence kernel / utilisateur** :  

- Le **kernel** (ou mode superviseur) a tous les droits, accès direct au matériel et aux instructions critiques.  
- Le **mode utilisateur** a des droits limités, ne peut pas tout toucher, isolé pour protéger le système.  

---

## Qui est où ?

| Répertoire | Contenu |
|------------|---------|
| /etc       | fichiers de configuration du système |
| /bin et /usr/bin | programmes essentiels utilisables par tous |
| /sbin et /usr/sbin | programmes réservés à l’administration (root) |
| /home      | répertoires personnels des utilisateurs |
| /var       | fichiers qui changent souvent : logs, bases, spool |
| /var/log   | journaux système et applications |
| /var/lib   | données persistantes des services |

---

## Où est le pilote ?

Pour gérer les modules et pilotes sous Linux :  

- Liste des pilotes chargés : `lsmod`  
- Fichier binaire du module : `modinfo <module>`  
- Charger/décharger : `modprobe <module>` / `rmmod <module>`  
- Messages émis : `dmesg | grep <module>` ou `/var/log/kern.log`  

---

## MS Windows (WSL)

- WSL permet de lancer Linux **dans Windows** sans machine virtuelle.  
- Accès aux fichiers Windows via `/mnt/c`.  
- On peut utiliser Bash et tous les outils Linux.  
- Protocoles utilisés : NT Kernel et pipes UNIX.  

---

## On commence (sécuriser SSH)

Après installation d’un Linux, pour sécuriser SSH :  

1. Interdire la connexion root : `/etc/ssh/sshd_config` → `PermitRootLogin no`  
2. Changer le port SSH par défaut  
3. Utiliser uniquement des clés publiques/privées  
4. Activer firewall et fail2ban pour limiter les attaques  

---

## Alerte ! Problème de clef publique

Si SSH dit que la clef de l’hôte n’est pas reconnue :  

- Vérifier la clef  
- Supprimer la mauvaise clef dans `~/.ssh/known_hosts`  
- Pourquoi : pour éviter les attaques Man-in-the-Middle  

---

## Oh les gourmands !

Pour savoir qui prend le plus de place sur le disque (répertoires sous /home) :

```bash
du -sh /home/* | sort -hr | head -n 10
