
# Gestion de Congés

Ce projet est une application de gestion des congés développée avec Symfony. Elle permet de gérer les demandes de congés des employés, en incluant la soumission de demandes, le suivi des dates de début et de fin de congés, et le statut de chaque demande.

## Prérequis

- **PHP** 8.3.11 ou plus récent
- **Composer** pour la gestion des dépendances PHP
- **Symfony CLI** (facultatif, mais recommandé pour des commandes simplifiées)
- **Docker** et **Docker Compose** pour gérer la base de données et autres services
- **Node.js** et **npm** (si vous utilisez des outils front-end comme Webpack)

## Installation

### 1. Cloner le dépôt

Clonez le dépôt du projet sur votre machine locale :

```bash
git clone <URL_DU_DEPOT>
cd gestion_conges
```

### 2. Installation des dépendances PHP

Utilisez Composer pour installer les dépendances PHP du projet :

```bash
composer install
```

### 3. Configuration de l'environnement

Dupliquez le fichier `.env` pour créer un fichier `.env.local`, où vous pouvez définir vos variables d'environnement locales. Assurez-vous que les informations de connexion à la base de données sont correctement définies.

Voici un exemple de configuration `.env.local` :

```dotenv
APP_ENV=dev
APP_SECRET=your_secret_key
DATABASE_URL="postgresql://app:!ChangeMe!@database:5432/app"
```

### 4. Configuration de Docker pour la Base de Données

Le projet est configuré pour utiliser PostgreSQL via Docker. Vérifiez que le fichier `docker-compose.yaml` contient la configuration suivante pour la base de données :

```yaml
services:
  ###> doctrine/doctrine-bundle ###
  database:
    image: postgres:${POSTGRES_VERSION:-16}-alpine
    environment:
      POSTGRES_DB: ${POSTGRES_DB:-app}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-!ChangeMe!}
      POSTGRES_USER: ${POSTGRES_USER:-app}
    volumes:
      - database_data:/var/lib/postgresql/data:rw
  ###< doctrine/doctrine-bundle ###

volumes:
  ###> doctrine/doctrine-bundle ###
  database_data:
  ###< doctrine/doctrine-bundle ###
```

**Note** : Modifiez les valeurs des variables d’environnement `POSTGRES_DB`, `POSTGRES_PASSWORD`, et `POSTGRES_USER` selon vos besoins dans le fichier `.env.local` pour éviter d’éventuels conflits.

### 5. Lancer les Services avec Docker

Démarrez les conteneurs Docker avec la commande suivante :

```bash
docker-compose up -d
```

Cette commande lancera le service de base de données PostgreSQL défini dans le fichier `docker-compose.yaml`.

### 6. Créer la Base de Données et les Schémas

Utilisez les commandes Symfony suivantes pour créer la base de données et les tables nécessaires :

```bash
php bin/console doctrine:database:create
php bin/console doctrine:migrations:migrate
```

### 7. Installer les Dépendances Front-End (si applicable)

Si votre projet inclut des dépendances front-end, vous devrez également les installer :

```bash
npm install
npm run dev
```

### 8. Lancer le Serveur Symfony

Pour démarrer le serveur Symfony en mode de développement, exécutez :

```bash
symfony serve
```

Ou, si vous utilisez PHP directement :

```bash
php -S localhost:8000 -t public
```

Votre application devrait maintenant être accessible à l’adresse `http://localhost:8000`.

## Utilisation

L'application permet de gérer les demandes de congés, avec les fonctionnalités suivantes :

- Création de demandes de congés
- Gestion des dates de début et de fin de congés
- Suivi de l'état des demandes de congés (approuvé, en attente, rejeté)

## Tests

Pour exécuter les tests unitaires, utilisez la commande suivante :

```bash
php bin/phpunit
```

## Variables d'Environnement

Les principales variables d'environnement à configurer dans `.env.local` sont les suivantes :

- `APP_ENV`: environnement d'exécution (dev, prod, test)
- `APP_SECRET`: clé secrète pour la sécurité de l'application
- `DATABASE_URL`: URL de connexion à la base de données PostgreSQL

## Dépannage

Si vous rencontrez des problèmes lors du démarrage de la base de données, assurez-vous que :
- Docker est bien démarré.
- Les ports ne sont pas déjà utilisés par d'autres services.
- Les variables `POSTGRES_USER`, `POSTGRES_PASSWORD` et `POSTGRES_DB` sont correctement configurées.



