---
title: "JavaScript Optimisation"
description: "Techniques avancées pour réduire, optimiser et contrôler le JavaScript côté client dans des applications modernes et Next.js."
updated_at: "2026-03-08"
tags:
  - Performance
  - JavaScript
  - Optimisation
  - Next.js
  - RSC
---

# JavaScript Optimisation

Le JavaScript est l’un des principaux facteurs de lenteur sur le web moderne.  
Réduire, contrôler et optimiser le JS côté client est essentiel pour améliorer :

- l’INP
- le TTI
- le TBT
- la consommation mémoire
- la fluidité générale

Ce guide regroupe les techniques avancées pour optimiser le JavaScript dans des projets front-end modernes, avec un focus particulier sur Next.js.

## Sommaire

1. [Principes généraux](#1-principes-généraux)
2. [Réduire la quantité de JS](#2-réduire-la-quantité-de-js)
3. [Code splitting](#3-code-splitting)
4. [Dynamic imports](#4-dynamic-imports)
5. [Tree-shaking](#5-tree-shaking)
6. [Optimisation des dépendances](#6-optimisation-des-dépendances)
7. [React Server Components](#7-react-server-components)
8. [Hydration & streaming](#8-hydration--streaming)
9. [Analyse des bundles](#9-analyse-des-bundles)
10. [Checklist JS Optimisation](#10-checklist-js-optimisation)

---

## 1. Principes généraux

- Le meilleur JS est celui qui **n’est pas envoyé au client**.
- Le JS doit être **chargé en dernier recours**, uniquement si nécessaire.
- Le JS doit être **découpé**, **lazy-loadé**, **minifié**, **arborisé**.
- Le JS doit être **court**, **stateless**, **prévisible**.
- Le JS doit être **déporté côté serveur** dès que possible (RSC).

---

## 2. Réduire la quantité de JS

### 2.1. Supprimer les dépendances inutiles

- lodash → préférer les fonctions natives
- moment.js → préférer `Intl.DateTimeFormat`
- axios → préférer `fetch`
- jQuery → inutile en React/Next

### 2.2. Remplacer les librairies lourdes

- chart.js → recharts / visx
- swiper → embla
- date-fns → dayjs

### 2.3. Éviter les polyfills globaux

Utiliser des polyfills ciblés uniquement si nécessaire.

---

## 3. Code splitting

Découper le JS en chunks indépendants pour éviter de charger tout le bundle d’un coup.

### Exemple Next.js :

```tsx
import dynamic from "next/dynamic";

const HeavyComponent = dynamic(() => import("./HeavyComponent"));
```

````

### Bonnes pratiques

- découper les pages lourdes
- découper les composants rarement utilisés
- découper les dashboards, graphiques, éditeurs

---

## 4. Dynamic imports

Permet de charger un module uniquement quand il est nécessaire.

### Exemple avec options :

```tsx
const Editor = dynamic(() => import("./Editor"), {
  ssr: false,
  loading: () => <p>Chargement…</p>,
});
```

### À utiliser pour :

- composants interactifs
- librairies lourdes
- composants non critiques

---

## 5. Tree-shaking

Supprime le code mort (dead code elimination).

### Bonnes pratiques

- utiliser des imports nommés
- éviter les imports globaux
- éviter les librairies non tree-shakeables

### Exemple :

✔️ `import { debounce } from "lodash-es"`
✖️ `import _ from "lodash"`

---

## 6. Optimisation des dépendances

### 6.1. Préférer les librairies ESM

Elles sont plus faciles à tree-shaker.

### 6.2. Vérifier le poids des dépendances

Utiliser :

- bundlephobia
- npm trends
- next-bundle-analyzer

### 6.3. Remplacer les dépendances lourdes

Exemples :

- moment.js → dayjs
- lodash → lodash-es ou natif
- chart.js → visx

---

## 7. React Server Components

### 7.1. Pourquoi c’est important ?

Les RSC permettent de **supprimer du JS côté client**.

### 7.2. Bonnes pratiques

- mettre la logique côté serveur
- éviter `"use client"`
- découper les composants client au strict minimum
- utiliser les RSC pour :
  - fetch
  - transformation de données
  - rendu statique

---

## 8. Hydration & streaming

### 8.1. Hydration

La hydration est coûteuse en JS.

Réduire la hydration en :

- utilisant RSC
- réduisant les composants client
- évitant les states inutiles

### 8.2. Streaming

Le streaming permet d’afficher la page avant que tout le JS soit prêt.

---

## 9. Analyse des bundles

### 9.1. Next.js Bundle Analyzer

```js
const withBundleAnalyzer = require("@next/bundle-analyzer")({
  enabled: process.env.ANALYZE === "true",
});
module.exports = withBundleAnalyzer({});
```

### 9.2. À surveiller

- poids du bundle principal
- poids des chunks dynamiques
- dépendances lourdes
- duplication de modules

---

## 10. Checklist JS Optimisation

- [ ] JS minimal côté client
- [ ] Aucun `"use client"` inutile
- [ ] Dynamic imports pour les composants lourds
- [ ] Tree-shaking activé
- [ ] Librairies ESM privilégiées
- [ ] Analyse du bundle effectuée
- [ ] RSC maximisés
- [ ] Hydration réduite
- [ ] Aucun polyfill global inutile
- [ ] Aucun module dupliqué
````
