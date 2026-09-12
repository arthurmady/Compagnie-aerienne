# Compagnie Aérienne

Application web de gestion pour une compagnie aérienne. Permet de gérer les avions, les vols, les billets, le personnel et la maintenance via une interface HTML/PHP connectée à une base PostgreSQL.

## Table des matières

- [Fonctionnalités](#fonctionnalités)
- [Stack technique](#stack-technique)
- [Prérequis](#prérequis)
- [Installation](#installation)
- [Structure du projet](#structure-du-projet)
- [Schéma de la base de données](#schéma-de-la-base-de-données)
- [Utilisation](#utilisation)
- [Auteurs](#auteurs)

## Fonctionnalités

### Avions
- Recherche par référence, type ou nombre de sièges
- Liste de tous les avions enregistrés

### Vols
- Recherche par ville de départ/arrivée, date ou référence
- Création d'un vol avec sélection d'avion, responsable et équipage
- Modification des horaires et dates de départ/arrivée
- Suppression de vol

### Billets
- Recherche par ville de départ/arrivée, date ou vol
- Ajout d'un billet avec sélection du vol, passager et place
- Annulation de billet

### Personnel
- Recherche par numéro de poste, nom, prénom ou fonction
- Ajout et suppression d'employés

### Maintenance
- Recherche par date, référence, avion ou responsable
- Ajout d'une intervention de maintenance avec équipe
- Suppression d'intervention

## Stack technique

| Composant | Technologie |
|-----------|-------------|
| Langage | PHP |
| Front-end | HTML, CSS inline |
| Base de données | PostgreSQL |
| Pilote DB | Extension `pgsql` PHP (`pg_connect`, `pg_query`) |
| Serveur | Apache / Nginx + PHP |

## Prérequis

- PHP 7.x+ avec l'extension `pgsql` activée
- PostgreSQL 9.x+
- Serveur web (Apache, Nginx ou `php -S` pour le développement)

## Installation

1. **Cloner le dépôt**

```bash
git clone https://github.com/votre-user/Compagnie-aerienne.git
cd Compagnie-aerienne
```

2. **Créer la base de données PostgreSQL**

```sql
CREATE DATABASE gestion_compagnie;
```

3. **Exécuter le script de création des tables**

```bash
psql -U votre_user -d gestion_compagnie -f codeSQL.sql
```

4. **Configurer la connexion**

Modifier les identifiants de connexion dans chaque fichier PHP. La ligne à modifier est :

```php
pg_connect("host=serveur-etu.polytech-lille.fr user=amady port=5432 password=postgres dbname=gestion_compagnie")
```

Remacer les valeurs par vos propres credentials.

5. **Lancer le serveur**

```bash
php -S localhost:8000
```

6. **Accéder à l'application**

Ouvrir `http://localhost:8000/index.php` dans un navigateur.

## Structure du projet

```
Compagnie-aerienne/
├── index.php                  # Tableau de bord principal (recherche et affichage)
│
├── -- Vols --
├── nouveauVol.php             # Créer un vol
├── modifierVol.php            # Modifier un vol
├── supprimerVol.php           # Supprimer un vol
│
├── -- Billets --
├── ajouterBillet.php          # Ajouter un billet
├── annulerBillet.php          # Annuler un billet
│
├── -- Personnel --
├── ajouterEmployé.php         # Ajouter un employé
├── supprimerEmployé.php       # Supprimer un employé
│
├── -- Maintenance --
├── ajouterMaintenance.php     # Ajouter une intervention
├── supprimerMaintenance.php   # Supprimer une intervention
│
├── -- Base de données --
├── codeSQL.sql                # Script DDL PostgreSQL (schéma officiel)
├── codeSQL                    # Brouillon DDL (ancienne version)
├── test.sql                   # Table junction Billet_vol (non utilisée)
│
├── -- Maquette --
├── modele.html                # Prototype HTML (wireframe)
│
└── README.md
```

## Schéma de la base de données

Le schéma comporte **9 tables** :

```
Type ──1:N── Avion ──1:N── Vol ──1:N── Billet
                │            │
                │            ├──1:N── Membre_équipage ──N:1── Employé
                │
                └──1:N── Maintenance ──1:N── Equipe_maintenance ──N:1── Employé
```

| Table | Description |
|-------|-------------|
| `Type` | Type d'avion (Boeing, Airbus, etc.) |
| `Avion` | Avion avec référence, type, sièges, date de mise en service |
| `Fonction` | Poste / rôle de l'employé |
| `Employé` | Employé avec numéro de poste, nom, prénom, fonction |
| `Vol` | Vol avec trajet, dates, horaires, avion affecté |
| `Billet` | Billet avec passager, siège, vol associé |
| `Maintenance` | Intervention de maintenance sur un avion |
| `Membre_équipage` | Table de jonction : équipage d'un vol |
| `Equipe_maintenance` | Table de jonction : équipe d'une maintenance |

Les clés étrangères utilisent `ON DELETE CASCADE` pour la suppression en cascade.

## Utilisation

L'application se compose d'un **tableau de bord unique** (`index.php`) qui permet de :

1. **Rechercher** une entité (avion, vol, billet, employé, maintenance) selon différents critères
2. **Afficher** les résultats dans des tableaux HTML
3. **Naviguer** vers les pages de création, modification ou suppression

Chaque opération d'écriture (CRUD) dispose de sa propre page PHP dédiée.

## Auteurs

Projet réalisé dans le cadre du cours de bases de données à **Polytech Lille**.

---

> **Note** : L'application ne dispose pas d'authentification. Toutes les pages sont accessibles sans restriction. En environnement de production, il serait nécessaire d'ajouter un système d'authentification et de protéger les routes d'écriture.
