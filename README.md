# PHP Docker Development Environment
Un environnement de développement PHP moderne avec Apache et PHP-FPM, optimisé pour le développement sous WSL2 et intégrable avec VS Code.

# Prérequis
- WSL2 (Windows Subsystem for Linux 2)
- Docker Desktop pour Windows configuré pour utiliser WSL2
- VS Code avec l'extension Remote Development (recommandé)
# Structure du projet
```
dockerphp/
├── .devcontainer/             # Configuration pour VS Code Remote Development
│   ├── devcontainer.json      # Configuration de l'environnement de développement
│   └── postCreateCommand.sh   # Scripts exécutés lors de la création du conteneur
├── apache2/                   # Configuration Apache
│   ├── Dockerfile             # Image Docker pour Apache
│   └── apache.conf            # Configuration du serveur web Apache
├── php/                       # Configuration PHP
│   └── Dockerfile             # Image Docker pour PHP-FPM
├── src/                       # Vos fichiers source PHP
│   └── index.php              # Point d'entrée de l'application
├── .bash_history              # Historique des commandes (persistant)
└── docker-compose.yml         # Configuration des services Docker
```
# Installation et démarrage
## Clonez ce dépôt :
```sh
git clone https://github.com/username/dockerphp.git

cd dockerphp
```
## Démarrez les conteneurs :

```sh
docker compose up -d
```

Vérifiez que tout fonctionne correctement en accédant à http://localhost dans votre navigateur

# Développement avec VS Code
- Option 1 : Développement à distance dans le conteneur
Ouvrez le dossier du projet dans VS Code
Cliquez sur l'icône verte dans le coin inférieur gauche
Sélectionnez "Reopen in Container"
VS Code redémarre et se connecte à l'intérieur du conteneur PHP

- Option 2 : Développement local avec le volume monté
Ouvrez le dossier src dans VS Code
Modifiez les fichiers - ils seront automatiquement synchronisés avec le conteneur
Accédez à http://localhost pour voir les changements
Commandes utiles

# Gestion des conteneurs
## Démarrer les conteneurs
docker compose up -d

## Arrêter les conteneurs
docker compose down

## Reconstruire les images (après modification des Dockerfiles)
docker compose build --no-cache

Accès aux conteneurs
## Ouvrir un terminal dans le conteneur PHP
docker compose exec php bash

## Exécuter une commande PHP
docker compose exec php php -v

## Vérifier les logs
docker compose logs -f

Développement PHP
Le répertoire src/ est monté à la fois dans le conteneur Apache et PHP-FPM, ce qui signifie que:

Tous les fichiers que vous créez ou modifiez dans src/ sont automatiquement disponibles pour le serveur web
Vous pouvez modifier ces fichiers directement depuis l'hôte WSL2 ou depuis VS Code
Aucun redémarrage de conteneur n'est nécessaire après modification des fichiers PHP
Exemple:

# Créer un fichier phpinfo
echo '<?php phpinfo();' > src/info.php

# Accéder à http://localhost/info.php pour voir le résultat

Personnalisation
Ajout d'extensions PHP
Modifiez le fichier php/Dockerfile et ajoutez les extensions requises :

# Exemple: ajouter l'extension intl
RUN apt-get update && apt-get install -y libicu-dev \
    && docker-php-ext-install intl

Configuration d'Apache
Modifiez le fichier apache2/apache.conf pour ajuster les paramètres du serveur web.

Variables d'environnement
Ajoutez des variables d'environnement dans docker-compose.yml :

services:
  php:
    # Configuration existante...
    environment:
      - APP_ENV=development
      - DB_HOST=mysql

Dépannage
Problèmes de permission
Si vous rencontrez des problèmes de permission dans les fichiers créés à l'intérieur du conteneur :

# Dans le répertoire du projet
sudo chown -R $(id -u):$(id -g) src/

Ports déjà utilisés
Si le port 80 est déjà utilisé sur votre machine :

# Dans docker-compose.yml
services:
  apache:
    # Configuration existante...
    ports:
      - "8080:80"  # Utilisez le port 8080 à la place

Fonctionnalités
Apache 2.4 avec support PHP via proxy FastCGI
PHP 8.2 avec FPM pour des performances optimales
Volumes persistants pour le code source et la configuration
Intégration VS Code via l'extension Remote Development
Configuration modulaire facile à personnaliser
Optimisé pour WSL2 et Docker Desktop
License
MIT