---
title: "Next.js & SEO"
description: "Bonnes pratiques SEO pour les projets Next.js : structure, rendu, navigation, sémantique, indexation et liens vers les optimisations techniques."
updated_at: "2026-03-08"
tags:
  - SEO
  - Next.js
  - Architecture
  - Content
---

# Next.js & SEO

Next.js offre un environnement idéal pour produire des pages rapides, structurées et SEO‑friendly.  
Ce document couvre **uniquement les aspects SEO** : structure, rendu adapté, navigation, sémantique, indexation, bonnes pratiques éditoriales.

Pour les optimisations techniques (rendu, JS, images, caching, Core Web Vitals), voir la section **Références techniques** en bas de page.

## Sommaire

1. [Principes SEO avec Next.js](#1-principes-seo-avec-nextjs)
2. [Structure & architecture des pages](#2-structure--architecture-des-pages)
3. [Rendu & SEO](#3-rendu--seo)
4. [Navigation & maillage interne](#4-navigation--maillage-interne)
5. [Sémantique & contenu](#5-sémantique--contenu)
6. [Indexation & accessibilité](#6-indexation--accessibilité)
7. [Erreurs fréquentes](#7-erreurs-fréquentes)
8. [Références techniques](#8-références-techniques)

---

## 1. Principes SEO avec Next.js

Next.js facilite le SEO grâce à :

- une structure claire
- un rendu flexible (SSR, SSG, ISR, RSC)
- une gestion propre des routes
- une bonne intégration des métadonnées
- une performance généralement supérieure à la moyenne

Le rôle du SEO est de **choisir la bonne stratégie de rendu**, de structurer le contenu, et de garantir une indexation propre.

---

## 2. Structure & architecture des pages

### 2.1. Une page = une intention claire

Chaque page doit répondre à une intention utilisateur unique.

### 2.2. URL propres et stables

- éviter les paramètres inutiles
- privilégier les slugs descriptifs
- éviter les changements fréquents d’URL

### 2.3. Arborescence logique

Exemples :

```

/produits/
/produits/guitares/
/produits/guitares/stratocaster/

```

### 2.4. Layouts cohérents

Les layouts doivent renforcer la structure du site, pas la complexifier.

---

## 3. Rendu & SEO

### 3.1. Le SEO ne dicte pas la stratégie de rendu

Le SEO s’adapte au rendu choisi, mais ne le détermine pas.

### 3.2. Règles SEO générales

- les pages **statiques** sont idéales pour le SEO
- les pages **dynamiques** doivent rester rapides
- les pages **personnalisées** doivent être indexables uniquement si pertinentes
- les pages **non indexables** doivent être explicitement marquées (`noindex`)

### 3.3. RSC (React Server Components)

Les RSC sont excellents pour le SEO car ils réduisent le JS client, mais ce point est **technique** → voir `/performance/rendering.md`.

---

## 4. Navigation & maillage interne

### 4.1. Liens internes explicites

- utiliser des ancres descriptives
- éviter “cliquez ici”
- privilégier les liens contextuels

### 4.2. Navigation cohérente

- menus clairs
- fil d’Ariane
- pagination propre

### 4.3. Next.js & navigation

La navigation client-side est rapide, mais ne change rien au SEO si les pages sont bien structurées.

---

## 5. Sémantique & contenu

### 5.1. Titres & headings

- un seul `<h1>` par page
- structure hiérarchique claire
- titres descriptifs

### 5.2. Métadonnées

- title
- description
- open graph
- twitter cards

### 5.3. Contenu

- clair
- utile
- structuré
- riche sémantiquement

---

## 6. Indexation & accessibilité

### 6.1. Conditions d’indexation

- page accessible
- contenu visible sans interaction
- pas de blocage robots.txt
- pas de `noindex` involontaire

### 6.2. Accessibilité

- texte alternatif pour les images
- labels pour les formulaires
- structure sémantique propre

### 6.3. Redirections

- éviter les chaînes de redirections
- éviter les redirections inutiles

---

## 7. Erreurs fréquentes

- pages dynamiques non indexables
- contenu rendu uniquement côté client
- absence de métadonnées
- structure de headings incorrecte
- URLs non descriptives
- navigation trop complexe
- absence de maillage interne

---

## 8. Références techniques

Pour les optimisations techniques liées à Next.js :

- Rendu (SSR, SSG, ISR, RSC) → `/performance/rendering.md`
- Optimisation du JS → `/performance/js-optimisation.md`
- Optimisation des images → `/performance/images.md`
- Caching & revalidation → `/performance/caching.md`
- Core Web Vitals → `/performance/core-web-vitals.md`
