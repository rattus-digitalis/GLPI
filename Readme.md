📘 GLPI + Zabbix – Infrastructure Docker automatisée avec Ansible

Infrastructure complète comprenant :

Traefik v3 (reverse proxy & TLS)

GLPI (ITSM / Helpdesk)

Zabbix Server (monitoring)

Docker + Docker Compose

Ansible pour l'installation, le déploiement et l'orchestration

Ce projet fournit une stack IT totalement automatisée, reproductible et production-ready, idéale pour un lab ou une petite infra.

🚀 Fonctionnalités

✔ Reverse-proxy avec HTTPS (Traefik v3)
✔ GLPI fonctionnel avec DB et configuration
✔ Zabbix Server + Web UI
✔ Playbooks Ansible : déploiement automatisé complet
✔ Support Arch Linux & Debian/Ubuntu
✔ Configuration idempotente
✔ Séparation propre des rôles Ansible
✔ Variables centralisées dans group_vars/

🏗️ Architecture
┌──────────────────────────┐
│        Traefik v3        │
│ glpi.localhost           │
│ zabbix.localhost         │
└─────────────┬────────────┘
              │
     ┌────────┴───────────┐
     │                    │
┌────▼───────┐      ┌─────▼──────┐
│   GLPI     │      │   Zabbix   │
│  MariaDB   │      │   MySQL    │
└────────────┘      └────────────┘

📁 Arborescence du projet
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
│   ├── full_deploy.yml
│   ├── install_glpi.yml
│   ├── install_zabbix.yml
│   └── install_traefik.yml
│
└── roles/
    ├── docker/
    ├── traefik/
    ├── glpi/
    └── zabbix/

🌍 Prérequis
Systèmes compatibles

Arch Linux ✔

Debian / Ubuntu ✔

Paquets requis
sudo pacman -S ansible
# ou
sudo apt install ansible

Ports requis
Service	Ports
Traefik	80 / 443 / 8080
Zabbix Server	10051
Bases de données	3306 (interne Docker)
⚙️ Configuration Ansible
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

▶️ Déploiement des services
1️⃣ Installer Docker + Traefik
ansible-playbook ansible/playbooks/install_traefik.yml

2️⃣ Installer GLPI
ansible-playbook ansible/playbooks/install_glpi.yml

3️⃣ Installer Zabbix
ansible-playbook ansible/playbooks/install_zabbix.yml

🚀 Déploiement complet automatique

💥 Tout installer en une seule commande :

ansible-playbook ansible/playbooks/full_deploy.yml


Ce playbook :

Installe Docker

Configure le daemon

Déploie Traefik

Déploie GLPI

Déploie Zabbix

Aucune intervention manuelle.

🌐 Accès aux services Web
Service	URL
Traefik Dashboard	http://localhost:8080

GLPI	https://glpi.localhost

Zabbix	https://zabbix.localhost

⚠️ Les certificats ACME sont auto-signés (staging).

🛠️ Commandes utiles
Vérifier les conteneurs
docker ps

Redémarrer Traefik
cd /opt/traefik
docker compose down && docker compose up -d

Mode dry-run (simuler sans exécuter)
ansible-playbook playbooks/install_glpi.yml --check

🔐 Sécurité

acme.json possède les permissions correctes (0600)

Tout le stack est isolé dans /opt/<service>

Les conteneurs ne sont jamais exposés directement (Traefik only)

Pour un environnement prod :

Utiliser Let's Encrypt production

Ajouter middlewares Traefik (security headers, rate-limits)

Ajouter une sauvegarde automatique des volumes (Restic/Borg)
