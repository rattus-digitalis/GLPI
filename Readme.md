📘 GLPI + Zabbix – Infrastructure Docker automatisée avec Ansible

Ce projet fournit une infrastructure IT automatisée, reproductible et prête pour la production, idéale pour un lab d’entreprise, une petite infrastructure ou un environnement d’apprentissage.

La stack inclut :

Traefik v3 (reverse proxy + HTTPS)

GLPI (ITSM / Helpdesk)

Zabbix Server (monitoring)

Docker + Docker Compose

Ansible pour l'automatisation et l'orchestration complète

🚀 Fonctionnalités

✔ Reverse-proxy complet avec HTTPS (Traefik v3)

✔ Déploiement GLPI entièrement automatisé

✔ Zabbix Server + interface Web

✔ Playbooks Ansible modulaires et idempotents

✔ Compatible Arch Linux et Debian/Ubuntu

✔ Variables centralisées (group_vars/)

✔ Structure claire avec rôles séparés

🌍 Prérequis
Systèmes compatibles

Arch Linux ✔

Debian / Ubuntu ✔

Installation d’Ansible
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


Ce playbook gère automatiquement :

Installation de Docker

Configuration du daemon

Déploiement de Traefik

Déploiement de GLPI

Déploiement de Zabbix

👉 Aucune intervention manuelle requise.

🌐 Accès aux services Web
Service	URL
Traefik Dashboard	http://localhost:8080

GLPI	https://glpi.localhost

Zabbix	https://zabbix.localhost

⚠️ Certificats ACME en mode staging (auto-signés).

🛠️ Commandes utiles
Vérifier les conteneurs
docker ps

Redémarrer Traefik
cd /opt/traefik
docker compose down && docker compose up -d

Mode dry-run (simulation)
ansible-playbook ansible/playbooks/install_glpi.yml --check

🔐 Sécurité

Le fichier acme.json possède les permissions correctes (0600)

Chaque service est isolé dans /opt/<service>

Aucun conteneur n’est exposé directement (Traefik only)

Pour une utilisation production :

Activer Let's Encrypt en mode production

Ajouter des middlewares Traefik :

Security headers

Rate limiting

Mettre en place une sauvegarde automatique des volumes (Restic, BorgBackup)
