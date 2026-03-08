---
title: "Rendering Strategies"
description: "Guide technique complet sur les stratégies de rendu : SSR, SSG, ISR, CSR, RSC, streaming et hydration dans Next.js."
updated_at: "2026-03-08"
tags:
  - Performance
  - Rendering
  - Next.js
  - RSC
  - SSR
  - Hydration
---

# Rendering Strategies

Le rendu est un pilier fondamental de la performance web.  
Chaque stratégie (SSR, SSG, ISR, CSR, RSC) a un impact direct sur :

- le temps de chargement
- la quantité de JavaScript envoyée
- la charge serveur
- la fluidité de l’expérience
- la stabilité du rendu

Ce guide couvre les stratégies de rendu modernes, leur fonctionnement interne, leurs avantages et leurs limites.

## Sommaire

1. [Principes généraux](#1-principes-généraux)
2. [CSR (Client-Side Rendering)](#2-csr-client-side-rendering)
3. [SSR (Server-Side Rendering)](#3-ssr-server-side-rendering)
4. [SSG (Static Site Generation)](#4-ssg-static-site-generation)
5. [ISR (Incremental Static Regeneration)](#5-isr-incremental-static-regeneration)
6. [RSC (React Server Components)](#6-rsc-react-server-components)
7. [Hydration](#7-hydration)
8. [Streaming & Suspense](#8-streaming--suspense)
9. [Choisir la bonne stratégie](#9-choisir-la-bonne-stratégie)
10. [Checklist Rendering](#10-checklist-rendering)

---

## 1. Principes généraux

- Le rendu doit être **adapté au type de page**.
- Le rendu doit minimiser le **JS côté client**.
- Le rendu doit réduire la **latence perçue**.
- Le rendu doit être **prévisible** et **stable**.
- Le rendu doit être **mesuré** (DevTools, WebPageTest, Vercel).

---

## 2. CSR (Client-Side Rendering)

### 2.1. Fonctionnement

- HTML minimal envoyé
- JS télécharge le reste
- React hydrate la page
- Rendu complet côté client

### 2.2. Avantages

- interactions riches
- logique client avancée

### 2.3. Inconvénients

- lourd en JS
- lent au premier chargement
- mauvais pour LCP / INP

### 2.4. À utiliser pour :

- dashboards
- interfaces complexes
- pages privées

---

## 3. SSR (Server-Side Rendering)

### 3.1. Fonctionnement

- HTML généré à chaque requête
- JS hydrate ensuite

### 3.2. Avantages

- contenu toujours frais
- bon pour les pages dynamiques

### 3.3. Inconvénients

- dépend du serveur
- latence plus élevée
- coûteux en ressources

### 3.4. À utiliser pour :

- pages personnalisées
- données très fraîches

---

## 4. SSG (Static Site Generation)

### 4.1. Fonctionnement

- HTML généré au build
- servi via CDN

### 4.2. Avantages

- ultra rapide
- très stable
- peu coûteux

### 4.3. Inconvénients

- contenu figé
- rebuild nécessaire

### 4.4. À utiliser pour :

- pages statiques
- landing pages
- documentation

---

## 5. ISR (Incremental Static Regeneration)

### 5.1. Fonctionnement

- SSG + revalidation automatique
- régénération en arrière-plan

### Exemple :

```ts
export const revalidate = 60;
```

````

### 5.2. Avantages

- contenu frais
- performances maximales
- pas de rebuild complet

### 5.3. À utiliser pour :

- catalogues produits
- listings dynamiques

---

## 6. RSC (React Server Components)

### 6.1. Fonctionnement

- rendu côté serveur
- zéro JS côté client pour les RSC
- streaming natif
- data-fetching côté serveur

### 6.2. Avantages

- réduction massive du JS client
- meilleure performance
- meilleure stabilité
- hydration réduite

### 6.3. Bonnes pratiques

- éviter `"use client"`
- mettre la logique côté serveur
- découper les composants client

---

## 7. Hydration

### 7.1. Fonctionnement

- React “réactive” le HTML statique
- coûteux en JS
- peut créer des erreurs de mismatch

### 7.2. Réduire la hydration

- utiliser RSC
- réduire les composants client
- éviter les states inutiles
- éviter les effets globaux

---

## 8. Streaming & Suspense

### 8.1. Streaming

Permet d’envoyer le HTML **par morceaux**.

Avantages :

- affichage plus rapide
- meilleure perception du chargement

### 8.2. Suspense

Permet de différer le rendu de certaines parties.

Exemple :

```tsx
<Suspense fallback={<Loading />}>
  <HeavyComponent />
</Suspense>
```

---

## 9. Choisir la bonne stratégie

### Pages statiques

→ SSG

### Pages semi-dynamiques

→ ISR

### Pages dynamiques personnalisées

→ SSR

### Dashboards / apps interactives

→ CSR

### Pages publiques modernes

→ RSC + streaming

---

## 10. Checklist Rendering

- [ ] RSC maximisés
- [ ] Aucun `"use client"` inutile
- [ ] Pages statiques en SSG
- [ ] Pages dynamiques en ISR ou SSR
- [ ] Streaming activé si pertinent
- [ ] Hydration réduite
- [ ] Aucun composant client inutile
- [ ] fetch() côté serveur
- [ ] Aucune dépendance lourde côté client
````
