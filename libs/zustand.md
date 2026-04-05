title: "Zustand"
description: "Guide complet pour utiliser Zustand dans un contexte React/Next moderne : patterns, bonnes pratiques, pièges et snippets."
updated_at: "2026-03-08"
tags:

- State Management
- React
- Next.js
- Zustand
- Patterns

---

# Zustand

Zustand est une bibliothèque de state management minimaliste, rapide et modulaire.  
Elle est idéale pour gérer du **client state global**, sans complexité inutile.

Ce document couvre :

- les concepts clés
- les patterns recommandés
- les pièges à éviter
- les snippets prêts à coller
- l’intégration avec Next.js App Router

---

# 1. Pourquoi Zustand ?

## ✔️ 1.1. Simple

Une API minuscule, facile à apprendre.

## ✔️ 1.2. Performant

- pas de re-render global
- selectors précis
- stores modulaires

## ✔️ 1.3. Flexible

- logique métier
- stores multiples
- middlewares (persist, immer, devtools)

## ✔️ 1.4. Parfait pour le client state

- UI state global
- préférences utilisateur
- interactions complexes
- logique métier non liée au serveur

---

# 2. Concepts fondamentaux

## 2.1. Un store = une fonction create()

```js
import { create } from "zustand";

const useStore = create((set) => ({
  count: 0,
  inc: () => set((s) => ({ count: s.count + 1 })),
}));
```

## 2.2. Sélecteurs

Zustand ne re-render que si la valeur sélectionnée change.

```js
const count = useStore((s) => s.count);
```

## 2.3. Stores multiples

Zustand encourage la modularité.

```js
const useCart = create(...)
const useTheme = create(...)
```

## 2.4. Middlewares

- `persist`
- `immer`
- `devtools`

---

# 3. Patterns recommandés

## 3.1. Pattern “slice” (modulaire)

```js
const createUserSlice = (set) => ({
  user: null,
  setUser: (u) => set({ user: u }),
});

const createUIStore = (set) => ({
  sidebarOpen: false,
  toggleSidebar: () => set((s) => ({ sidebarOpen: !s.sidebarOpen })),
});

export const useStore = create((set) => ({
  ...createUserSlice(set),
  ...createUIStore(set),
}));
```

---

## 3.2. Pattern “immer”

```js
import { create } from "zustand";
import { immer } from "zustand/middleware/immer";

const useStore = create(
  immer((set) => ({
    todos: [],
    addTodo: (text) =>
      set((s) => {
        s.todos.push({ id: Date.now(), text });
      }),
  })),
);
```

---

## 3.3. Pattern “persist”

```js
import { create } from "zustand";
import { persist } from "zustand/middleware";

const useTheme = create(
  persist(
    (set) => ({
      theme: "light",
      toggle: () =>
        set((s) => ({ theme: s.theme === "light" ? "dark" : "light" })),
    }),
    { name: "theme-storage" },
  ),
);
```

---

## 3.4. Pattern “selector stable”

Toujours extraire les actions **hors** du composant.

```js
const inc = useStore((s) => s.inc);
```

---

## 3.5. Pattern “store local + global”

Zustand n’est pas là pour remplacer l’état local.

```js
const [open, setOpen] = useState(false); // local
const theme = useTheme((s) => s.theme); // global
```

---

# 4. Intégration Next.js (App Router)

## 4.1. Zustand = client state

Donc :  
👉 **toujours dans un client component**

```js
"use client";
```

## 4.2. Pas de store pour du server state

→ utiliser RSC + fetch + revalidate  
→ ou TanStack Query si besoin d’invalidation

## 4.3. Hydration safe

Zustand est SSR‑safe si tu n’utilises pas `persist`.

---

# 5. Pièges à éviter

## ❌ 5.1. Mettre tout dans un seul store

→ découper en slices  
→ stores multiples

## ❌ 5.2. Utiliser Zustand pour du server state

→ utiliser RSC ou TanStack Query

## ❌ 5.3. Muter l’état sans immer

```js
s.todos.push(todo); // ❌
```

## ❌ 5.4. Utiliser des selectors instables

```js
useStore((s) => ({ a: s.a, b: s.b })); // ❌
```

## ❌ 5.5. Mettre des objets complexes dans le store

→ préférer des IDs  
→ éviter les références instables

---

# 6. Snippets prêts à coller

## 🔹 Store minimal

```js
export const useCounter = create((set) => ({
  count: 0,
  inc: () => set((s) => ({ count: s.count + 1 })),
}));
```

---

## 🔹 Store avec immer

```js
export const useTodos = create(
  immer((set) => ({
    todos: [],
    add: (text) =>
      set((s) => {
        s.todos.push({ id: Date.now(), text });
      }),
  })),
);
```

---

## 🔹 Store persisté

```js
export const useTheme = create(
  persist(
    (set) => ({
      theme: "light",
      toggle: () =>
        set((s) => ({ theme: s.theme === "light" ? "dark" : "light" })),
    }),
    { name: "theme" },
  ),
);
```

---

## 🔹 Store modulaire (slices)

```js
const createUserSlice = (set) => ({
  user: null,
  setUser: (u) => set({ user: u }),
});

const createCartSlice = (set) => ({
  items: [],
  add: (item) => set((s) => ({ items: [...s.items, item] })),
});

export const useStore = create((set) => ({
  ...createUserSlice(set),
  ...createCartSlice(set),
}));
```

---

# 🎯 Conclusion

Zustand est un outil :

- simple
- performant
- modulaire
- idéal pour le client state
- parfaitement compatible avec Next.js App Router

Il doit être utilisé pour :

- l’état global **non serveur**
- la logique métier
- les interactions complexes
- les préférences utilisateur
- les stores modulaires

Il ne doit **pas** remplacer :

- l’état local
- le server state
- TanStack Query
- les RSC
