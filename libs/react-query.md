---
title: "React Query (TanStack Query)"
description: "Guide complet pour gérer le server state avec React Query dans un contexte React/Next moderne : caching, invalidation, mutations, patterns et pièges."
updated_at: "2026-03-08"
tags:
  - State Management
  - Server State
  - React
  - Next.js
  - React Query
  - TanStack Query
---

# React Query (TanStack Query)

React Query est la bibliothèque de référence pour gérer le **server state** dans React/Next.  
Elle apporte un système complet de :

- caching
- synchronisation
- invalidation
- pagination
- mutations
- optimistic UI
- retry
- stale‑while‑revalidate

Ce document couvre :

- les concepts clés
- les patterns recommandés
- l’intégration Next.js App Router
- les pièges à éviter
- les snippets prêts à coller

---

# 1. Pourquoi React Query ?

## ✔️ 1.1. Le server state est différent du client state

- il vient du serveur
- il peut être obsolète
- il doit être synchronisé
- il doit être revalidé
- il peut être partagé entre pages

React Query est conçu **exactement** pour ça.

---

## ✔️ 1.2. Caching intelligent

- stale time
- cache time
- garbage collection
- background refetch

---

## ✔️ 1.3. Invalidation ciblée

Invalidate **juste ce qu’il faut**, pas tout.

---

## ✔️ 1.4. Mutations puissantes

- optimistic updates
- rollback
- retry
- invalidation automatique

---

## ✔️ 1.5. Parfait pour Next.js App Router

- compatible RSC
- hydration
- streaming
- server prefetch

---

# 2. Concepts fondamentaux

## 2.1. Query = lecture (GET)

```js
const { data, isLoading } = useQuery({
  queryKey: ["user", id],
  queryFn: () => fetch(`/api/users/${id}`).then((r) => r.json()),
});
```

---

## 2.2. Mutation = écriture (POST/PUT/DELETE)

```js
const mutation = useMutation({
  mutationFn: (payload) =>
    fetch("/api/todos", { method: "POST", body: JSON.stringify(payload) }),
});
```

---

## 2.3. Query Key = identifiant unique

Toujours stable, toujours structuré.

```js
["todos", { filter: "active" }];
```

---

## 2.4. Stale vs Fresh

- **fresh** = pas besoin de refetch
- **stale** = peut être refetché en background

---

## 2.5. Invalidation

Permet de recharger uniquement ce qui doit l’être.

```js
queryClient.invalidateQueries({ queryKey: ["todos"] });
```

---

# 3. Patterns recommandés

## 3.1. Pattern : query key structurée

```js
const todoKeys = {
  all: ["todos"],
  list: (filter) => ["todos", "list", { filter }],
  detail: (id) => ["todos", "detail", id],
};
```

---

## 3.2. Pattern : fetcher générique

```js
const fetcher = (url) => fetch(url).then((r) => r.json());

useQuery({
  queryKey: ["user", id],
  queryFn: () => fetcher(`/api/users/${id}`),
});
```

---

## 3.3. Pattern : optimistic update

```js
const mutation = useMutation({
  mutationFn: updateTodo,
  onMutate: async (updated) => {
    await queryClient.cancelQueries(["todos"]);
    const previous = queryClient.getQueryData(["todos"]);
    queryClient.setQueryData(["todos"], (old) =>
      old.map((t) => (t.id === updated.id ? updated : t)),
    );
    return { previous };
  },
  onError: (_err, _new, ctx) => {
    queryClient.setQueryData(["todos"], ctx.previous);
  },
  onSettled: () => {
    queryClient.invalidateQueries(["todos"]);
  },
});
```

---

## 3.4. Pattern : pagination

```js
useQuery({
  queryKey: ["posts", page],
  queryFn: () => fetch(`/api/posts?page=${page}`).then((r) => r.json()),
  keepPreviousData: true,
});
```

---

## 3.5. Pattern : infinite query

```js
useInfiniteQuery({
  queryKey: ["feed"],
  queryFn: ({ pageParam = 0 }) =>
    fetch(`/api/feed?cursor=${pageParam}`).then((r) => r.json()),
  getNextPageParam: (lastPage) => lastPage.nextCursor,
});
```

---

# 4. Intégration Next.js (App Router)

## 4.1. React Query = client state du server state

Donc :  
👉 **toujours dans un client component**

```js
"use client";
```

---

## 4.2. Prefetch côté serveur (RSC)

```js
// page.tsx (server component)
import { dehydrate, QueryClient } from "@tanstack/react-query";
import Hydrate from "./hydrate";

export default async function Page() {
  const queryClient = new QueryClient();

  await queryClient.prefetchQuery({
    queryKey: ["todos"],
    queryFn: () => fetch("https://...").then((r) => r.json()),
  });

  return (
    <Hydrate state={dehydrate(queryClient)}>
      <Todos />
    </Hydrate>
  );
}
```

---

## 4.3. Hydration côté client

```js
"use client";
import { Hydrate as RQHydrate } from "@tanstack/react-query";

export default function Hydrate({ children, state }) {
  return <RQHydrate state={state}>{children}</RQHydrate>;
}
```

---

## 4.4. Quand utiliser React Query dans Next.js ?

### ✔️ Oui si :

- données dynamiques
- invalidation nécessaire
- mutations
- pagination
- infinite scroll
- optimistic UI

### ❌ Non si :

- données statiques
- données peu changeantes
- données déjà gérées par RSC
- pas de mutation

---

# 5. Pièges à éviter

## ❌ 5.1. Utiliser React Query pour du client state

→ utiliser Zustand ou l’état local

---

## ❌ 5.2. Mauvaise query key

```js
["todos", filter]; // ❌ instable si filter est un objet
```

---

## ❌ 5.3. Mettre des fonctions dans les query keys

→ interdit

---

## ❌ 5.4. Utiliser React Query pour remplacer RSC

→ RSC doit rester la source de vérité

---

## ❌ 5.5. Oublier l’invalidation après mutation

→ données obsolètes

---

# 6. Snippets prêts à coller

## 🔹 Query simple

```js
useQuery({
  queryKey: ["user", id],
  queryFn: () => fetch(`/api/users/${id}`).then((r) => r.json()),
});
```

---

## 🔹 Mutation simple

```js
const mutation = useMutation({
  mutationFn: (payload) =>
    fetch("/api/todos", {
      method: "POST",
      body: JSON.stringify(payload),
    }),
});
```

---

## 🔹 Invalidation

```js
queryClient.invalidateQueries({ queryKey: ["todos"] });
```

---

## 🔹 Infinite Query

```js
useInfiniteQuery({
  queryKey: ["feed"],
  queryFn: ({ pageParam = 0 }) =>
    fetch(`/api/feed?cursor=${pageParam}`).then((r) => r.json()),
  getNextPageParam: (lastPage) => lastPage.nextCursor,
});
```

---

# 🎯 Conclusion

React Query est l’outil idéal pour gérer le **server state** dans React/Next :

- caching intelligent
- invalidation ciblée
- mutations puissantes
- pagination
- infinite scroll
- optimistic UI
- intégration parfaite avec Next.js App Router

Il complète parfaitement :

- **Zustand** (client state)
- **RSC** (server state statique)
- **Next.js fetch + revalidate**

C’est un pilier du niveau senior.
