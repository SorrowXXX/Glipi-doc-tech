
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

## 🖥️ Installation de FusionInventory côté client

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

## ✅ Conclusion

Vous avez maintenant installé et configuré GLPI avec FusionInventory. Assurez-vous de sécuriser votre installation en suivant les meilleures pratiques de sécurité. N'oubliez pas de configurer GLPI et FusionInventory selon vos besoins spécifiques.
