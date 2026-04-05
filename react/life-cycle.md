---
title: "Cycle de vie React moderne (React 18+)"
description: "Comprendre en profondeur les phases Render, Commit, Effects, le comportement concurrent, Strict Mode, et l'exécution des hooks."
author: "Kamil Cassam-Chenaï"
tags:
  - react
  - lifecycle
  - hooks
  - javascript
  - frontend
  - bonnes-pratiques
updated_date: "2026-03-02"
---

# 🔄 Cycle de vie React moderne (React 18+)

## 📑 Sommaire
1. Introduction : pourquoi comprendre le cycle de vie ?
2. Le modèle mental moderne : React = machine à rendre
3. Les 3 grandes phases du cycle de vie
   - 3.1 Render Phase
   - 3.2 Commit Phase
   - 3.3 Effect Phase
   - 3.4 Cleanup Phase
4. Le cycle complet (schéma)
5. Où s’exécutent les hooks ?
6. Le cycle de vie en mode concurrent (React 18)
7. Le cycle de vie en Strict Mode
8. Exemples concrets (avec code)
9. Erreurs courantes
10. Antisèche ultra condensée

---

# 1. Introduction : pourquoi comprendre le cycle de vie ?

Comprendre le cycle de vie React, c’est comprendre **quand** et **comment** React :

- exécute ton composant  
- met à jour le DOM  
- déclenche les effets  
- nettoie les abonnements  
- peut interrompre ou recommencer un rendu  
- optimise les interactions utilisateur  

C’est la base pour :

- écrire du code fiable  
- éviter les bugs subtils  
- optimiser les performances  
- comprendre useEffect  
- comprendre React concurrent  
- comprendre Strict Mode  

---

# 2. Le modèle mental moderne : React = machine à rendre

React n’est pas un framework MVC.  
React n’est pas un système d’événements.  
React n’est pas un moteur de templates.

> **React est une machine à rendre.**

Il prend **state + props → JSX**, et il recommence **à chaque mise à jour**.

Le rendu doit être :

- **pur**  
- **sans effets de bord**  
- **sans mutation externe**  
- **sans lecture DOM**  

Tout ce qui n’est pas du rendu pur doit aller dans un **effet**.

---

# 3. Les 3 grandes phases du cycle de vie

React moderne (React 18+) fonctionne en **3 phases principales** :

1. **Render Phase**  
2. **Commit Phase**  
3. **Effect Phase**  
(+ une phase transversale : **Cleanup Phase**)

---

## 3.1 Render Phase (phase de rendu)

C’est la phase où React :

- exécute ton composant (fonction)  
- calcule le JSX  
- prépare les changements à appliquer au DOM  
- peut **interrompre**, **reprendre**, **annuler** le rendu (React concurrent)  

### 🧠 Caractéristiques

- **pure** : pas d’effets de bord  
- **peut être exécutée plusieurs fois**  
- **peut être annulée**  
- **ne touche pas au DOM**  
- **ne déclenche pas les effets**  

### ❌ Ce qu’il ne faut PAS faire ici

- lire le DOM  
- modifier le DOM  
- lancer un fetch  
- démarrer un timer  
- accéder à des APIs externes  

---

## 3.2 Commit Phase (phase de commit)

C’est la phase où React :

- applique les changements au DOM  
- met à jour les refs  
- rend l’UI visible  

### 🧠 Caractéristiques

- **synchrone**  
- **non interruptible**  
- **très courte**  
- c’est ici que s’exécute **useLayoutEffect**  

### ✔️ Ce qu’on peut faire ici

- lire le DOM  
- mesurer le DOM  
- synchroniser un layout  

---

## 3.3 Effect Phase (phase des effets)

C’est la phase où React exécute :

- `useEffect`  
- les cleanups des effets précédents  

### 🧠 Caractéristiques

- **asynchrone**  
- **non bloquante**  
- peut être **retardée**  
- peut être **annulée avant exécution**  

### ✔️ Ce qu’on fait ici

- fetch (via React Query idéalement)  
- timers  
- abonnements  
- logs  
- synchronisation externe  

---

## 3.4 Cleanup Phase (phase de nettoyage)

Avant chaque nouvel effet ou avant le démontage, React exécute :

    return () => { ... }

### 🧠 Caractéristiques

- exécuté **avant** le prochain effet  
- exécuté **avant** le démontage  
- peut être exécuté **même si l’effet n’a jamais tourné** (React concurrent)  
- peut être exécuté **plusieurs fois** (Strict Mode)  

---

# 4. Le cycle complet (schéma)

Voici un schéma ASCII **qui ne casse pas le Markdown** :

    ┌──────────────────────────────┐
    │        Render Phase          │
    │  - exécution du composant    │
    │  - peut être interrompue     │
    │  - peut être annulée         │
    └───────────────┬──────────────┘
                    │
                    ▼
    ┌──────────────────────────────┐
    │        Commit Phase          │
    │  - mise à jour du DOM        │
    │  - refs mises à jour         │
    │  - useLayoutEffect           │
    └───────────────┬──────────────┘
                    │
                    ▼
    ┌──────────────────────────────┐
    │        Effect Phase          │
    │  - exécution des useEffect   │
    │  - asynchrone                │
    └───────────────┬──────────────┘
                    │
                    ▼
    ┌──────────────────────────────┐
    │        Cleanup Phase         │
    │  - avant prochain effet      │
    │  - avant démontage           │
    └──────────────────────────────┘

---

# 5. Où s’exécutent les hooks ?

| Hook | Phase | Notes |
|------|--------|--------|
| `useState` | Render | déclenche un nouveau rendu |
| `useMemo` | Render | stabilise une valeur |
| `useCallback` | Render | stabilise une fonction |
| `useRef` | Render | stable entre rendus |
| `useLayoutEffect` | Commit | synchrone, avant peinture |
| `useEffect` | Effect | asynchrone, après peinture |

---

# 6. Le cycle de vie en mode concurrent (React 18)

React 18 introduit un comportement **fondamental** :

> **Le rendu peut être interrompu, annulé, recommencé.**

Conséquences :

- un composant peut être rendu **plusieurs fois** avant un commit  
- un effet peut être **nettoyé avant d’être exécuté**  
- un effet peut être **retardé**  
- un rendu peut être **abandonné**  

### Exemple :

    render A
    render B
    cleanup A
    commit B
    effect B

---

# 7. Le cycle de vie en Strict Mode

Strict Mode **double volontairement** :

- le rendu  
- le montage  
- les effets  
- les cleanups  

Objectif :

- détecter les effets non idempotents  
- détecter les abonnements mal nettoyés  
- détecter les fetch dans useEffect  
- détecter les mutations cachées  

Ce n’est **pas un bug**, c’est un outil de détection.

---

# 8. Exemples concrets

## Exemple 1 : effet simple

    useEffect(() => {
      console.log("effect")
      return () => console.log("cleanup")
    }, [value])

En concurrent mode :

    cleanup (rendu annulé)
    cleanup (rendu annulé)
    effect (rendu final)

---

## Exemple 2 : mesure DOM

    useLayoutEffect(() => {
      const rect = ref.current.getBoundingClientRect()
      setSize(rect.width)
    })

→ exécuté **avant** la peinture  
→ évite les flashs visuels

---

## Exemple 3 : synchronisation externe

    useEffect(() => {
      const unsub = store.subscribe(() => setState(store.getState()))
      return () => unsub()
    }, [])

---

# 9. Erreurs courantes

- lire le DOM dans le rendu  
- fetch dans useEffect (sans React Query)  
- dériver un état dans un effet  
- synchroniser une prop dans un effet  
- oublier un cleanup  
- dépendances incorrectes  
- utiliser useLayoutEffect pour tout  
- croire que useEffect = “componentDidMount”  

---

# 10. Antisèche ultra condensée

### 🎯 À retenir

- **Render Phase** → pur, peut être interrompu  
- **Commit Phase** → DOM mis à jour, useLayoutEffect  
- **Effect Phase** → useEffect, asynchrone  
- **Cleanup Phase** → avant prochain effet / démontage  
- **Strict Mode** → double les effets pour détecter les erreurs  
- **Concurrent Mode** → rendus annulés, effets retardés  
- **Règle d’or** → rendu pur, effets de bord dans useEffect  
- **useLayoutEffect** → DOM avant peinture  
- **useEffect** → effets après peinture  

Cycle global :

Render → Commit → Layout Effects → Effects → Cleanup
