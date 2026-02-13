# **React Query SSR Prefetch & Hydration avec Next.js**

## Prérequis

- **Next.js 13.4+** (App Router obligatoire, car on utilise les React Server Components)
- **React Query 5+**
- **React 18+** (pour `cache()` et le modèle RSC)

---

## Contexte & objectif

Dans les applications modernes, on veut :

- charger les données **côté serveur** pour améliorer le SEO et le Time‑to‑Content
- éviter les loaders côté client
- réutiliser le cache côté client sans refaire un fetch inutile
- garder une architecture simple et maintenable

Ce pattern est appelé :

### 👉 **SSR Prefetch + Dehydration/Hydration Pattern**

Il est utilisé par :

- **Vercel** (dans leurs exemples officiels)
- **Shopify Hydrogen**
- **Remix / TanStack Query**
- **Airbnb / Netflix** (pattern équivalent avec leurs propres libs internes)

C’est aujourd’hui **le standard** pour faire du SSR performant avec React Query.

---

# Version REST ultra‑simplifiée (optimisée avec `cache()`)

Cette version montre comment mettre en place :

- un **prefetch côté serveur** avec React Query
- la **déshydratation** du cache
- la **réhydratation** côté client
- un **fetch REST simple**
- un **QueryClient optimisé** avec `cache()`
- **sans fuite de cache**

---

## 1. Installation

```bash
npm install @tanstack/react-query
```

---

## 2. QueryClient optimisé avec `cache()`

**Fichier : `src/lib/getQueryClient.ts`**

```ts
import { QueryClient } from "@tanstack/react-query";
import { cache } from "react";

// Un QueryClient PAR requête, mémorisé dans le même rendu RSC
export const getQueryClient = cache(() => new QueryClient());
```

---

## 3. Provider client (réhydratation)

**Fichier : `src/components/ReactQueryProvider.tsx`**

```tsx
"use client";

import { QueryClientProvider, HydrationBoundary } from "@tanstack/react-query";

export function ReactQueryProvider({ children, client, state }) {
  return (
    <QueryClientProvider client={client}>
      <HydrationBoundary state={state}>{children}</HydrationBoundary>
    </QueryClientProvider>
  );
}
```

---

## 4. Hook React Query (REST)

**Fichier : `src/features/products/useProducts.ts`**

```ts
import { useQuery } from "@tanstack/react-query";

async function fetchProducts() {
  const res = await fetch("https://fakestoreapi.com/products");
  return res.json();
}

export function useProducts() {
  return useQuery({
    queryKey: ["products"],
    queryFn: fetchProducts,
  });
}
```

---

## 5. Prefetch SSR + déshydratation

**Fichier : `src/app/page.tsx`**

```tsx
import { dehydrate } from "@tanstack/react-query";
import { getQueryClient } from "@/lib/getQueryClient";
import { ReactQueryProvider } from "@/components/ReactQueryProvider";
import ProductsList from "@/features/products/ProductsList";

async function fetchProducts() {
  const res = await fetch("https://fakestoreapi.com/products");
  return res.json();
}

export default async function Page() {
  const queryClient = getQueryClient();

  await queryClient.prefetchQuery({
    queryKey: ["products"],
    queryFn: fetchProducts,
  });

  const dehydratedState = dehydrate(queryClient);

  return (
    <ReactQueryProvider client={queryClient} state={dehydratedState}>
      <ProductsList />
    </ReactQueryProvider>
  );
}
```

---

## 6. Composant client réhydraté

**Fichier : `src/features/products/ProductsList.tsx`**

```tsx
"use client";

import { useProducts } from "./useProducts";

export default function ProductsList() {
  const { data } = useProducts();

  if (!data) return <p>Chargement...</p>;

  return (
    <div className="grid grid-cols-2 md:grid-cols-3 gap-6">
      {data.map((p) => (
        <div key={p.id} className="border rounded p-4">
          <img src={p.image} alt={p.title} className="mb-2" />
          <p>{p.title}</p>
          <p className="font-bold">{p.price} €</p>
        </div>
      ))}
    </div>
  );
}
```

---

## 7. Résumé visuel

```
SSR
 └── getQueryClient()  ← optimisé avec cache()
      └── prefetchQuery()
           └── dehydrate()
                └── HTML + cache envoyés au client

Client
 └── ReactQueryProvider
      └── HydrationBoundary
           └── useProducts()
                └── données déjà prêtes → affichage instantané
```

---

## Conclusion

Cette version offre :

- un pipeline SSR → hydration propre
- un QueryClient optimisé avec `cache()`
- zéro fuite de cache
- une structure simple et pédagogique
- une base idéale pour évoluer vers une version intermédiaire ou production‑ready
