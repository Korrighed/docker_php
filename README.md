# PHP Docker Development Environment
Un environnement de développement PHP moderne avec Apache et PHP-FPM, optimisé pour le développement sous WSL2 et intégrable avec VS Code.

# Prérequis
- WSL2 (Windows Subsystem for Linux 2)
- Docker Desktop pour Windows configuré pour utiliser WSL2
- VS Code avec l'extension Remote Development obligatoire

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
```
```sh 
cd dockerphp
```
## Démarrez les conteneurs :
```sh
docker compose up -d
```
# Gestion des conteneurs
## monter le conteneur
```sh 
docker compose build --no-cache
```
Cette opération prends naturellement du temps. 
Vous téléchargez un système d'exploitation. 

## démarre le conteneur 
```sh
docker compose up -d
```
## Arrêter les conteneurs
```sh
docker compose down
```

# Vérifier l'intallation 
- Ouvrir un terminal dans le conteneur PHP
```sh 
docker compose exec -it php bash
```
- Vérifier l'installation de php
```sh 
php -v

exit
```

# Développement PHP
Le répertoire src/ est monté à la fois dans le conteneur Apache et PHP-FPM, ce qui signifie que:

Tous les fichiers que vous créez ou modifiez dans src/ sont automatiquement disponibles pour le serveur web
Vous pouvez modifier ces fichiers directement depuis l'hôte WSL2 ou depuis VS Code
Aucun redémarrage de conteneur n'est nécessaire après modification des fichiers PHP
Exemple:

## Créer un fichier phpinfo
```sh 
sudo nano info.php
```
Ajouter la liste de dépendance dans le fichier à la racine: 

```info.php
<?php phpinfo();
```
Accéder à http://localhost/info.php pour voir le résultat

## VScode 

Avec l'extension remote dev installer. 
En bas à gauche sur l'icône '><' => attache to running container =>  docker_php-php-1. 
Une nouvelle instance de wsl vas s'ouvrir dans le conteneur. 
Cette instance va s'ouvrir dans le repertoire home de la distribution linux du conteneur. 
Pour se diriger vers le répertoire servi.

```sh
cd /var/www/html/
```

```sh
code . 
```

Une nouvelle fenêtre vs code s'ouvre dans le répertoire distribué par apache2. 

A vous de créer de nouveaux repertoires qui seront servis à l'adresse. 

http://localhost/nomDuNouveauRepertoire

# Fonctionnalités
Apache 2.4 avec support PHP via proxy FastCGI
PHP 8.2 avec FPM pour des performances optimales
Volumes persistants pour le code source et la configuration
Intégration VS Code via l'extension Remote Development
Configuration modulaire facile à personnaliser
Optimisé pour WSL2 et Docker Desktop

# License
MIT

