# RTK Query

- [Core Pattern](#Core-Pattern)
- [Porting From Reducers](#Porting-From-Reducers)

---

## Core Pattern

Integrating RTK Query allows us to create a frontend api to manage state, mutations and async code.

Typical folder structure:

```
/app
/features
    /api
        apiSlice
    /todos
```

`apiSlice`:

```js
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";

export const apiSlice = createApi({
  reducerPath: "api",
  baseQuery: fetchBaseQuery({ baseUrl: "url" }),
  endpoints: (builder) => ({
    getTodos: builder.query({
      query: () => "/todos",
    }),
  }),
});
```

---

## Porting From Reducers

---
