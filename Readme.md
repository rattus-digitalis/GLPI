📘 README complet pour ton projet

Tu peux coller ça directement dans un fichier README.md à la racine du repo.

GLPI + Zabbix – Infrastructure Docker automatisée avec Ansible

Infrastructure complète incluant :

Traefik v3 (reverse proxy & TLS)

GLPI (ITSM)

Zabbix (monitoring)

Docker + Docker Compose

Ansible (installation & orchestration automatique)

Ce projet sert à déployer automatiquement une stack IT complète sur une machine (Debian, Ubuntu, Arch Linux…).

🚀 Fonctionnalités

✔ Reverse-proxy Traefik avec HTTPS
✔ GLPI entièrement fonctionnel + configuration DB + templates
✔ Zabbix Server + Web UI + Base de données
✔ Playbooks Ansible pour déploiement automatique
✔ Automated Docker installation (Arch & Debian support)
✔ Idempotence complète : rejouer les playbooks n’est jamais destructif
✔ Structure professionnelle des rôles Ansible
✔ Découpage clair : docker / traefik / glpi / zabbix
✔ Compatible environnement production ou lab

🏗️ Architecture technique
┌──────────────────────────┐
│        Traefik v3        │  ← TLS, routing, proxy
│  glpi.localhost          │
│  zabbix.localhost        │
└─────────────┬────────────┘
              │
     ┌────────┴───────────┐
     │                    │
┌────▼───────┐      ┌─────▼──────┐
│   GLPI     │      │   Zabbix   │
│  + MariaDB │      │  + MySQL   │
└────────────┘      └────────────┘

📁 Structure du dépôt
ansible/
│
├── ansible.cfg
├── inventory/
│   ├── hosts.ini
│   └── group_vars/
│       ├── all.yml
│       ├── glpi.yml
│       └── zabbix.yml
│
├── playbooks/
│   ├── install_glpi.yml
│   ├── install_zabbix.yml
│   ├── install_traefik.yml
│   └── full_deploy.yml
│
└── roles/
    ├── docker/
    ├── traefik/
    ├── glpi/
    └── zabbix/

🌍 Prérequis
Système compatible :

Arch Linux ✔

Debian / Ubuntu ✔

Toutes distributions gérées par Ansible ✔

Paquets requis
sudo pacman -S ansible
# ou
sudo apt install ansible

Ports ouverts :

80 / 443 (Traefik)

3306 (DB internes)

10051 (Zabbix server)

⚙️ Configuration de l’inventaire
inventory/hosts.ini
[local]
127.0.0.1 ansible_connection=local ansible_become=true

group_vars/all.yml
acme_email: "admin@example.com"
timezone: "Europe/Paris"

group_vars/glpi.yml
glpi_port: 80
glpi_domain: "glpi.localhost"
glpi_db_name: "glpidb"
glpi_db_user: "glpiuser"
glpi_db_password: "glpipassword"

group_vars/zabbix.yml
zabbix_domain: "zabbix.localhost"
zabbix_db_name: "zabbix"
zabbix_db_password: "zabbixpwd"

▶️ Déploiement pas à pas
1️⃣ Installer Traefik
ansible-playbook playbooks/install_traefik.yml

2️⃣ Installer GLPI
ansible-playbook playbooks/install_glpi.yml

3️⃣ Installer Zabbix
ansible-playbook playbooks/install_zabbix.yml

🚀 Déploiement complet automatique

💥 La commande ultime :

ansible-playbook playbooks/full_deploy.yml


Ce playbook :

Installe Docker

Configure le daemon

Déploie Traefik

Déploie GLPI

Déploie Zabbix
→ Sans intervention manuelle

🌐 Accès aux services
Service	URL
Traefik dashboard	http://localhost:8080

GLPI	https://glpi.localhost

Zabbix	https://zabbix.localhost
🛠️ Commandes utiles
Vérifier les conteneurs
docker ps

Redémarrer toute la stack Traefik
cd /opt/traefik
docker compose down
docker compose up -d

Rejouer Ansible sans changement
ansible-playbook playbooks/install_glpi.yml --check

🔐 Sécurité

Les dossiers /opt/glpi et /opt/zabbix doivent être root-only

Le fichier acme.json est en 600 (correct)

Les règles Traefik peuvent être renforcées (headers, middlewares)

Prévoir un backup régulier des volumes Docker

Je peux ajouter une section Sécurité avancée si tu veux :

Fail2ban

Headers Traefik security

Audit GLPI automatisé

Monitoring auto des conteneurs via Zabbix alerts
