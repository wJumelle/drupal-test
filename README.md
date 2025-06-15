# Installation de la version 11 de Drupal
Début du projet 15/06/2025.
Pour l'installation de l'ensemble des pré-requis j'ai suivi le [**guide Drupal**](https://www.drupal.org/docs/getting-started/installing-drupal/install-drupal-using-ddev-for-local-development).

## Pré-requis

* php version >= 8.3
* DDEV donc Docker Desktop pour Windows

## Installation de Docker Desktop pour Windows

On récupère l'exe sur le site de [**Docker**](https://www.docker.com/products/docker-desktop/).
On décoche la case WSL2 (qui correspond à une install Windows sur une machine Linux).
Il faut activer le SVM pour AMD dans le BIOS pour que cela fonctionne.

## Installation de DDEV

Installation de la dernière [**release de DDEV**](https://github.com/ddev/ddev/releases).
Chacune des release de DDEV inclus des installers de Windows pour AMD64 (AMD) ou ARM64 (autres CPU), de la forme
suivante `ddev_windows_<architecture>_installer.<version>.exe`.

Ensuite on exécute toutes les commandes ci-dessous les unes après les autres afin d'être
sur que tout passe correctement.
Normalement, si c'est le premier projet que l'on initialise avec DDEV et Docker, Docker
va demander une permission et faire foirer dès la deuxième commande.

```
// On créé un dossier project et on s'y déplace pour effectuer les actions
mkdir project && cd project
ddev config --project-type=drupal11 --docroot=web
ddev start
ddev composer create-project "drupal/recommended-project:^11"
ddev composer require drush/drush
ddev drush site:install --account-name=admin --account-pass=admin -y
ddev launch

# or automatically log in with
ddev launch $(ddev drush uli)
```

Dans notre Docker on se retrouve alors avec :
* ddev-ssh-agent
* ddev-project
* ddev-router

## Redémarrage du projet à chaque lancement de session de développement

```
cd project
ddev start
```

`ddev start` va :
* Lancer les conteneurs Docker (web, db, etc.)
* Donner l'URL de développement (ex: https://project.ddev.site)

Puis on exécute `ddev launch` pour ouvrir le site affiché dans le terminal.

## Commandes utiles

* `ddev start` => redémarrer le projet
* `ddev stop` => arrêter le projet du dossier en cours
* `ddev poweroff`  => arrêter tous les projets DDEV en cours
* `ddev ssh` => accès shell dans le conteneur web
* `ddev drush cr` => vider les caches Drupal
* `ddev drush uli` => connexion admin rapide
* `composer update` => mise à jour des dépendances

## Structure du projet sous /project

* `.ddev/` => configuration DDEV (PHP version, MySQL, etc.)
* `web/` => racine publique Drupal (avec index.php)
* `composer.json` => dépendances PHP/Drupal

## Environnement local

Il faudra créer un fichier `settings.local.php` en suivant le chemin `project/web/sites/default/`.
