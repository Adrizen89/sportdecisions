# Cas Pratique - SPORTS DECISIONS

This template should help get you started developing with Vue 3 in Vite.

## 1️⃣ Introduction

Ce projet a pour objectif de créer une vue détaillée "End of Contracts" pour un club sportif. La vue affiche la date de fin de contrat de chaque joueur, ainsi que les éventuelles options pour prolonger le contrat. Le projet est réalisé en utilisant Vue.js, et doit être responsive pour s'adapter automatiquement aux différentes tailles d'écran.

## 2️⃣ Installation

### Prérequis

- Node.js et npm doivent être installés sur votre machine.
- Vue CLI (optionnel pour certaines configurations).

### Etapes d'installation

Clonez le dépôt du projet :

```sh
git clone https://github.com/Adrizen89/sportdecisions.git
```

```sh
cd endofcontracts
```

Installez les dépendances :

```sh
npm install
```

Lancez le serveur de développement :

```sh
npm run serve
```

## 3️⃣ Utilisation

### Structure du Projet

- PlayerInfo.vue : Composant réutilisable pour afficher les informations d'un joueur.

- HeadingTable.vue : Composant pour gérer les en-têtes du tableau, y compris le tri et la navigation des colonnes.

- views/
  - EndOfContracts.vue : Vue principale contenant le tableau des joueurs avec leurs dates de fin de contrat.

### Fonctionnalités Principales

- Affichage Responsive : Le tableau s'adapte automatiquement à la taille de l'écran. Sur les écrans de moins de 768px, une seule colonne est affichée à la fois, et les autres colonnes peuvent être visualisées en utilisant les flèches de navigation.

- Tri des Colonnes : Les colonnes peuvent être triées en ordre ascendant ou descendant en cliquant sur les en-têtes.

- Gestion des Données : Les données des joueurs sont fournies dans un fichier JavaScript et sont affichées dans le tableau avec un format de date spécifique (jj-mm-aaaa).

### Configuration de la Vue

Le projet utilise une seule vue pour gérer l'affichage sur différentes tailles d'écran. Les breakpoints de Bootstrap sont utilisés pour masquer ou afficher les colonnes selon la taille de l'écran.

### Navigation entre les Colonnes

Sur les appareils mobiles (écran < 768px), vous pouvez naviguer entre les colonnes à l'aide des flèches positionnées dans les en-têtes. Les flèches permettent de faire défiler les colonnes pour afficher les informations pertinentes.
