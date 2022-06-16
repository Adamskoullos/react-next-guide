## RTK Query API Advanced

- [Core Pattern](#Core-Pettern)
- [Tags](Tags)

---

## Core Pattern

The pattern creates an app wide `apiSlice` that incorporates multiple `slices` and takes advantage of normailised data and selectors with the use of the `createEntityAdapter` api. This also allows us to ulitlise the adapter methods within each slice maintaining the normalised data structure.

Folder structure:

In the below example both the `postsSlice` and `usersSlice` have there own `endpoints` and methods which are injected into the main `apiSlice`.

```
/features
  /api
    apiSlice.js
  /posts
    postsSlice.js
  /users
    usersSlice.js
```

---

We create the `apiSlice` as normal but we do not add any enpoints here, instead we create `extendedApiSlices` and inject them into the `apiSlice` endpoints object.

`apiSlice.js`

```js
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";

export const apiSlice = createApi({
  reducerPath: "api",
  baseQuery: fetchBaseQuery({ baseUrl: "url" }),
  tagTypes: ["Post", "User"],
  endpoints: (builder) => ({}),
});
```

We add the `apiSlice` to the store and add the apiSlice middleware to the default middleware array:

```js
import { configureStore } from "@reduxjs/toolkit";
import { apiSlice } from "../features/api/apiSlice";

export const store = configureStore({
  reducer: {
    [apiSlice.reducerPath]: apiSlice.reducer,
  },
  middleware: getDefaultMiddleware => getDefaultMiddleware().concat(apiSlice.middleware);
});
```

Then we can wrap the app with the store provider as normal:

```js
ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <Router>
        <Routes>
          <Route path="/*" element={<App />} />
        </Routes>
      </Router>
    </Provider>
  </React.StrictMode>,
  document.getElementById("root")
);
```

We then create a slice as normal but instead of using `createSlice` and using reducers, we use `apiSlice.injectEndpoints({...})` which we pass an object into with the `endpoints` for that slice. This allows for different slices/endpoints for each data/feature to be injected into the main app wide `apiSlice`.

`postsSlice.js`
Even though we are still calling the file a slice, actually we have one `apiSlice` and then create slice files for each endpoints object we create. This file contains all the same stuff a normal slice would: adapter, selectors, initial state and of course the endpoint functions, which would normally be reducer function.

Another key aspect to this pattern is the use of `createEntityAdapter`, we initially create a `postsAdapter` also configuring its `sortComparer` method.
We then create a `normalised` `initialState`, then once we get the posts using `getPosts` within the `transformResponse` we return the data via `postsAdapter.setAll(initialState, loadedPosts)` adding normalised data to the store. Each `mutation` function

```js
import { createSelector, createEntityAdapter } from "@reduxjs/toolkit";
import { sub } from "date-fns";
import { apiSlice } from "../api/apiSlice";

const postsAdapter = createEntityAdapter({
  sortComparer: (a, b) => b.date.localeCompare(a.date),
});

const initialState = postsAdapter.getInitialState();

export const extendedApiSlice = apiSlice.injectEndpoints({
  endpoints: (builder) => ({
    getPosts: builder.query({
      query: () => "/posts",
      transformResponse: (responseData) => {
        let min = 1;
        const loadedPosts = responseData.map((post) => {
          if (!post?.date)
            post.date = sub(new Date(), { minutes: min++ }).toISOString();
          if (!post?.reactions)
            post.reactions = {
              thumbsUp: 0,
              wow: 0,
              heart: 0,
              rocket: 0,
              coffee: 0,
            };
          return post;
        });
        return postsAdapter.setAll(initialState, loadedPosts);
      },
      providesTags: (result, error, arg) => [
        { type: "Post", id: "LIST" },
        ...result.ids.map((id) => ({ type: "Post", id })),
      ],
    }),
    getPostsByUserId: builder.query({
      query: (id) => `/posts/?userId=${id}`,
      transformResponse: (responseData) => {
        let min = 1;
        const loadedPosts = responseData.map((post) => {
          if (!post?.date)
            post.date = sub(new Date(), { minutes: min++ }).toISOString();
          if (!post?.reactions)
            post.reactions = {
              thumbsUp: 0,
              wow: 0,
              heart: 0,
              rocket: 0,
              coffee: 0,
            };
          return post;
        });
        return postsAdapter.setAll(initialState, loadedPosts);
      },
      providesTags: (result, error, arg) => [
        ...result.ids.map((id) => ({ type: "Post", id })),
      ],
    }),
    addNewPost: builder.mutation({
      query: (initialPost) => ({
        url: "/posts",
        method: "POST",
        body: {
          ...initialPost,
          userId: Number(initialPost.userId),
          date: new Date().toISOString(),
          reactions: {
            thumbsUp: 0,
            wow: 0,
            heart: 0,
            rocket: 0,
            coffee: 0,
          },
        },
      }),
      invalidatesTags: [{ type: "Post", id: "LIST" }],
    }),
    updatePost: builder.mutation({
      query: (initialPost) => ({
        url: `/posts/${initialPost.id}`,
        method: "PUT",
        body: {
          ...initialPost,
          date: new Date().toISOString(),
        },
      }),
      invalidatesTags: (result, error, arg) => [{ type: "Post", id: arg.id }],
    }),
    deletePost: builder.mutation({
      query: ({ id }) => ({
        url: `/posts/${id}`,
        method: "DELETE",
        body: { id },
      }),
      invalidatesTags: (result, error, arg) => [{ type: "Post", id: arg.id }],
    }),
    addReaction: builder.mutation({
      query: ({ postId, reactions }) => ({
        url: `posts/${postId}`,
        method: "PATCH",
        // In a real app, we'd probably need to base this on user ID somehow
        // so that a user can't do the same reaction more than once
        body: { reactions },
      }),
      async onQueryStarted(
        { postId, reactions },
        { dispatch, queryFulfilled }
      ) {
        // `updateQueryData` requires the endpoint name and cache key arguments,
        // so it knows which piece of cache state to update
        const patchResult = dispatch(
          extendedApiSlice.util.updateQueryData(
            "getPosts",
            undefined,
            (draft) => {
              // The `draft` is Immer-wrapped and can be "mutated" like in createSlice
              const post = draft.entities[postId];
              if (post) post.reactions = reactions;
            }
          )
        );
        try {
          await queryFulfilled;
        } catch {
          patchResult.undo();
        }
      },
    }),
  }),
});

export const {
  useGetPostsQuery,
  useGetPostsByUserIdQuery,
  useAddNewPostMutation,
  useUpdatePostMutation,
  useDeletePostMutation,
  useAddReactionMutation,
} = extendedApiSlice;

// returns the query result object
export const selectPostsResult = extendedApiSlice.endpoints.getPosts.select();

// Creates memoized selector
const selectPostsData = createSelector(
  selectPostsResult,
  (postsResult) => postsResult.data // normalized state object with ids & entities
);

//getSelectors creates these selectors and we rename them with aliases using destructuring
export const {
  selectAll: selectAllPosts,
  selectById: selectPostById,
  selectIds: selectPostIds,
  // Pass in a selector that returns the posts slice of state
} = postsAdapter.getSelectors(
  (state) => selectPostsData(state) ?? initialState
);
```

---

We then ammend the root `index.js` file if we want to load the data on initial app load:

```js
import { extendedApiSlice } from "./features/posts/postsSlice";

store.dispatch(extendedApiSlice.endpoints.getPosts.initiate());
```

---

Then within the posts component the app has already made the call to get the posts, but we can use the `useGetPostsQuery` to grab the request status props and then also import `selectPostIds` selector to be used with `useSelector` to grab the ordered ids array, which is then used to create a list of post excerpts.

We could grab the data directly from `useGetPostsQuery` but doing it this way via a selector allows the soerting logic to be extracted away within the slice.

`PostsList`

```js
import { useSelector } from "react-redux";
import { selectPostIds } from "./postsSlice";
import PostsExcerpt from "./PostsExcerpt";
import { useGetPostsQuery } from "./postsSlice";

const PostsList = () => {
  const { isLoading, isSuccess, isError, error } = useGetPostsQuery();

  const orderedPostIds = useSelector(selectPostIds);

  let content;
  if (isLoading) {
    content = <p>"Loading..."</p>;
  } else if (isSuccess) {
    content = orderedPostIds.map((postId) => (
      <PostsExcerpt key={postId} postId={postId} />
    ));
  } else if (isError) {
    content = <p>{error}</p>;
  }

  return <section>{content}</section>;
};
export default PostsList;
```

---

## Tags

### Providing Tags

### Invalidating Tags

### Tag helper functions
