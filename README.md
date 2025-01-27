# Docker WordPress avec PostgreSQL

Ce projet permet de déployer un environnement WordPress utilisant PostgreSQL comme base de données, le tout conteneurisé avec Docker. Il inclut une configuration automatisée pour les clés de sécurité WordPress.

## Structure du projet

```
dock-post-press/
├── docker-compose.yml          # Fichier de configuration Docker Compose
├── .dockerignore               # Fichier pour ignorer des fichiers dans Docker
├── .env                        # Variables d'environnement pour Docker Compose
└── wordpress/
    └── Dockerfile              # Dockerfile personnalisé pour WordPress
```

## Fichiers clés

### `docker-compose.yml`
Définit les services suivants :
- **wordpress-service** : Conteneur WordPress avec une configuration personnalisée.
- **postgres-service** : Conteneur PostgreSQL pour la base de données.

### `.env`
Contient les variables d'environnement nécessaires pour Docker Compose, telles que :
- `WORDPRESS_DB_NAME`, `WORDPRESS_DB_USER`, `WORDPRESS_DB_PASSWORD`, `WORDPRESS_DB_HOST`
- `POSTGRES_DB`, `POSTGRES_USER`, `POSTGRES_PASSWORD`

### `wordpress/Dockerfile`
Dockerfile personnalisé pour WordPress qui :
1. Installe les dépendances nécessaires (git, curl, libpq-dev).
2. Configure PostgreSQL pour WordPress.
3. Télécharge et configure `wp-config.php` avec les clés de sécurité générées dynamiquement.

## Prérequis

- Docker et Docker Compose installés sur votre machine.
- Un accès à Internet pour télécharger les images Docker et les dépendances.

## Installation

1. Clonez ce dépôt :
   ```bash
   git clone https://github.com/votre-utilisateur/dock-post-press.git
   cd dock-post-press
   ```

2. Configurez les variables d'environnement dans le fichier `.env` :
   ```bash
   cp .env.example .env
   nano .env  # Modifiez les valeurs selon vos besoins
   ```

3. Démarrez les conteneurs :
   ```bash
   docker-compose up -d
   ```

4. Accédez à WordPress :
   - Ouvrez votre navigateur à l'adresse `http://localhost:8080` (ou le port spécifié dans `docker-compose.yml`).

## Commandes utiles

- **Arrêter les conteneurs** :
  ```bash
  docker-compose down
  ```

- **Reconstruire les conteneurs** :
  ```bash
  docker-compose up -d --build
  ```

- **Supprimer les conteneurs et les volumes** :
  ```bash
  docker-compose down -v
  ```

- **Voir les logs des conteneurs** :
  ```bash
  docker-compose logs -f
  ```

## Personnalisation

- Pour modifier la configuration de WordPress, éditez le fichier `wordpress/Dockerfile`.
- Pour changer les paramètres de la base de données, et du port modifiez le fichier `.env`.

## Licence
Ce projet est sous licence MIT. Voir le fichier [LICENSE](LICENSE) pour plus de détails.
