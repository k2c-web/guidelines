---
title: "Optimisations & Performances en React moderne"
description: "Guide complet des optimisations React : lazy loading, mémoïsation, transitions concurrentes, virtualisation, React Query, RSC, Profiler, et futur de React (React Forget)."
author: "Kamil Cassam-Chenaï"
tags:
  - react
  - performance
  - optimisation
  - javascript
  - frontend
  - bonnes-pratiques
updated_date: "2026-03-02"
---

# ⚡ Optimisations & Performances en React moderne

## 📑 Sommaire

1. Introduction : pourquoi optimiser ?
2. Lazy loading & code splitting
3. Mémoïsation des composants (React.memo)
4. Mémoïsation des valeurs et fonctions (useMemo / useCallback)
5. Optimisation du rendu
6. Optimisation des listes
7. Optimisation des interactions (useTransition / useDeferredValue)
8. Optimisation du réseau (React Query & co)
9. Optimisation du DOM
10. Optimisation des animations
11. Optimisation des images
12. Optimisations avancées
13. Anti‑patterns de performance
14. Futur de React : React Forget (compiler)
15. Antisèche ultra condensée

---

# 1. Introduction : pourquoi optimiser ?

React n’est pas lent.  
Ce qui est coûteux, ce sont :

- les **re-renders inutiles**,
- les **calculs lourds**,
- les **listes volumineuses**,
- les **objets/fonctions instables**,
- les **fetch non optimisés**,
- les **mesures DOM répétées**,
- les **animations JS mal gérées**.

## 🎯 Objectif de l’optimisation

- réduire les re-renders
- réduire le travail du navigateur
- réduire le JS chargé
- améliorer la fluidité des interactions
- améliorer la perception utilisateur

## 🧠 Quand optimiser ?

- quand un composant rerend trop souvent
- quand une interaction lag
- quand une liste est trop lourde
- quand un calcul est coûteux
- quand le bundle JS est trop gros

## ❌ Quand NE PAS optimiser ?

- quand le code est déjà fluide
- quand l’optimisation ajoute de la complexité inutile
- quand tu n’as pas mesuré le problème

> **Mesurer → Comprendre → Optimiser.**

---

# 2. Lazy loading & code splitting

## 🎯 Objectif

Charger **moins de JavaScript** au démarrage.

## ✔️ React.lazy

    const Page = React.lazy(() => import('./Page'))

## ✔️ Suspense

    <Suspense fallback={<Spinner />}>
      <Page />
    </Suspense>

## ✔️ Dynamic import

    const Chart = dynamic(() => import('./Chart'))

## ✔️ Split par route (Next.js)

Automatique dans App Router.

## ✔️ Split par composant

Utile pour :

- éditeurs (Monaco, TipTap)
- graphiques (Recharts, D3)
- cartes (Leaflet, Mapbox)
- libs 3D (Three.js)

---

# 3. Mémoïsation des composants (React.memo)

## ✔️ React.memo

Évite les re-renders si les props n’ont pas changé.

    const Button = React.memo(function Button(props) {
      return <button>{props.label}</button>
    })

## ✔️ Comparaison personnalisée

    React.memo(Component, (prev, next) => prev.id === next.id)

## ❌ Quand NE PAS utiliser React.memo

- composant très simple
- composant rarement rendu
- props instables (objets/fonctions inline)
- si ça complique la lecture

---

# 4. Mémoïsation des valeurs et fonctions

## ✔️ useMemo

Évite de recalculer une valeur coûteuse.

    const filtered = useMemo(() => heavyFilter(list), [list])

## ✔️ useCallback

Stabilise une fonction.

    const handleClick = useCallback(() => {
      doSomething(id)
    }, [id])

## ❌ Mauvais usages

- calculs triviaux
- primitives
- optimisation “par réflexe”

## ✔️ Profiling

Toujours mesurer avant d’optimiser.

---

# 5. Optimisation du rendu

## ✔️ Batching automatique (React 18)

Plusieurs setState → un seul rendu.

## ✔️ Découper les composants

Un composant = une responsabilité.

## ✔️ Pure rendering

Composants stables + props stables = re-renders minimisés.

## ❌ Objets inline

    <List options={{ sort: true }} />

→ recréé à chaque rendu.

## ✔️ Solution

    const options = useMemo(() => ({ sort: true }), [])

---

# 6. Optimisation des listes

## ✔️ Keys stables

    key={item.id}

## ✔️ Virtualisation

- React Window
- React Virtualized
- TanStack Virtual

## ✔️ Pagination / infinite scroll

Évite de charger 10 000 items.

## ✔️ Lazy rendering

Rendre seulement ce qui est visible.

---

# 7. Optimisation des interactions

## ✔️ useTransition

Pour les mises à jour **non urgentes**.

    const [isPending, startTransition] = useTransition()

    startTransition(() => {
      setFiltered(heavyFilter(list, query))
    })

## ✔️ useDeferredValue

Pour différer une valeur dérivée lourde.

    const deferredQuery = useDeferredValue(query)

---

# 8. Optimisation du réseau

## ❌ Pourquoi éviter fetch dans useEffect ?

- double fetch en Strict Mode
- pas de cache
- pas de déduplication
- pas de retry
- pas d’invalidation
- race conditions

## ✔️ React Query / SWR / TanStack Query

- cache
- revalidation
- retry
- déduplication
- Suspense-ready
- optimistic updates

---

# 9. Optimisation du DOM

## ✔️ useLayoutEffect

Pour les mesures DOM.

## ❌ Eviter les forced reflows

- offsetHeight
- getBoundingClientRect
- scrollTop

## ✔️ Patterns propres

- regrouper les lectures DOM
- regrouper les écritures DOM

---

# 10. Optimisation des animations

## ✔️ CSS > JS

Toujours.

## ✔️ requestAnimationFrame

Pour les animations JS.

## ✔️ Framer Motion

Optimisé, performant.

## ✔️ Transitions concurrentes

Fluidifie les interactions lourdes.

---

# 11. Optimisation des images

## ✔️ Formats modernes

- WebP
- AVIF

## ✔️ Lazy loading natif

    <img loading="lazy" />

## ✔️ Next.js Image

Optimisation automatique.

---

# 12. Optimisations avancées

## ✔️ Profiler React DevTools

Pour mesurer les re-renders.

## ✔️ Profiler Chrome

Pour mesurer le coût JS / layout / paint.

## ✔️ Suspense + streaming

Rendu progressif.

## ✔️ Server Components (RSC)

- moins de JS côté client
- moins de re-renders
- data-fetching optimisé

## ✔️ Edge rendering

Plus rapide, plus proche de l’utilisateur.

## ✔️ Offscreen rendering (futur React)

Préparer des UI en arrière-plan.

---

# 13. Anti‑patterns de performance

- sur‑mémoïsation
- useEffect pour dériver des valeurs
- re-renders en cascade
- state global inutile
- context utilisé comme store
- composants trop gros
- refs utilisées comme cache

---

# 14. Futur de React : React Forget (compiler)

## 🎯 Idée clé

React Forget est un **compiler** qui ajoute automatiquement :

- useMemo
- useCallback
- React.memo

Tu écris du code simple → il génère du code optimisé.

## ✔️ Ce que Forget fait

- stabilise les fonctions
- stabilise les objets/tableaux
- évite les re-renders inutiles
- optimise mieux que les humains
- supprime les erreurs de dépendances

## ✔️ Ce que ça change

- moins de code
- code plus lisible
- plus de performance
- moins d’erreurs humaines

## ✔️ État actuel

- utilisé chez Meta
- intégré dans React 19 (expérimental)
- bientôt activé par défaut
- Next.js va l’intégrer automatiquement

---

# 15. Antisèche ultra condensée

- **memo** pour éviter les re-renders
- **useMemo** pour les calculs lourds
- **useCallback** pour stabiliser les fonctions
- **useTransition** pour les mises à jour non urgentes
- **lazy** pour charger moins de JS
- **React Query** pour le réseau
- **virtualisation** pour les grandes listes
- **Profiler** pour mesurer avant d’optimiser
- **React Forget** va automatiser la mémoïsation
