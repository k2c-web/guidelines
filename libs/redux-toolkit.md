---
title: "Redux Toolkit"
description: "Guide complet pour utiliser Redux Toolkit dans un contexte React/Next moderne : slices, reducers, RTK Query, patterns et pièges."
updated_at: "2026-03-08"
tags:
  - State Management
  - Redux
  - Redux Toolkit
  - React
  - Next.js
  - Patterns
---

# Redux Toolkit

Redux Toolkit (RTK) est la version moderne, simplifiée et recommandée de Redux.  
Il apporte :

- une API claire
- des slices modulaires
- immer intégré
- RTK Query pour le server state
- une architecture prévisible et scalable

RTK est idéal pour :

- les applications complexes
- les équipes nombreuses
- les flows métier structurés
- les besoins de tooling (devtools, time travel)

---

# 1. Pourquoi Redux Toolkit ?

## ✔️ 1.1. Prévisible

Architecture claire : actions → reducers → store.

## ✔️ 1.2. Scalable

Parfait pour les grandes apps.

## ✔️ 1.3. Immer intégré

Tu peux muter l’état sans muter réellement.

## ✔️ 1.4. RTK Query

Un système complet pour le server state.

## ✔️ 1.5. Standard de facto

Facile à onboarder en équipe.

---

# 2. Concepts fondamentaux

## 2.1. Slice = module d’état

```js
import { createSlice } from "@reduxjs/toolkit";

const counterSlice = createSlice({
  name: "counter",
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value++;
    },
    add: (state, action) => {
      state.value += action.payload;
    },
  },
});

export const { increment, add } = counterSlice.actions;
export default counterSlice.reducer;
```

````

---

## 2.2. Store

```js
import { configureStore } from "@reduxjs/toolkit";
import counter from "./counterSlice";

export const store = configureStore({
  reducer: { counter },
});
```

---

## 2.3. useSelector / useDispatch

```js
const value = useSelector((s) => s.counter.value);
const dispatch = useDispatch();
```

---

# 3. RTK Query (server state)

## 3.1. API slice

```js
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";

export const api = createApi({
  reducerPath: "api",
  baseQuery: fetchBaseQuery({ baseUrl: "/api" }),
  endpoints: (builder) => ({
    getUser: builder.query({
      query: (id) => `/users/${id}`,
    }),
  }),
});

export const { useGetUserQuery } = api;
```

---

## 3.2. Mutation

```js
const [addTodo] = api.useAddTodoMutation();
```

---

## 3.3. Invalidation

```js
invalidatesTags: ["Todos"];
```

---

# 4. Patterns recommandés

## 4.1. Pattern : slices modulaires

```
/store/
  userSlice.ts
  uiSlice.ts
  cartSlice.ts
  api.ts
```

---

## 4.2. Pattern : actions explicites

```js
dispatch(add(5));
dispatch(increment());
```

---

## 4.3. Pattern : selectors

```js
export const selectUser = (s) => s.user.data;
```

---

## 4.4. Pattern : RTK Query + Redux Toolkit

- RTK Query → server state
- Slices → client state

---

# 5. Intégration Next.js (App Router)

## 5.1. Redux = client state

Donc :
👉 **toujours dans un client component**

---

## 5.2. Provider

```js
"use client";
import { Provider } from "react-redux";
import { store } from "./store";

export default function Providers({ children }) {
  return <Provider store={store}>{children}</Provider>;
}
```

---

## 5.3. RTK Query + RSC

RTK Query peut hydrater des données préfetchées côté serveur.

---

# 6. Pièges à éviter

## ❌ 6.1. Utiliser Redux pour tout

→ Redux = flows complexes, pas UI state simple.

---

## ❌ 6.2. Mettre trop de logique dans les reducers

→ utiliser des thunks ou RTK Query.

---

## ❌ 6.3. Ne pas découper en slices

→ store trop gros.

---

## ❌ 6.4. Utiliser Redux pour du server state simple

→ RSC ou React Query suffisent.

---

# 7. Snippets prêts à coller

## 🔹 Slice minimal

```js
export const counterSlice = createSlice({
  name: "counter",
  initialState: 0,
  reducers: {
    inc: (state) => state + 1,
  },
});
```

---

## 🔹 RTK Query — query

```js
const { data, isLoading } = useGetUserQuery(id);
```

---

## 🔹 RTK Query — mutation

```js
const [addTodo] = useAddTodoMutation();
```

---

# 🎯 Conclusion

Redux Toolkit est un outil :

- robuste
- prévisible
- scalable
- idéal pour les flows complexes
- parfait en équipe

Il complète parfaitement :

- Zustand (client state simple)
- Jotai (state atomique)
- React Query (server state)
- RSC (server-first)
````
