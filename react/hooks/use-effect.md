---
title: "Guide complet du useEffect en React moderne"
description: "Référence complète sur useEffect, ses dépendances, ses pièges, React concurrent, Strict Mode, useMemo et bonnes pratiques."
author: "Kamil Cassam-Chenaï"
tags:
  - react
  - hooks
  - useeffect
  - javascript
  - frontend
  - bonnes-pratiques
updated_date: "2026-03-02"
---

# 📘 Guide complet du `useEffect` en React moderne

## 📑 Sommaire

1. Qu’est‑ce que useEffect ?
2. useEffect vs useLayoutEffect
3. Bonnes pratiques pour les dépendances
4. Stabilisation des références (useCallback / useMemo)
5. Découper les effets
6. Dépendances implicites
7. Quand NE PAS utiliser useEffect
8. useEffect et React concurrent
9. Effets fantômes et Strict Mode
10. Antisèche ultra condensée

---

# 1. Qu’est‑ce que `useEffect` ?

`useEffect` permet d’exécuter du code après le rendu du composant.  
C’est le mécanisme que React met à disposition pour gérer **tout ce qui n’appartient pas au rendu pur** : on appelle cela les _effets de bord_.

Dans React, le rendu doit rester **prévisible, pur et sans conséquences** : un composant doit se contenter de calculer du JSX à partir de ses props et de son state.  
Mais une application réelle doit aussi **interagir avec le monde extérieur** : écouter des événements, lancer des timers, synchroniser des données, manipuler le DOM, effectuer des requêtes réseau, etc.

`useEffect` sert précisément à **isoler et contrôler ces effets de bord**, afin qu’ils soient exécutés au bon moment, nettoyés correctement, et qu’ils ne perturbent jamais le cycle de rendu.  
C’est cette séparation entre _rendu pur_ et _effets de bord contrôlés_ qui fait la robustesse du modèle mental de React.

## 🎯 À quoi sert un effet ?

- fetch _(privilégier une librairie comme React Query)_
- événements
- DOM
- timers
- synchronisation externe
- logs

## 🧠 Quand s’exécute un effet ?

    useEffect(fn, [])        // au montage
    useEffect(fn, [a, b])    // quand a ou b change
    useEffect(fn)            // à chaque rendu

## 🔄 Cycle d’un effet

1. Render
2. Commit
3. Effect
4. Cleanup

---

# 2. `useEffect` vs `useLayoutEffect`

## 🟦 useEffect — après la peinture

- asynchrone
- non bloquant
- idéal pour : logs, timers, synchronisation externe, requêtes réseau (via React Query)

## 🟧 useLayoutEffect — avant la peinture

- synchrone
- bloque l’affichage
- idéal pour : mesures DOM, layout

---

# 3. Bonnes pratiques pour gérer le tableau de dépendances

> Toute valeur lue dans l’effet doit être dans les dépendances.

Exemple :

    useEffect(() => {
      console.log(user.name)
    }, [user])

---

# 4. Stabiliser les références : `useCallback` et `useMemo`

## ✔️ useCallback

Stabilise une fonction :

    const fetchData = useCallback(() => {
      // ...
    }, [id])

## ✔️ useMemo

Stabilise un objet ou un tableau :

    const options = useMemo(() => ({ sort: true }), [])

---

# 5. Découper les effets trop complexes

    useEffect(updateUI, [theme])
    useEffect(fetchData, [id])
    useEffect(logEvent, [user, isLoggedIn])

---

# 6. Dépendances implicites

Stables par nature :

- setState
- constantes externes
- fonctions importées

Exemple :

    useEffect(() => {
      setCount(c => c + 1)
    }, [])

---

# 7. Quand NE PAS utiliser `useEffect`

## ❌ 1. Dériver un état

    const fullName = first + " " + last

## ❌ 2. Synchroniser une prop

Préférer un state contrôlé ou passer la prop directement.

## ❌ 3. Code à chaque rendu

Souvent un anti‑pattern.

## ❌ 4. Mise à jour DOM avant affichage

Utiliser `useLayoutEffect`.

## ❌ 5. Transitions complexes

Utiliser `useTransition` ou `useDeferredValue`.

---

# 8. `useEffect` et React concurrent

React 18 peut :

- interrompre un rendu
- recommencer un rendu
- retarder les effets
- nettoyer avant d’exécuter

Les effets ne sont plus liés 1:1 aux rendus.

---

# 9. Effets fantômes et Strict Mode

En Strict Mode, React double volontairement :

- rendu
- montage
- exécution des effets
- cleanup

Pour détecter les effets non sûrs.

Pour les éviter :

- toujours retourner un cleanup propre
- éviter les effets non idempotents
- stabiliser les références

---

# 10. Antisèche ultra condensée

### 🎯 À retenir

- **useEffect = gestion des effets de bord**
- **useLayoutEffect = effets avant peinture**
- **Règle d’or : tout ce que l’effet lit doit être dans les dépendances**
- **Stabilisation : useCallback / useMemo**
- **Strict Mode : effets doublés pour détecter les erreurs**
- **React concurrent : effets retardés, cleanups anticipés**
- **Ne pas utiliser useEffect pour : dériver un état, synchroniser une prop, transitions, DOM avant peinture**

Cycle global :

Render → Commit → Layout Effects → Effects → Cleanup
