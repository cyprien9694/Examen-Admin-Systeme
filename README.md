#!/bin/bash

# ===============================
# Script : setup_exam_git.sh
# Usage : ./setup_exam_git.sh <nom_de_dossier> <URL_depot_GitHub>
# Exemple : ./setup_exam_git.sh Examen-Admin-Systeme https://github.com/tonPseudo/Examen-Admin-Systeme.git
# ===============================

# Vérification des arguments
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <nom_de_dossier> <URL_depot_GitHub>"
    exit 1
fi

PROJET_DIR=$1
GIT_URL=$2

# 1. Création du dossier du projet
echo "Création du dossier $PROJET_DIR..."
mkdir -p "$PROJET_DIR"
cd "$PROJET_DIR" || { echo "Erreur : impossible d'entrer dans le dossier"; exit 1; }

# 2. Création du README.md
echo "Création du README.md..."
cat << 'EOF' > README.md
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
| Debian  | Stabilité, open source, polyvalente | apt, /etc/apt, dpkg, systemd |
| Red Hat | Entreprise, support commercial | yum/dnf, /etc/yum.repos.d, rpm, systemd |
| Autres  | Rolling release (Arch), personnalisation (Gentoo) | pacman (Arch), zypper (openSUSE), emerge (Gentoo) |

---

## Back to basics

**Mode kernel/superviseur vs mode utilisateur**  
- Kernel : Accès complet au matériel et aux ressources critiques, exécution privilégiée.  
- Utilisateur : Accès limité, ne peut pas exécuter certaines instructions sensibles, isolation pour sécurité.

---

## Qui est où ?  

| Répertoire | Contenu |
|------------|---------|
| /etc     | Fichiers de configuration système |
| /bin et /usr/bin | Binaries essentiels pour tous les utilisateurs |
| /sbin et /usr/sbin | Binaries pour l’administration système (root) |
| /home    | Répertoires personnels des utilisateurs |
| /var     | Fichiers variables : logs, bases de données, spool |
| /var/log | Logs système et applicatifs |
| /var/lib | Données persistantes des services |

---

## Où est le pilote ?

- Liste des pilotes chargés : lsmod  
- Fichiers binaires des modules : modinfo <module>  
- Charger/décharger : modprobe <module> / rmmod <module>  
- Messages : dmesg | grep <module> ou /var/log/kern.log

---

## MS Windows

**WSL (Windows Subsystem for Linux)**  
- Permet d’exécuter GNU/Linux sur Windows sans VM  
- Accès aux fichiers Windows (/mnt/c), support Bash et outils GNU/Linux  
- Protocoles : NT Kernel, pipes UNIX

---

## On commence

Sécurisation SSH :
1. Désactiver root login
2. Changer port par défaut
3. Utiliser clés publiques/privées
4. Activer fail2ban ou firewall

---

## Alerte !

- Vérifier la clef publique de l’hôte  
- Supprimer clef incorrecte dans ~/.ssh/known_hosts  
- Pour éviter attaques Man-in-the-Middle

---

## Oh les gourmands !

\`\`\`bash
du -sh /home/* | sort -hr | head -n 10
\`\`\`

---

## On va aider les devs

- Debian : sudo apt install build-essential git  
- Red Hat : sudo dnf groupinstall "Development Tools"

---

## Qui fait quoi ?

- Dernières connexions : last  
- Connexions sudo : grep sudo /var/log/auth.log (Debian), /var/log/secure (Red Hat)

---

## Post mortem

- Logs système : /var/log/syslog, /var/log/messages, journalctl -xe  
- Journaux services : journalctl -u <service>

---

## On surveille...

\`\`\`bash
systemctl is-enabled bidule
systemctl status bidule
systemctl enable bidule
journalctl -u bidule
systemctl restart bidule
dpkg -S $(which bidule)  # Debian
rpm -qf $(which bidule)  # Red Hat
\`\`\`

---

## Zut

- Mode recovery / live CD pour réinitialiser mot de passe root  
- Monter partition et éditer /etc/shadow

---

## C’est la faute à Rémy

Outils diagnostic HTTP(S) :
- systemctl status apache2/nginx
- journalctl -u apache2
- netstat / ss
- curl -I http://localhost
- ping, traceroute
- tcpdump

---

## Au charbon !

ElasticSearch sur Debian :
1. Ajouter clé GPG et repo officiel  
2. sudo apt update && sudo apt install elasticsearch  
3. Config /etc/elasticsearch/elasticsearch.yml  
4. systemctl enable --now elasticsearch  
5. Vérifier : curl -X GET "localhost:9200/"

---

## Le Web

- Paquets : apache2, php, libapache2-mod-php, mysql-server/mariadb-server  
- Organisation : /var/www/site1, /var/www/site2  
- PHP : apt install php libapache2-mod-php  
- Logs séparés par VirtualHost (CustomLog / ErrorLog)  
- Outils : GoAccess, AWStats

EOF

echo "README.md créé avec succès."

# 3. Initialiser Git
echo "Initialisation du dépôt Git..."
git init

# 4. Ajouter et commit
git add README.md
git commit -m "Ajout du README complet pour l'examen d'administration système"

# 5. Lier dépôt distant
git remote add origin "$GIT_URL"

# 6. Pousser sur GitHub
git branch -M main
git push -u origin main

echo "✅ Dépôt créé et README.md publié sur GitHub !"
echo "URL : $GIT_URL"
