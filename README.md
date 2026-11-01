# 🎧 Symfony Order Form

Application web **Symfony 7.3** de commande d'un casque audio premium : un formulaire construit avec les **Form Types** de Symfony, validé côté serveur, avec une interface **Bootstrap 5.3** responsive et un environnement **Docker** prêt à l'emploi.

![Symfony](https://img.shields.io/badge/Symfony-7.3-000000?logo=symfony&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-%E2%89%A5%208.2-777BB4?logo=php&logoColor=white)
![Twig](https://img.shields.io/badge/Twig-3-BACF29)
![Bootstrap](https://img.shields.io/badge/Bootstrap-5.3-7952B3?logo=bootstrap&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-Doctrine%20ORM%203-4479A1?logo=mysql&logoColor=white)

> TP Symfony – Formulaire simple (projet pédagogique).

---

## 📑 Sommaire

- [Aperçu](#-aperçu)
- [Fonctionnalités](#-fonctionnalités)
- [Stack technique](#-stack-technique)
- [Prérequis](#-prérequis)
- [Installation](#-installation)
- [Commandes utiles](#-commandes-utiles)
- [Structure du projet](#-structure-du-projet)
- [Compétences mises en pratique](#-compétences-mises-en-pratique)
- [Pistes d'amélioration](#-pistes-damélioration)
- [Auteur](#-auteur)

---

## 👀 Aperçu

La page de commande présente le produit (image, prix, description, caractéristiques) et un formulaire permettant de choisir la **quantité** et la **couleur** du casque. À la soumission, les données sont validées côté serveur, un **message flash** confirme la commande et l'utilisateur est **redirigé** (pattern Post/Redirect/Get).

<!-- Ajoutez une capture d'écran : placez l'image dans docs/screenshot.png puis décommentez la ligne suivante -->
<!-- ![Aperçu de l'application](docs/screenshot.png) -->

## ✨ Fonctionnalités

### 1. Formulaire Symfony personnalisé
- `ProductOrderType` construit avec le **Form Builder**
- Champs : **quantité** (`IntegerType`) et **couleur** (`ChoiceType`)
- Bouton de soumission stylisé avec Bootstrap

### 2. Interface utilisateur
- Design **responsive** (Bootstrap 5.3)
- Image du produit, prix, description et caractéristiques
- Formulaire intégré de façon cohérente avec la page

### 3. Gestion des soumissions
- Traitement de la requête `POST` dans le contrôleur
- **Validation côté serveur** des données saisies
- Messages flash de confirmation
- Redirection après soumission réussie

### 4. Personnalisation Twig
- Utilisation des helpers de formulaire Twig
- Héritage du template de base (`extends`)
- Bootstrap chargé via CDN

## 🧰 Stack technique

| Domaine | Technologies |
|---|---|
| Backend | PHP ≥ 8.2, Symfony 7.3 (Form, Validator, Twig, Security, Mailer, Messenger…) |
| Base de données | MySQL, Doctrine ORM 3, Doctrine Migrations |
| Frontend | Twig, Bootstrap 5.3, AssetMapper, Stimulus, Symfony UX Turbo |
| Tests | PHPUnit 12, BrowserKit, CssSelector |
| Outils dev | Symfony Maker Bundle, Web Profiler, Debug Bundle, Xdebug |
| Infra | Docker Compose (Nginx, PHP-FPM, MySQL, phpMyAdmin), Makefile |

## ✅ Prérequis

- [Docker](https://docs.docker.com/get-docker/) et Docker Compose
- `make` (optionnel, pour utiliser les raccourcis du `Makefile`)
- Git

> PHP et Composer n'ont pas besoin d'être installés en local : tout s'exécute dans les conteneurs.

## 🚀 Installation

```bash
# 1. Cloner le dépôt
git clone https://github.com/CharafAZIZI/symfony-order-form.git
cd symfony-order-form

# 2. Démarrer les conteneurs
docker compose up -d

# 3. Installer les dépendances PHP
docker compose exec php composer install

# 4. Créer la configuration locale (environnement dev)
cp .env.dev .env.dev.local

# 5. Créer la base de données et appliquer les migrations
docker compose exec php php bin/console doctrine:database:create --if-not-exists
docker compose exec php php bin/console doctrine:migrations:migrate --no-interaction
```

L'application est ensuite disponible sur **http://localhost**.

### Services exposés

| Service | URL / Port | Rôle |
|---|---|---|
| Application (Nginx) | http://localhost (`80`) · `443` | Serveur web |
| phpMyAdmin | http://localhost:9002 | Administration de la base |
| MySQL | `localhost:33060` | Accès direct à la base |
| Xdebug | `9003` (interne au conteneur `php`) | Débogage pas à pas |

> ⚠️ Les identifiants MySQL présents dans `compose.yaml` (`root` / `secret`) sont **réservés au développement local**. Ne les réutilisez jamais en production.

## 🛠️ Commandes utiles

Des raccourcis sont disponibles via le `Makefile` :

| Commande | Description |
|---|---|
| `make start` | Démarre les conteneurs (`docker compose up -d`) |
| `make down` | Arrête et supprime les conteneurs |
| `make down-volumes` | Idem, en supprimant aussi les volumes (⚠️ efface la base) |
| `make install-packages` | Lance `composer install` dans le conteneur PHP |
| `make create-database` | Crée la base de données |
| `make drop-database` | Supprime la base de données |
| `make enter service=php` | Ouvre un shell dans un conteneur |
| `make list` | Liste les conteneurs en cours |

> ⚠️ `make install` et `make stop` exécutent `docker stop $(docker ps -aq)`, qui arrête **tous** les conteneurs de la machine, pas seulement ceux de ce projet. Préférez `make start` / `make down` si vous avez d'autres projets Docker en cours.

Commandes Symfony courantes :

```bash
# Vider le cache
docker compose exec php php bin/console cache:clear

# Lancer les tests
docker compose exec php php bin/phpunit
```

## 📁 Structure du projet

```
symfony-order-form/
├── assets/          # JS / CSS (AssetMapper, Stimulus)
├── bin/             # Console Symfony et exécutables
├── config/          # Configuration de l'application
├── infra/           # Dockerfiles et configuration Nginx / PHP-FPM / Xdebug
├── migrations/      # Migrations Doctrine
├── public/          # Point d'entrée web (index.php)
├── src/             # Code applicatif (contrôleurs, formulaires, ...)
├── templates/       # Vues Twig
├── tests/           # Tests PHPUnit
├── translations/    # Fichiers de traduction
├── compose.yaml     # Services Docker
├── Makefile         # Raccourcis de développement
└── composer.json    # Dépendances PHP
```

## 🎓 Compétences mises en pratique

- Maîtrise des formulaires Symfony (**Form Types**, Form Builder)
- Personnalisation des vues avec **Twig**
- Intégration de **Bootstrap** dans un projet Symfony
- Gestion des **validations** de formulaire
- Traitement d'une requête **POST** et pattern Post/Redirect/Get
- Structuration d'un petit projet Symfony et conteneurisation avec **Docker**

## 🔭 Pistes d'amélioration

- [ ] Persister les commandes en base avec une entité Doctrine `Order`
- [ ] Ajouter des contraintes de validation explicites (quantité min/max) avec messages traduits
- [ ] Envoyer un e-mail de confirmation via Symfony Mailer
- [ ] Ajouter des tests fonctionnels du formulaire (PHPUnit + BrowserKit)
- [ ] Mettre en place une CI GitHub Actions (tests + analyse statique)
- [ ] Ajouter une licence et des captures d'écran

## 👤 Auteur

**Charaf AZIZI** : étudiant en génie informatique (EHEIM, Oujda)

- GitHub : [@CharafAZIZI](https://github.com/CharafAZIZI)
- LinkedIn : [charaf-azizi](https://www.linkedin.com/in/charaf-azizi-4ba398351/)
