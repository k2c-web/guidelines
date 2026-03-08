---
title: "Images & SEO"
description: "Impact des images sur le SEO : bonnes pratiques, structure, accessibilité, indexation et liens vers les optimisations techniques."
updated_at: "2026-03-08"
tags:
  - SEO
  - Images
  - Content
  - Indexation
---

# Images & SEO

Les images jouent un rôle important dans le SEO, non seulement pour Google Images, mais aussi pour la qualité globale des pages, l’accessibilité et certains signaux Core Web Vitals.

Ce document couvre **uniquement les aspects SEO** : structure, sémantique, indexation, bonnes pratiques éditoriales.  
Pour les optimisations techniques (formats, compression, lazy loading, CDN, LCP…), voir la section **Références techniques** en bas de page.

## Sommaire

1. [Rôle des images en SEO](#1-rôle-des-images-en-seo)
2. [Bonnes pratiques SEO](#2-bonnes-pratiques-seo)
3. [Accessibilité & sémantique](#3-accessibilité--sémantique)
4. [Indexation & Google Images](#4-indexation--google-images)
5. [Impact sur les Core Web Vitals](#5-impact-sur-les-core-web-vitals)
6. [Erreurs fréquentes](#6-erreurs-fréquentes)
7. [Références techniques](#7-références-techniques)

---

## 1. Rôle des images en SEO

Les images influencent le SEO à plusieurs niveaux :

- **Google Images** → source de trafic supplémentaire
- **Qualité perçue** → impact sur l’engagement
- **Sémantique** → contexte pour Google
- **Accessibilité** → meilleure compréhension du contenu
- **Core Web Vitals** → LCP, CLS

Les images ne sont pas un simple élément visuel : elles participent à la compréhension globale de la page.

---

## 2. Bonnes pratiques SEO

### 2.1. Utiliser des images pertinentes

Une image doit renforcer le contenu, pas le décorer.

### 2.2. Utiliser des noms de fichiers descriptifs

Exemples :

- `guitare-electrique-fender-stratocaster.webp`
- `schema-architecture-nextjs.png`

Éviter :

- `IMG_1234.png`
- `image-final-v3.png`

### 2.3. Utiliser des légendes si pertinent

Les légendes sont l’un des éléments les plus lus d’une page.

### 2.4. Utiliser des images uniques

Éviter les images stock trop génériques.

---

## 3. Accessibilité & sémantique

### 3.1. Texte alternatif (`alt`)

- doit décrire l’image
- doit être utile pour les lecteurs d’écran
- doit être concis

Exemples :

- ✔️ `alt="Guitare électrique Fender Stratocaster noire"`
- ✔️ `alt="Diagramme montrant l’architecture d’une application Next.js"`
- ✖️ `alt="image"`
- ✖️ `alt=""` (sauf images décoratives)

### 3.2. Images décoratives

Utiliser `alt=""` uniquement si l’image n’a **aucune** valeur sémantique.

### 3.3. Ne pas mettre de texte dans les images

Sauf cas exceptionnels (schémas, graphiques).

---

## 4. Indexation & Google Images

### 4.1. Conditions pour être indexé

- image accessible (pas en lazy JS-only)
- URL stable
- pas de `noindex` sur la page
- pas de blocage robots.txt
- format lisible par Google

### 4.2. Sitemaps d’images

Optionnel mais utile pour les gros sites.

### 4.3. Contexte textuel

Google comprend mieux une image si elle est entourée de texte pertinent.

---

## 5. Impact sur les Core Web Vitals

Les images influencent indirectement :

### 5.1. LCP (Largest Contentful Paint)

L’image principale d’une page est souvent l’élément LCP.

### 5.2. CLS (Cumulative Layout Shift)

Les images sans dimensions provoquent des décalages visuels.

### 5.3. INP (Interaction to Next Paint)

Images lourdes → JS + rendu plus lents → interactions dégradées.

👉 Pour les optimisations techniques, voir `/performance/core-web-vitals.md`.

---

## 6. Erreurs fréquentes

- images trop lourdes
- absence de texte alternatif
- noms de fichiers non descriptifs
- images décoratives avec alt non vide
- images sans contexte textuel
- images non pertinentes
- images dupliquées ou trop génériques

---

## 7. Références techniques

Pour les optimisations techniques (formats, compression, lazy loading, CDN, Next.js `<Image>`, LCP…), voir :

- `/performance/images.md`
- `/performance/core-web-vitals.md`
- `/performance/rendering.md`
- `/performance/caching.md`

```

```
