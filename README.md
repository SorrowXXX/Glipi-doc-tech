
# 🛠️ Installation et configuration de GLPI avec FusionInventory

## 📋 Prérequis côté serveur

Avant de commencer, assurez-vous que votre système est à jour. Cela garantit que vous disposez des dernières corrections de sécurité et des versions les plus récentes des packages logiciels.

```bash
sudo apt update
sudo apt upgrade
```

## 📦 Installation des dépendances

Installez Apache, MySQL, PHP et les modules PHP nécessaires. Apache est le serveur web, MySQL est le système de gestion de base de données et PHP est le langage de script côté serveur.

```bash
sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql php-cli php-gd php-ldap php-imap php-xml php-curl php-mbstring php-xmlrpc php-apcu php-json php-zip php-intl
sudo systemctl restart apache2
```

## ⚙️ Configuration de MySQL

Créez une base de données et un utilisateur pour GLPI. Cela permet à GLPI de stocker et de gérer ses données dans une base de données MySQL.

```bash
sudo mysql -u root -p
CREATE DATABASE glpi;
GRANT ALL PRIVILEGES ON glpi.* TO 'glpi_user'@'localhost' IDENTIFIED BY 'your_password';
```

## 🌐 Installation de GLPI

Téléchargez et installez GLPI. GLPI est une application web de gestion de services informatiques (ITSM) et de gestion des actifs informatiques (ITAM).

```bash
cd /var/www/html && wget https://github.com/glpi-project/glpi/releases/download/9.5.x/glpi-9.5.x.tgz
sudo tar -zxvf glpi-10.0.6.tgz -C /var/www/html/
sudo chown -R www-data:www-data /var/www/html/glpi
sudo chmod -R 755 /var/www/html/glpi
sudo systemctl restart apache2
```

## 🖥️ Installation de FusionInventory côté client Windows

Téléchargez et installez FusionInventory. FusionInventory est un plugin pour GLPI qui permet de réaliser des inventaires de parc informatique.

```bash
# Téléchargement
wget https://github.com/fusioninventory/fusioninventory-agent/releases/download/2.6/fusioninventory-agent_windows-x64_2.6-portable.exe

# Instructions d'installation
1. Lancer le fichier.exe
2. Choisir la langue
3. Accepter la licence
4. Valider tous les composants à installer
5. Choisir le dossier d'installation
6. Choisir le mode serveur avec l'adresse IP du serveur GLPI "http://   <ip_du_serveur_glpi>/glpi/plugins/fusioninventory/"
7. Désactiver SSL
8. Pas de proxy
9. Choisir "comme une tâche Windows"
10. Choisir la fréquence voulue
11. Options diverses (ne pas sélectionner "seulement en mode local")
12. Suivre les étapes jusqu'à l'installation
13. Attendre quelques minutes avant que l'agent remonte dans GLPI
```

## 🖥️ Installation de FusionInventory côté client linux

Téléchargez et installez FusionInventory. FusionInventory est un plugin pour GLPI qui permet de réaliser des inventaires de parc informatique.

```bash
# Téléchargement
wget https://github.com/fusioninventory/fusioninventory-agent/releases/download/2.6/fusioninventory-agent_windows-x64_2.6-portable.exe](https://github.com/fusioninventory/fusioninventory-agent/releases/download/2.6/fusioninventory-agent_2.6-1_all.deb)

# Instructions d'installation
1. sudo dpkg -i fusioninventory-agent_2.6-1_all.deb
ou s'il y'a des erreurs
sudo apt-get -f install

2. fusioninventory-agent --version
3. sudo systemctl start fusioninventory-agent
4. sudo systemctl enable fusioninventory-agent
5. sudo fusioninventory-agent --server [URL_DU_SERVEUR]/glpi/plugins/fusioninventory
6. sudo systemctl status fusioninventory-agent
7. Attendre quelques minutes avant que l'agent remonte dans GLPI
```

## 📊 Surveillance de l'utilisation du CPU

Le script Bash suivant est utilisé pour surveiller l'utilisation du CPU sur un serveur. Il récupère l'utilisation actuelle du CPU, et si elle dépasse 90%, il crée une alerte dans le système de gestion de tickets GLPI.

```
#!/bin/bash
cpu_usage=$(top -bn 1 | grep "Cpu(s)" | awk '{print $2 + $4}')
cpu_usage_int=${cpu_usage%%,}
if [ "$cpu_usage_int" -ge 90 ]; then
session_token_json=$(curl -s -H 'Content-Type: application/json' http://10.255.0.17/glpi/apirest.php/initSession -H "Authorization: Basic Z2xwaTpnbHBp")
session_token=$(echo $session_token_json | jq '.session_token')
echo $session_token
session_token="${session_token//\"/}"
curl -X POST -H 'Content-Type: application/json' -H "Session-Token: $session_token" http://10.255.0.17/glpi/apirest.php/Ticket -d '{"input": {"name": "Alerte CPU","content": "La CPU est à 90% sur le serveur"}}'
```

`* * * * * /home/admglpi/script.sh` est une tâche cron qui exécute ce script toutes les minutes. Cela permet de surveiller l'utilisation du CPU en continu et de créer une alerte dès que l'utilisation dépasse 90%.

Ce script est utile pour surveiller les ressources du serveur et réagir rapidement en cas de surcharge du CPU, ce qui pourrait affecter les performances du serveur.

## ✅ Conclusion

Vous avez maintenant installé et configuré GLPI avec FusionInventory. Assurez-vous de sécuriser votre installation en suivant les meilleures pratiques de sécurité. N'oubliez pas de configurer GLPI et FusionInventory selon vos besoins spécifiques.
