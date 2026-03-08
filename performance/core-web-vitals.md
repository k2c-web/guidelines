---
title: "Core Web Vitals (Technique)"
description: "Guide technique complet pour comprendre, mesurer et optimiser LCP, CLS et INP dans des applications modernes et Next.js."
updated_at: "2026-03-08"
tags:
  - Performance
  - Core Web Vitals
  - LCP
  - CLS
  - INP
  - Next.js
---

# Core Web Vitals (Technique)

Les Core Web Vitals sont des métriques de performance réelles (RUM) mesurées sur les utilisateurs.  
Elles reflètent la vitesse, la stabilité et la réactivité d’un site.

Ce guide couvre **uniquement les aspects techniques** : causes, diagnostic, outils, optimisations.

## Sommaire

1. [Présentation des CWV](#1-présentation-des-cwv)
2. [LCP (Largest Contentful Paint)](#2-lcp-largest-contentful-paint)
3. [CLS (Cumulative Layout Shift)](#3-cls-cumulative-layout-shift)
4. [INP (Interaction to Next Paint)](#4-inp-interaction-to-next-paint)
5. [Mesure & diagnostic](#5-mesure--diagnostic)
6. [Optimisations globales](#6-optimisations-globales)
7. [Checklist CWV](#7-checklist-cwv)

---

## 1. Présentation des CWV

### 1.1. Les trois métriques

- **LCP** → vitesse d’affichage du contenu principal
- **CLS** → stabilité visuelle
- **INP** → réactivité aux interactions

### 1.2. Seuils recommandés

- **LCP** < 2.5s
- **CLS** < 0.1
- **INP** < 200ms

---

## 2. LCP (Largest Contentful Paint)

### 2.1. Causes techniques fréquentes

- image LCP trop lourde
- absence de `priority` (Next.js)
- absence de dimensions d’image
- fonts bloquantes
- JS bloquant le rendu
- serveur lent (TTFB élevé)
- absence de caching / CDN

### 2.2. Diagnostic

- Chrome DevTools → Performance → LCP
- PageSpeed Insights → “Élément LCP”
- WebPageTest → Filmstrip
- CrUX → données réelles

### 2.3. Optimisations techniques

#### Images

- utiliser `<Image>` Next.js
- utiliser `priority`
- compresser (WebP / AVIF)
- réduire la résolution
- précharger l’image LCP si nécessaire

#### Fonts

- utiliser `font-display: swap`
- précharger les fonts critiques
- éviter les fonts trop lourdes

#### Serveur

- activer ISR / SSG
- réduire le TTFB
- utiliser un CDN

#### JS

- réduire le JS client
- éviter les composants client inutiles
- utiliser RSC

---

## 3. CLS (Cumulative Layout Shift)

### 3.1. Causes techniques fréquentes

- images sans dimensions
- iframes sans dimensions
- fonts qui changent la mise en page
- bannières / popups injectées tardivement
- hydration React tardive

### 3.2. Diagnostic

- Chrome DevTools → Performance → Layout shifts
- PageSpeed Insights → CLS contributions
- WebPageTest → Layout shift visualization

### 3.3. Optimisations techniques

#### Images

- toujours définir `width` et `height`
- utiliser `<Image>` qui gère le ratio

#### Iframes

- définir un conteneur avec ratio fixe

#### Fonts

- utiliser `font-display: swap`
- précharger les fonts
- éviter les FOIT/FOUT

#### UI

- réserver l’espace des éléments dynamiques
- éviter les injections tardives

---

## 4. INP (Interaction to Next Paint)

### 4.1. Causes techniques fréquentes

- JS trop lourd
- handlers d’événements lents
- re-renders React inutiles
- composants client trop complexes
- librairies lourdes (charts, sliders…)
- hydration lente

### 4.2. Diagnostic

- Chrome DevTools → Performance → Interactions
- PageSpeed Insights → INP contributions
- Web Vitals JS (RUM)

### 4.3. Optimisations techniques

#### JS

- réduire le JS client
- utiliser RSC
- découper les handlers
- éviter les closures inutiles
- éviter les re-renders (memo, useCallback)

#### React

- réduire les composants client
- éviter les states globaux
- éviter les effets lourds

#### Librairies

- remplacer les librairies lourdes
- lazy-load des composants interactifs

---

## 5. Mesure & diagnostic

### 5.1. Outils

- Chrome DevTools
- PageSpeed Insights
- WebPageTest
- Lighthouse (indicatif)
- CrUX (réel)
- Vercel Analytics
- Web Vitals JS

### 5.2. Méthodes

- analyser le LCP réel
- identifier les shifts visuels
- mesurer les interactions lentes
- analyser le TTFB
- analyser le JS client

---

## 6. Optimisations globales

### 6.1. Réduire le JS client

- RSC
- dynamic imports
- tree-shaking
- suppression des dépendances lourdes

### 6.2. Optimiser les images

- formats modernes
- compression
- dimensions
- lazy loading
- priority pour LCP

### 6.3. Optimiser le rendu

- SSG / ISR
- streaming
- éviter SSR inutile

### 6.4. Optimiser le réseau

- CDN
- caching
- preconnect / preload

---

## 7. Checklist CWV

- [ ] LCP < 2.5s
- [ ] Image LCP optimisée
- [ ] `priority` utilisé
- [ ] Dimensions d’images définies
- [ ] Aucun shift visuel (CLS < 0.1)
- [ ] Fonts préchargées
- [ ] JS client minimal
- [ ] Handlers optimisés
- [ ] RSC maximisés
- [ ] Aucune dépendance lourde inutile
- [ ] TTFB optimisé
- [ ] CDN actif
