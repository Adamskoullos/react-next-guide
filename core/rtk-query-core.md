# RTK Query Core Pattern

Integrating RTK Query allows us to create a frontend api to manage state, mutations and async code.

Typical folder structure:

```
/app
/features
    /api
        apiSlice
    /todos
```

Within the `endpoints` function below, the passed in `builder` object provides two key methods:

1. `query` > used for `GET` requests
2. `mutation` > used for `POST`, `PATCH`, `PUT` methods

In order for the dom to re-render when the the store is updated we need to tell the `cache` to update. We do this by assigning a `tag` to the `cache`and then add tag flags to each method telling the cache to update:

- Add `tagTypes: ['todos']` to object
- Add `providesTags: ['todos']` below `GET` - `query` function to update the cache
- Add `invalidatesTags: ['todos']` below `mutation` - `query` function to update the cache

We can also add the `transformResponse` property which returns a function that takes in the `respsonse`. This allows us to organise and work with the data before adding it to the store/cache and using it in the app. The example below sorts the data.

`apiSlice`:

```js
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";

export const apiSlice = createApi({
  reducerPath: "api",
  baseQuery: fetchBaseQuery({ baseUrl: "url" }),
  tagTypes: ["todos"],
  endpoints: (builder) => ({
    getTodos: builder.query({
      query: () => "/todos",
      transformRespsonse: (res) => res.sort((a, b) => b.id - e.id),
      providesTags: ["todos"],
    }),
    addTodo: builder.mutation({
      query: (todo) => ({
        url: "/todos",
        method: "POST",
        body: todo,
      }),
      invalidatesTags: ["todos"],
    }),
    updateTodo: builder.mutation({
      query: (todo) => ({
        url: `/todos/${todo.id}`,
        method: "PATCH",
        body: todo, // just pass in the whole todo
      }),
      invalidatesTags: ["todos"],
    }),
    deleteTodo: builder.mutation({
      query: ({ id }) => ({
        url: `/todos/${id}`,
        method: "DELETE",
        body: id,
      }),
      invalidatesTags: ["todos"],
    }),
  }),
});

export const {
  useGetTodosQuery,
  useAddTodoMutation,
  useUpdateTodoMutation,
  useDeleteTodoMutation,
} = apiSlice;
```

We can then wrap the app or feature with `ApiProvider` giving it access to the `apiSlice`:

`Root index.js`

```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import { ApiProvider } from '@reduxjs/toolkit/query/react';
import { apiSlice } from './features/api/apiSlice';

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <ApiProvider api={apiSlice}>
      <App />
    </ApiProvider>
  </Reacy.StrictMode>
);

```

Then we can use the `apiSlice` hook `useGetTodosQuery` created from the method `getTodos` to undertake the `async get request` to the db and cache the data in the store.
The hook provides the`data` and some further request state properties out of the box:

```js
import { useGetTodosQuery } from "../api/apiSlice";

// inside component
const {
  data: todos,
  isLoading,
  isSuccess,
  isError,
  error,
} = useGetTodosQuery();
```

For `mutation` methods we just return the function:

```js
const [addTodo] = useAddTodoMutation();
```

Using the `mutation` functions can be done directly:

`POST`

```js
const handleSubmit = (e) => {
  e.preventDefault();
  addTodo({ userId: 1, text: newTodo, completed: false });
  setNewTodo("");
};
```

`PATCH`

```js
onChange={() => updateTodo({...todo, completed: !todo.completed})}
```

`DELETE`

```js
onClick={() => deleteTodo({id: todo.id})}
```

---
