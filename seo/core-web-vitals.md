---
title: "Core Web Vitals"
description: "Guide complet pour optimiser LCP, CLS et INP dans des applications front-end et Next.js."
updated_at: "2026-03-08"
tags:
  - SEO
  - Performance
  - Core Web Vitals
  - Front-End
  - Next.js
---

# Core Web Vitals

Les Core Web Vitals sont des indicateurs de performance centrés sur l’expérience utilisateur. Ils influencent directement le SEO, le taux de conversion et la satisfaction utilisateur. Ce guide détaille les bonnes pratiques pour optimiser LCP, CLS et INP dans des projets front-end et Next.js.

## Sommaire

1. [Introduction](#1-introduction)
2. [Largest Contentful Paint (LCP)](#2-largest-contentful-paint-lcp)
3. [Cumulative Layout Shift (CLS)](#3-cumulative-layout-shift-cls)
4. [Interaction to Next Paint (INP)](#4-interaction-to-next-paint-inp)
5. [Optimisations transversales](#5-optimisations-transversales)
6. [Mesure & monitoring](#6-mesure--monitoring)
7. [Spécificités Next.js](#7-spécificités-nextjs)

---

## 1. Introduction

Les Core Web Vitals se composent de trois métriques principales :

| Métrique | Objectif | Description                                       |
| -------- | -------- | ------------------------------------------------- |
| **LCP**  | < 2.5s   | Temps de chargement du plus grand élément visible |
| **CLS**  | < 0.1    | Stabilité visuelle de la page                     |
| **INP**  | < 200ms  | Réactivité globale aux interactions               |

---

## 2. Largest Contentful Paint (LCP)

### 2.1. Identifier l’élément LCP

Souvent :

- image hero
- titre principal
- bloc de texte large
- vidéo en autoplay

### 2.2. Optimisations clés

#### ✔️ Optimiser l’image LCP

- Utiliser des formats modernes (WebP, AVIF)
- Utiliser `<Image>` de Next.js
- Définir `priority` sur l’image LCP

#### ✔️ Précharger les ressources critiques

- `<link rel="preload" as="image" href="...">`
- `<link rel="preload" as="font" href="...">`

#### ✔️ Réduire le temps serveur

- Utiliser SSG ou ISR pour les pages publiques
- Minimiser les appels API bloquants

#### ✔️ Minimiser le CSS critique

- Éviter les frameworks CSS lourds
- Utiliser le CSS-in-JS côté serveur (RSC)

#### ✔️ Éviter les scripts bloquants

- Charger les scripts non critiques en `defer` ou `async`

---

## 3. Cumulative Layout Shift (CLS)

### 3.1. Causes courantes

- Images sans dimensions
- Bannières ou popups injectées tardivement
- Iframes sans dimensions
- Fonts non optimisées (FOIT/FOUT)
- Composants React hydratés tardivement

### 3.2. Optimisations clés

#### ✔️ Toujours définir `width` et `height`

Même avec Next.js `<Image>`, vérifier que les dimensions sont cohérentes.

#### ✔️ Réserver l’espace pour les éléments dynamiques

- placeholders
- skeletons
- containers avec hauteur fixe

#### ✔️ Charger les fonts proprement

- Utiliser `font-display: swap`
- Précharger les fonts critiques

#### ✔️ Éviter les injections tardives

- Consent banners
- Chat widgets
- Publicités

#### ✔️ Stabiliser les composants hydratés

- Prévoir la taille finale avant hydration

---

## 4. Interaction to Next Paint (INP)

### 4.1. Causes courantes de mauvaise réactivité

- Trop de JavaScript côté client
- Hydration lourde
- Event handlers complexes
- Re-renders inutiles
- Longues tâches JS (>50ms)

### 4.2. Optimisations clés

#### ✔️ Réduire le JavaScript client

- Utiliser React Server Components
- Supprimer les libs inutiles
- Éviter les polyfills globaux

#### ✔️ Découper les longues tâches

- `requestIdleCallback`
- `setTimeout` pour découper les blocs lourds

#### ✔️ Optimiser les handlers

- Debounce/throttle
- Utiliser `useCallback` et `useMemo` intelligemment

#### ✔️ Minimiser les re-renders

- Composants purs
- Séparation logique UI / data
- Suspense + streaming

---

## 5. Optimisations transversales

### ✔️ Minimiser le JS global

- Tree-shaking
- Dynamic imports
- Supprimer les polyfills inutiles

### ✔️ Optimiser le CSS

- CSS critical path
- Éviter les frameworks lourds
- Utiliser Tailwind ou CSS modules

### ✔️ Optimiser le réseau

- Compression Brotli
- HTTP/2 ou HTTP/3
- CDN pour les assets

### ✔️ Optimiser le cache

- Cache-Control agressif pour les assets statiques
- Revalidation intelligente (ISR)

---

## 6. Mesure & monitoring

### Outils recommandés

- Lighthouse
- PageSpeed Insights
- Web Vitals extension
- Chrome DevTools Performance
- Google Search Console (CWV report)
- SpeedCurve (si projet pro)

### Mesures en production

- Utiliser l’API Web Vitals
- Envoyer les métriques à :
  - LogRocket
  - Datadog
  - Sentry
  - Vercel Analytics

---

## 7. Spécificités Next.js

### ✔️ Utiliser les React Server Components

→ Moins de JS côté client → meilleur INP

### ✔️ Utiliser la Metadata API

→ Préchargement automatique des assets critiques

### ✔️ Utiliser `<Image>` correctement

- `priority` pour l’image LCP
- formats modernes automatiques

### ✔️ Préférer SSG/ISR pour les pages publiques

→ LCP plus rapide

### ✔️ Middleware pour redirections légères

→ Pas de surcharge côté client

### ✔️ Streaming + Suspense

→ Améliore la perception du chargement
