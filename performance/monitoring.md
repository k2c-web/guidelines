---
title: "Performance Monitoring (Technique)"
description: "Guide technique complet pour monitorer les performances réelles, les erreurs, les métriques RUM, les logs serveur, les bundles JS et les API."
updated_at: "2026-03-08"
tags:
  - Performance
  - Monitoring
  - RUM
  - Core Web Vitals
  - Logging
  - Next.js
---

# Performance Monitoring (Technique)

Le monitoring technique permet de détecter les régressions de performance avant qu’elles n’impactent les utilisateurs.  
Il repose sur trois piliers :

- **RUM** (Real User Monitoring)
- **Synthetic monitoring** (tests automatisés)
- **Logging & observabilité**

Ce guide couvre uniquement les aspects techniques du monitoring.

## Sommaire

1. [Objectifs du monitoring technique](#1-objectifs-du-monitoring-technique)
2. [RUM (Real User Monitoring)](#2-rum-real-user-monitoring)
3. [Synthetic monitoring](#3-synthetic-monitoring)
4. [Monitoring des Core Web Vitals](#4-monitoring-des-core-web-vitals)
5. [Monitoring des erreurs JS](#5-monitoring-des-erreurs-js)
6. [Monitoring des API](#6-monitoring-des-api)
7. [Monitoring des bundles JS](#7-monitoring-des-bundles-js)
8. [Monitoring réseau & CDN](#8-monitoring-réseau--cdn)
9. [Logs serveur & observabilité](#9-logs-serveur--observabilité)
10. [Checklist Monitoring Technique](#10-checklist-monitoring-technique)

---

## 1. Objectifs du monitoring technique

- détecter les régressions de performance
- identifier les pages lentes
- analyser les interactions lentes (INP)
- surveiller les erreurs JS
- surveiller les API lentes ou instables
- surveiller le poids des bundles
- surveiller le comportement réel des utilisateurs
- surveiller le CDN et les caches

---

## 2. RUM (Real User Monitoring)

### 2.1. Outils

- Web Vitals JS
- Vercel Analytics
- SpeedCurve RUM
- Datadog RUM
- New Relic Browser

### 2.2. Métriques à collecter

- LCP réel
- CLS réel
- INP réel
- TTFB réel
- Latence réseau
- Erreurs JS
- Erreurs API

### 2.3. Exemple Web Vitals JS

```js
import { onLCP, onCLS, onINP } from "web-vitals";

onLCP(console.log);
onCLS(console.log);
onINP(console.log);
```

````

---

## 3. Synthetic monitoring

### 3.1. Outils

- PageSpeed Insights
- WebPageTest
- Lighthouse CI
- SpeedCurve Synthetic

### 3.2. Ce que ça mesure

- performance théorique
- filmstrip
- waterfall
- TTFB
- poids des ressources

### 3.3. Bonnes pratiques

- tester les pages critiques
- tester sur mobile
- tester sur réseau lent
- tester avec throttling CPU

---

## 4. Monitoring des Core Web Vitals

### 4.1. Outils

- CrUX (Chrome UX Report)
- Web Vitals JS
- Vercel Analytics
- PageSpeed Insights

### 4.2. Ce qu’il faut surveiller

- LCP réel
- CLS réel
- INP réel
- distribution des percentiles (p75)

### 4.3. Actions

- identifier les pages lentes
- identifier les éléments LCP
- identifier les shifts visuels
- identifier les interactions lentes

---

## 5. Monitoring des erreurs JS

### 5.1. Outils

- Sentry
- Datadog
- New Relic
- LogRocket

### 5.2. Types d’erreurs à surveiller

- erreurs runtime
- erreurs de hydration
- erreurs de rendering
- erreurs réseau
- erreurs de dépendances

### 5.3. Bonnes pratiques

- capturer les stack traces
- capturer les breadcrumbs
- capturer les contextes utilisateur
- regrouper les erreurs par fingerprint

---

## 6. Monitoring des API

### 6.1. Métriques

- latence
- taux d’erreur
- timeouts
- saturation
- cold starts (serverless)

### 6.2. Outils

- Datadog APM
- New Relic APM
- OpenTelemetry
- Vercel Logs

### 6.3. Bonnes pratiques

- tracer chaque requête
- mesurer le TTFB
- monitorer les endpoints critiques
- monitorer les revalidations ISR

---

## 7. Monitoring des bundles JS

### 7.1. Outils

- next-bundle-analyzer
- webpack-bundle-analyzer
- Source Map Explorer

### 7.2. Ce qu’il faut surveiller

- poids du bundle principal
- poids des chunks dynamiques
- duplication de modules
- dépendances lourdes

### 7.3. Exemple Next.js

```js
const withBundleAnalyzer = require("@next/bundle-analyzer")({
  enabled: process.env.ANALYZE === "true",
});
module.exports = withBundleAnalyzer({});
```

---

## 8. Monitoring réseau & CDN

### 8.1. Métriques

- latence CDN
- taux de cache hit
- erreurs 4xx / 5xx
- temps de propagation
- taille des réponses

### 8.2. Outils

- Cloudflare Analytics
- Vercel Edge Network
- Akamai mPulse

### 8.3. Bonnes pratiques

- surveiller les assets lourds
- surveiller les images non compressées
- surveiller les réponses non cachées

---

## 9. Logs serveur & observabilité

### 9.1. Logs à collecter

- 404
- 500 / 503
- timeouts
- latence API
- erreurs serverless
- cold starts

### 9.2. Outils

- Datadog Logs
- Elastic Stack
- OpenTelemetry
- Vercel Logs

### 9.3. Bonnes pratiques

- corréler logs + RUM + synthetic
- tracer les requêtes de bout en bout
- monitorer les pics de charge
- détecter les régressions après déploiement

---

## 10. Checklist Monitoring Technique

- [ ] RUM activé
- [ ] LCP / CLS / INP réels monitorés
- [ ] Synthetic monitoring configuré
- [ ] Erreurs JS monitorées
- [ ] API monitorées (latence + erreurs)
- [ ] Bundles JS analysés
- [ ] CDN monitoré
- [ ] Logs serveur collectés
- [ ] Traces distribuées activées
- [ ] Alertes configurées sur les métriques critiques

````
