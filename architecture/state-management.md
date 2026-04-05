---
title: "State Management"
description: "Principes d’architecture pour gérer l’état dans une application React/Next : server state, client state, patterns, boundaries et décisions."
updated_at: "2026-03-08"
tags:
  - Architecture
  - State Management
  - React
  - Next.js
  - Patterns
---

# State Management

Ce document présente les **principes d’architecture** pour gérer l’état dans une application React/Next moderne.  
Il ne traite pas des bibliothèques (Zustand, Jotai, Redux Toolkit, TanStack Query) — elles sont documentées dans `/libs`.

Objectif :  
👉 comprendre **comment penser** l’état, **où le placer**, **comment le découper**, et **comment éviter la complexité inutile**.

## Sommaire

1. [Principes fondamentaux](#1-principes-fondamentaux)
2. [Server state vs Client state](#2-server-state-vs-client-state)
3. [Types d’état](#3-types-détat)
4. [Patterns d’architecture](#4-patterns-darchitecture)
5. [Boundaries & découpage](#5-boundaries--découpage)
6. [Comment choisir un outil](#6-comment-choisir-un-outil)
7. [Erreurs fréquentes](#7-erreurs-fréquentes)

---

# 1. Principes fondamentaux

### ✔️ 1.1. L’état doit être **colocalisé**

Un état doit vivre **au plus près** du composant qui l’utilise.

### ✔️ 1.2. L’état global est une **exception**

Un store global doit être justifié par :

- un besoin de partage large
- un besoin de persistance
- un besoin de synchronisation

### ✔️ 1.3. L’état doit être **minimal**

Ne stocker que ce qui est nécessaire.  
Tout le reste doit être **dérivé**.

### ✔️ 1.4. L’état doit être **prévisible**

- pas de mutation silencieuse
- pas d’effets cachés
- pas de dépendances implicites

### ✔️ 1.5. L’état doit être **stable**

Pour éviter les re-renders inutiles.

---

# 2. Server state vs Client state

## 2.1. Server state

Données provenant du serveur :

- API
- base de données
- CMS
- fichiers distants

Caractéristiques :

- source de vérité = serveur
- doit être synchronisé
- peut être mis en cache
- peut être revalidé

👉 Dans Next.js App Router :  
**le server state doit être géré côté serveur** (RSC + fetch + revalidate).

---

## 2.2. Client state

Données propres à l’UI :

- modales
- formulaires
- filtres locaux
- sélection d’éléments
- état temporaire

Caractéristiques :

- ne vient pas du serveur
- ne doit pas être synchronisé
- ne doit pas être global par défaut

👉 Le client state doit être **colocalisé**.

---

# 3. Types d’état

## 3.1. UI State

- modales
- menus
- hover
- focus
- stepper

→ local, simple, colocalisé.

---

## 3.2. Form State

- inputs
- validation
- erreurs

→ local, souvent géré par React ou une lib dédiée.

---

## 3.3. Derived State

- computed values
- filtres
- transformations

→ ne doit **jamais** être stocké.  
→ toujours dérivé.

---

## 3.4. Server State

- données fetchées
- données paginées
- données invalidables

→ géré côté serveur (RSC) ou via une lib spécialisée.

---

## 3.5. Global State

- auth
- user preferences
- panier
- thème

→ doit être rare et justifié.

---

# 4. Patterns d’architecture

## 4.1. Lifting State Up

Remonter l’état au parent commun minimal.

---

## 4.2. Colocation

Placer l’état **dans le composant qui en a besoin**.

---

## 4.3. Derived State

Ne jamais stocker :

- un filtre
- un tri
- une transformation
- une valeur calculée

Toujours dériver.

---

## 4.4. State Machines (optionnel)

Pour les flows complexes :

- onboarding
- checkout
- formulaires multi‑étapes

---

## 4.5. Event-driven state

Utiliser des événements plutôt que des mutations directes.

---

## 4.6. Server-first architecture (Next.js)

Dans App Router :

- fetch côté serveur
- revalidate
- pas de client fetch inutile
- pas de state global pour du server state

---

# 5. Boundaries & découpage

Les _state boundaries_ définissent **où commence et où s’arrête un état**, et comment découper l’application en zones cohérentes, indépendantes et prévisibles.

Elles permettent d’éviter :

- les stores trop gros
- les context omniprésents
- les re-renders en cascade
- les dépendances implicites
- la confusion entre server state et client state

Elles sont essentielles pour une architecture React/Next moderne.

---

## 5.1. UI Boundary — l’état local

L’état doit rester **dans le composant qui en a besoin**.

Exemples :

- modales
- menus
- hover/focus
- formulaires
- stepper

Outils :

- `useState`
- `useReducer`
- Jotai (atoms locaux)

---

## 5.2. Data Boundary — le server state

Le server state doit rester **côté serveur** ou dans un outil spécialisé.

Exemples :

- données fetchées
- données paginées
- données invalidables
- données synchronisées

Outils :

- RSC + `fetch`
- `revalidate`
- React Query (client hydration)

---

## 5.3. Context Boundary — le partage léger

Un contexte doit être :

- petit
- ciblé
- stable

Exemples :

- thème
- locale
- session utilisateur (si stable)

Outils :

- React Context
- Zustand (si besoin de global léger)

---

## 5.4. Store Boundary — la logique métier globale

Un store global doit être :

- découpé
- modulaire
- minimal
- prévisible

Exemples :

- panier
- préférences utilisateur
- logique métier complexe
- interactions globales

Outils :

- Zustand (slices)
- Redux Toolkit (slices)
- Jotai (atoms modulaires)

---

## 5.5. Boundary Map — résumé visuel

| Type d’état         | Boundary         | Outil recommandé                |
| ------------------- | ---------------- | ------------------------------- |
| UI state            | UI Boundary      | useState / useReducer           |
| Form state          | UI Boundary      | React Hook Form                 |
| Derived state       | UI Boundary      | derived atoms (Jotai)           |
| Server state        | Data Boundary    | RSC / React Query               |
| Global client state | Store Boundary   | Zustand / Jotai / Redux Toolkit |
| Partage léger       | Context Boundary | React Context                   |

---

## 5.6. Règle d’or

> **Un état doit vivre dans la boundary la plus basse possible.**

Si tu dois remonter l’état, fais-le **par nécessité**, pas par réflexe.

---

## 5.7. Anti-patterns liés aux boundaries

- tout mettre dans un store global
- utiliser Context pour tout
- stocker du server state dans Zustand
- fetch côté client alors que RSC suffit
- créer un store par réflexe
- mélanger server state et client state
- ne pas découper les slices
- mettre des objets complexes dans un store global

---

# 6. Comment choisir un outil

## 6.1. Pas d’outil

→ UI state simple  
→ formulaires simples  
→ derived state

---

## 6.2. Context

→ partage léger  
→ pas de logique complexe  
→ pas de fréquences élevées de mise à jour

---

## 6.3. Zustand / Jotai

→ client state global  
→ logique métier  
→ stores modulaires  
→ interactions rapides

---

## 6.4. TanStack Query

→ server state  
→ caching  
→ invalidation  
→ pagination

---

## 6.5. Redux Toolkit

→ flows complexes  
→ équipes nombreuses  
→ besoin de tooling

---

# 7. Erreurs fréquentes

- tout mettre dans un store global
- utiliser Context pour tout
- stocker du derived state
- fetch côté client alors que RSC suffit
- mélanger server state et client state
- créer un store par réflexe
- ne pas découper les boundaries
- muter l’état au lieu de le recréer

---

# 🎯 Conclusion

Le state management est un sujet **d’architecture**, pas un sujet d’outils.  
Les décisions doivent être guidées par :

- la nature de l’état
- sa durée de vie
- sa source de vérité
- son périmètre
- sa fréquence de mise à jour

Les bibliothèques viennent **après** la réflexion architecturale.
