---
title: "Images Optimisation (Technique)"
description: "Guide technique complet pour optimiser les images : formats, compression, dimensions, lazy loading, CDN, Next.js <Image> et bonnes pratiques de performance."
updated_at: "2026-03-08"
tags:
  - Performance
  - Images
  - Next.js
  - LCP
  - CDN
  - Optimisation
---

# Images Optimisation (Technique)

Les images sont souvent la ressource la plus lourde d’une page web.  
Une optimisation rigoureuse permet d’améliorer :

- le LCP
- le CLS
- le TTFB (via CDN)
- la bande passante
- la fluidité générale

Ce guide couvre **uniquement les aspects techniques** liés aux images.

## Sommaire

1. [Principes généraux](#1-principes-généraux)
2. [Formats d’image](#2-formats-dimage)
3. [Compression](#3-compression)
4. [Dimensions & ratio](#4-dimensions--ratio)
5. [Lazy loading](#5-lazy-loading)
6. [Images & LCP](#6-images--lcp)
7. [CDN & caching](#7-cdn--caching)
8. [Next.js <Image>](#8-nextjs-image)
9. [Responsive images](#9-responsive-images)
10. [Checklist Images](#10-checklist-images)

---

## 1. Principes généraux

- Une image doit être **compressée**, **dimensionnée**, **mise en cache**, **servie via CDN**.
- Le format doit être choisi selon le type de contenu.
- Les dimensions doivent être explicites pour éviter le CLS.
- Le lazy loading doit être utilisé partout sauf pour le LCP.
- Le CDN doit gérer la mise à l’échelle et la compression si possible.

---

## 2. Formats d’image

### 2.1. Formats recommandés

- **WebP** → excellent ratio qualité/poids
- **AVIF** → encore plus léger, mais plus lent à encoder
- **JPEG** → fallback acceptable
- **PNG** → uniquement pour transparence ou graphismes nets

### 2.2. Formats à éviter

- BMP
- TIFF
- GIF (sauf animations très légères)

### 2.3. Règles

- Toujours servir WebP ou AVIF si supportés.
- Toujours garder un fallback JPEG/PNG si nécessaire.

---

## 3. Compression

### 3.1. Compression recommandée

- WebP : qualité 70–85
- AVIF : qualité 40–60
- JPEG : qualité 70–80

### 3.2. Outils

- sharp
- imagemin
- squoosh
- CDN (Cloudflare, Vercel, Akamai)

### 3.3. Bonnes pratiques

- éviter les images > 300 KB
- éviter les images > 1500px si non nécessaires
- compresser systématiquement avant upload

---

## 4. Dimensions & ratio

### 4.1. Toujours définir `width` et `height`

Cela évite le **CLS**.

### 4.2. Ratio fixe

Pour les images responsives, utiliser un conteneur avec ratio :

```css
.aspect-ratio {
  aspect-ratio: 16 / 9;
}
```

````

### 4.3. Next.js gère automatiquement

- le ratio
- les dimensions
- le resizing

---

## 5. Lazy loading

### 5.1. Règles

- lazy loading pour toutes les images **hors viewport**
- pas de lazy loading pour l’image LCP

### 5.2. Next.js

Lazy loading activé par défaut.

---

## 6. Images & LCP

### 6.1. LCP = souvent une image

Pour optimiser :

- utiliser `priority`
- compresser l’image
- réduire la résolution
- utiliser WebP/AVIF
- précharger l’image si nécessaire

### 6.2. Exemple Next.js

```tsx
<Image src="/hero.webp" alt="" width={1200} height={800} priority />
```

---

## 7. CDN & caching

### 7.1. Pourquoi un CDN ?

- réduction de la latence
- resizing automatique
- compression automatique
- caching distribué

### 7.2. Cache-Control recommandé

```
Cache-Control: public, max-age=31536000, immutable
```

### 7.3. Bonnes pratiques

- servir les images via CDN
- activer le resizing dynamique
- activer AVIF/WebP automatiques

---

## 8. Next.js <Image>

### 8.1. Avantages

- optimisation automatique
- formats modernes
- lazy loading
- resizing intelligent
- ratio préservé
- CDN intégré (Vercel)

### 8.2. Exemple simple

```tsx
<Image src="/photo.webp" alt="" width={800} height={600} />
```

### 8.3. Mode `fill`

```tsx
<div className="relative w-full h-64">
  <Image src="/photo.webp" alt="" fill />
</div>
```

⚠️ Toujours définir un conteneur avec dimensions fixes.

---

## 9. Responsive images

### 9.1. Utiliser `sizes`

```tsx
<Image
  src="/photo.webp"
  alt=""
  width={800}
  height={600}
  sizes="(max-width: 768px) 100vw, 50vw"
/>
```

### 9.2. Pourquoi ?

- éviter de charger une image trop grande
- réduire le poids total
- améliorer le LCP

---

## 10. Checklist Images

- [ ] Formats modernes (WebP / AVIF)
- [ ] Compression optimisée
- [ ] Dimensions définies (`width` / `height`)
- [ ] Lazy loading activé
- [ ] `priority` pour l’image LCP
- [ ] CDN actif
- [ ] Cache long pour les assets
- [ ] Aucun shift visuel
- [ ] Poids raisonnable (< 300 KB si possible)
- [ ] Responsive images (`sizes`)

---
````
