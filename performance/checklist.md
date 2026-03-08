---
title: "Performance Checklist"
description: "Checklist complète pour garantir des performances front-end optimales : rendu, JavaScript, images, caching, Core Web Vitals et monitoring."
updated_at: "2026-03-08"
tags:
  - Performance
  - Checklist
  - Core Web Vitals
  - Rendering
  - Caching
---

# Performance Checklist

Cette checklist regroupe les points essentiels pour garantir un site rapide, stable et fluide.

---

## ⚙️ 1. Rendu

- [ ] Rendu serveur utilisé (RSC/SSR/SSG/ISR)
- [ ] Pas de rendu 100 % client inutile
- [ ] Streaming utilisé si pertinent
- [ ] Découpage intelligent des composants

---

## 🧩 2. JavaScript

- [ ] Bundle JS analysé
- [ ] Librairies lourdes identifiées
- [ ] Code splitting actif
- [ ] JS chargé uniquement quand nécessaire
- [ ] Hydration minimale

---

## 🖼️ 3. Images

- [ ] Formats modernes (WebP/AVIF)
- [ ] Compression optimisée
- [ ] Dimensions définies (`width` / `height`)
- [ ] Lazy loading activé
- [ ] `priority` pour l’image LCP
- [ ] CDN actif

---

## 🗄️ 4. Caching

- [ ] Cache-Control long pour les assets
- [ ] ISR utilisé si pertinent
- [ ] fetch() avec `cache` et `revalidate`
- [ ] CDN avec cache hits monitorés

---

## 📊 5. Core Web Vitals

### LCP

- [ ] Image principale optimisée
- [ ] Formats modernes
- [ ] `priority` utilisé

### CLS

- [ ] Dimensions définies
- [ ] Pas de contenu injecté tardivement

### INP

- [ ] JS réduit
- [ ] Interactions optimisées

### TTFB

- [ ] Rendu serveur performant
- [ ] CDN efficace

---

## 🔍 6. Monitoring

- [ ] RUM activé (Web Vitals JS, Vercel Analytics)
- [ ] Synthetic monitoring configuré
- [ ] Erreurs JS monitorées
- [ ] API monitorées (latence + erreurs)
- [ ] Bundles JS analysés
- [ ] CDN monitoré
- [ ] Logs serveur collectés

```

```
