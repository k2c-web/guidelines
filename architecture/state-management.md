---
title: "23 — State Management"
description: "Documentation architecturale complète pour structurer, comprendre et gouverner la gestion d’état dans une application React/Next.js moderne."
tags: ["architecture", "state-management", "react", "nextjs", "zustand", "react-query"]
updated: "2026-02-17"
---

# 23 — State Management

Documentation architecturale complète pour comprendre, structurer et gouverner la gestion d’état dans une application React/Next.js moderne.

---

## 📚 Sommaire

- [1. Résumé exécutif](#1-résumé-exécutif)
- [2. Les quatre types d’état](#2-les-quatre-types-détat)
  - [2.1 Local UI state](#21-local-ui-state)
  - [2.2 Derived state](#22-derived-state)
  - [2.3 Server state](#23-server-state)
  - [2.4 Global app state](#24-global-app-state)
- [3. Principes clés](#3-principes-clés)
- [4. Outils modernes](#4-outils-modernes)
- [5. State management dans Next.js](#5-state-management-dans-nextjs)
- [6. Anti‑patterns](#6-anti‑patterns)
- [7. Schéma conceptuel](#7-schéma-conceptuel-texte)
- [8. Liens internes](#8-liens-internes)
- [9. Réponses d’entretien](#9-réponses-dentretien-version-courte)
- [10. Conclusion](#10-conclusion)

---

## 1. Résumé exécutif

Le state management est un **problème d’architecture**, pas un choix de librairie.

Il consiste à organiser :

- la circulation des données  
- la propriété des données  
- la mutation des données  

Un architecte cherche à :

- **minimiser le global**  
- **séparer les responsabilités**  
- **optimiser les flux**  

---

## 2. Les quatre types d’état

### 2.1 Local UI state

État éphémère, lié à un composant.

- `useState`, `useReducer`
- Exemples : modals, inputs, toggles
- Toujours localisé

---

### 2.2 Derived state

Valeurs calculées à partir d’autres états.

- Jamais stocké
- Toujours dérivé via fonctions pures

---

### 2.3 Server state

Données provenant d’une API.

- Externalisé via **React Query** ou **RSC**
- Synchronisation automatique
- Cache, retry, pagination

---

### 2.4 Global app state

État transversal minimal.

- Auth, thème, préférences
- Zustand ou Redux Toolkit

---

## 3. Principes clés

### 3.1 Minimiser le global state  
Plus un état est global, plus il est dangereux.

### 3.2 Séparer UI state et server state  
Erreur classique : stocker le server state dans un store global.

### 3.3 Choisir l’outil adapté

| Type d’état | Outil recommandé |
|-------------|------------------|
| Local UI | useState, useReducer |
| Derived | Fonctions pures |
| Server | React Query / RSC |
| Global | Zustand / Redux Toolkit |

### 3.4 Réduire la surface de mutation  
Limiter les endroits où l’état peut changer.

---

## 4. Outils modernes

### 4.1 React Query  
Gestion avancée du server state.

- Cache intelligent  
- Revalidation  
- Pagination  
- Annulation  

### 4.2 Zustand  
Store léger et performant.

- API simple  
- Sélecteurs précis  
- Middlewares utiles  

### 4.3 Redux Toolkit  
Pour les très gros projets.

- Standardisation  
- Immutabilité garantie  
- Outils entreprise  

### 4.4 Context API  
À utiliser pour les données stables.

- Thème  
- Localisation  
- Config statique  

---

## 5. State management dans Next.js

### 5.1 React Server Components  
Déplacement d’une partie du state management côté serveur.

- Moins de client state  
- Moins de stores globaux  
- Meilleure performance  

### 5.2 Server Actions  
Mutations côté serveur sans gestion client complexe.

### 5.3 Cache et revalidation  
Mécanismes clés :

- `cache()`  
- `revalidateTag()`  
- `revalidatePath()`  

---

## 6. Anti‑patterns

- Stocker du server state dans Redux/Zustand  
- Utiliser Context pour des données qui changent souvent  
- Avoir un store global tentaculaire  
- Mélanger UI state et business state  
- Recalculer du derived state dans un store  

---

## 7. Schéma conceptuel (texte)

```
[Server] → React Query / RSC → [Server State]

[UI Components] → useState/useReducer → [Local UI State]

[Global Store] → Zustand/Redux → [Global App State]

[Derived State] = compute(Local + Global + Server)
```

---

## 8. Liens internes

- Voir aussi : **21 — Architecture moderne**  
- Voir aussi : **24 — Performance**  
- Voir aussi : **25 — Boundaries & Domain**  

---

## 9. Réponses d’entretien (version courte)

### 9.1 Réponse synthétique  
Le state management est un choix d’architecture.  
Je distingue quatre types d’état : local UI, derived, server et global app state.  
Chaque type a son outil optimal.

### 9.2 Points clés à dire

- Le server state ne doit jamais être dans un store global.  
- Le global state doit être minimal.  
- React Query gère le server state, Zustand gère le global léger.  
- Next.js déplace une partie du state management côté serveur.  
- Mon rôle est de cartographier les flux et réduire la surface de mutation.  

---

## 10. Conclusion

Une architecture de state management réussie repose sur :

- la clarté des responsabilités  
- la minimisation du global  
- l’utilisation judicieuse des outils modernes  

Cette page sert de référence pour concevoir, auditer et expliquer une stratégie de gestion d’état dans un environnement React/Next.js.

