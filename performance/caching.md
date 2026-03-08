---
title: "Caching & Revalidation"
description: "Guide technique complet sur le caching navigateur, serveur, CDN et Next.js, incluant Cache-Control, revalidation, ISR et stratégies avancées."
updated_at: "2026-03-08"
tags:
  - Performance
  - Caching
  - Next.js
  - CDN
  - Revalidation
  - HTTP
---

# Caching & Revalidation

Le caching est l’un des leviers techniques les plus puissants pour améliorer les performances web.  
Bien configuré, il permet :

- de réduire drastiquement les temps de chargement
- de diminuer la charge serveur
- d’améliorer le LCP
- d’accélérer la navigation interne
- d’optimiser les coûts d’infrastructure

Ce guide couvre les stratégies de caching modernes : navigateur, CDN, serveur, API, ISR, revalidation, et les mécanismes propres à Next.js.

## Sommaire

1. [Principes généraux](#1-principes-généraux)
2. [Caching HTTP](#2-caching-http)
3. [Cache-Control](#3-cache-control)
4. [Caching navigateur](#4-caching-navigateur)
5. [Caching CDN](#5-caching-cdn)
6. [Caching serveur](#6-caching-serveur)
7. [Revalidation & ISR](#7-revalidation--isr)
8. [Caching des API](#8-caching-des-api)
9. [Caching dans Next.js](#9-caching-dans-nextjs)
10. [Checklist Caching](#10-checklist-caching)

---

## 1. Principes généraux

- Le caching doit être **prédictible**.
- Le caching doit être **explicite**.
- Le caching doit être **adapté au type de ressource**.
- Le caching doit être **cohérent entre navigateur, CDN et serveur**.
- Le caching doit être **mesuré** (DevTools, CDN logs, headers).

---

## 2. Caching HTTP

Le caching HTTP repose principalement sur :

- `Cache-Control`
- `ETag`
- `Last-Modified`
- `Expires`
- `Vary`

### 2.1. Types de cache

- **Fresh cache** → ressource servie directement
- **Revalidated cache** → ressource validée via ETag
- **Stale cache** → ressource périmée mais encore servie (stale-while-revalidate)

---

## 3. Cache-Control

### 3.1. Exemples courants

#### Cache long (assets statiques)

```

Cache-Control: public, max-age=31536000, immutable

```

#### Cache court (HTML)

```

Cache-Control: no-cache

```

#### Cache avec revalidation

```

Cache-Control: public, max-age=0, must-revalidate

```

#### Stale-while-revalidate

```

Cache-Control: public, max-age=60, stale-while-revalidate=300

```

---

## 4. Caching navigateur

### 4.1. Bonnes pratiques

- utiliser des **hashs** dans les noms de fichiers
- mettre un cache long sur les assets statiques
- éviter de mettre un cache long sur le HTML
- utiliser `immutable` pour les assets versionnés

### 4.2. Ressources à cache long

- JS bundlé
- CSS
- images
- fonts

---

## 5. Caching CDN

### 5.1. Pourquoi utiliser un CDN ?

- réduction de la latence
- caching distribué
- offload serveur
- meilleure disponibilité

### 5.2. Bonnes pratiques

- laisser le CDN gérer les assets statiques
- utiliser `stale-while-revalidate`
- utiliser `stale-if-error`
- configurer des règles par type de ressource

---

## 6. Caching serveur

### 6.1. Types de cache serveur

- cache mémoire (Redis, in-memory)
- cache disque
- cache applicatif
- cache API

### 6.2. Bonnes pratiques

- invalider le cache sur mise à jour
- utiliser des TTL adaptés
- éviter les caches globaux non segmentés

---

## 7. Revalidation & ISR

### 7.1. ISR (Incremental Static Regeneration)

Next.js permet de régénérer une page statique **en arrière-plan**.

### Exemple :

```ts
export const revalidate = 60; // régénération toutes les 60s
```

### 7.2. Revalidation on-demand

```ts
await res.revalidate("/produits/guitare");
```

### 7.3. Avantages

- contenu frais
- performances maximales
- pas de rebuild complet

---

## 8. Caching des API

### 8.1. Cache côté serveur

```ts
export const revalidate = 300; // cache API 5 min
```

### 8.2. Cache côté client

- SWR
- React Query
- localStorage (rarement recommandé)

### 8.3. Bonnes pratiques

- ne jamais mettre en cache des données sensibles
- utiliser des TTL courts pour les données dynamiques
- utiliser des TTL longs pour les données stables

---

## 9. Caching dans Next.js

### 9.1. fetch() avec caching intégré

```ts
const data = await fetch(url, {
  next: { revalidate: 60 },
});
```

### 9.2. fetch() sans cache

```ts
const data = await fetch(url, { cache: "no-store" });
```

### 9.3. fetch() avec cache long

```ts
const data = await fetch(url, { cache: "force-cache" });
```

### 9.4. Segment-level caching

Next.js peut mettre en cache un layout entier.

### 9.5. Route handlers

```ts
export const revalidate = 120;
```

---

## 10. Checklist Caching

- [ ] Cache-Control configuré correctement
- [ ] Assets statiques en cache long
- [ ] HTML en no-cache
- [ ] CDN configuré
- [ ] ISR activé pour les pages dynamiques
- [ ] Revalidation on-demand utilisée si nécessaire
- [ ] fetch() configuré avec revalidate
- [ ] Aucun cache sur les données sensibles
- [ ] TTL adaptés par type de ressource
- [ ] Aucun conflit entre CDN / serveur / navigateur
