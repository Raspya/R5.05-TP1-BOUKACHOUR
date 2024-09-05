# Manuel d’installation CodeIgniter4

**Dernière modification** : 5 septembre 2024

**Présentation**

Ce manuel vous guide dans l'installation de CodeIgniter 4 sur Linux Fedora, depuis la configuration de l'environnement de développement jusqu'au déploiement en production. Destiné aux développeurs de tous niveaux, il offre des instructions simples pour mettre en place rapidement un système fonctionnel et sécurisé. Suivez ces étapes pour réussir votre transition du développement à la production en toute confiance.

…

**Intervenants**

Ce manuel d'installation a été élaboré par Matthias Bernouy et Arthur Lecomte, étudiants en troisième année de BUT Informatique. Passionnés par le développement web, ils ont mis leurs compétences en pratique pour créer ce guide afin de faciliter l'installation de CodeIgniter 4 sur Fedora, en alliant rigueur académique et expérience pratique.

# 1.  Environnement de développement

## a. Prérequis

```bash
# Installation des différents composants

sudo dnf install php-cli
sudo dnf install phpunit composer
sudo dnf install php-mysqli

# Vérifier les installations avec les commandes suivantes

php -v # minimum 7.4 requis
composer --version # minimum 2.7.6
```

## b. Création d’un projet

```bash

# Remplacer nomDuProjet par le nom que vous souhaitez
# La commande va installer le framework et toutes ses dépendances

composer create-project codeigniter4/appstarter nomDuProjet

# On se place dans le projet
cd nomDuProjet

# On lance l'application dans l'environnement de dev
php spark serve
```

<aside>
⚠️

Vous devez avoir le port 8080 de disponible

</aside>

Ouvrez un navigateur et rendez-vous sur http://localhost:8080, vous aurez un visuel semblable à la capture ci-dessous.

![image.png](Manuel%20d%E2%80%99installation%20CodeIgniter4%203e51de2ba88f477aa8e233c1d72e0c13/image.png)

On déplace le env vers .env pour que les variables d’environnement soient définit directement dans le dossier du projet.

```bash
mv env .env
```

Maintenant, on va modifier ce fichier .env

```bash
# Décommenter et modifier la ligne
app.baseURL = "http://localhost:8080"

# Décommenter et modifier la ligne
CI_ENVIRONMENT = development
```

Maintenant que le projet est défini en développement, on peut accéder à l’outil de debug

![image.png](Manuel%20d%E2%80%99installation%20CodeIgniter4%203e51de2ba88f477aa8e233c1d72e0c13/image%201.png)

![image.png](Manuel%20d%E2%80%99installation%20CodeIgniter4%203e51de2ba88f477aa8e233c1d72e0c13/image%202.png)

Quand on fait une erreur dans notre code, on a un message d’erreur dans la navigateur

![image.png](Manuel%20d%E2%80%99installation%20CodeIgniter4%203e51de2ba88f477aa8e233c1d72e0c13/image%203.png)

![image.png](Manuel%20d%E2%80%99installation%20CodeIgniter4%203e51de2ba88f477aa8e233c1d72e0c13/image%204.png)

## c. Le développement

# 2.  Transition vers l’environnement de production

## a. Prérequis

Vous devez avoir une base de données accessible du serveur où vous lancerez l’application.

Prévoir le nom de domaine de votre application pour que les clients puissent accéder à votre service.

Rendre le serveur accessible de l’extérieur si c’est dans un objectif public ou le garder dans votre réseau local.

```bash
# Installation des différents composants

sudo dnf install php-cli
sudo dnf install phpunit composer
sudo dnf install php-mysqli

# Vérifier les installations avec les commandes suivantes

php -v # même version que votre environnement de dev
composer --version # même version que votre environnement de dev

# Installation de apache

sudo dnf install httpd
sudo systemctl start httpd
sudo systemctl enable httpd

```

## b. Installation

## c. Prérequis

Modifier les variables d’environnements de votre projet (.env)

```bash
# Modifier la ligne
app.baseURL = "http://votre-nom-de-domaine.fr"

# Modifier la ligne
CI_ENVIRONMENT = production
```

Modifier le php --ini

```bash
# Obtenir l'emplacement du fichier
php --ini 

# Commenter les lignes suivantes
opcache.preload=/path/to/preload.php
opcache.preload_user=myuser
```

```bash
# Placer vous dans le dossier de votre projet
# La commande suivante permet de retirer les dépendances 
composer update --no-dev
```

Apache config

déplacer le dossier du projet vers /var/www/nomApp

Apache pointe vers /var/www/nomApp

Modifier la section  mime_module pour que ça ressemble à ça

```php
<IfModule mime_module>
   AddType text/html .php .phps
</IfModule>
```