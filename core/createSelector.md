## RTK createSelector

We can create memoized selectors with either `createSelector` or `createEntityAdapter`. If we are not using `createEntityAdapter` to provide: memoized selectors, a normalized data structure or using the adapter methods to maintain the normalized data structure and only want to use memoized selectors we can use `createSelector`.

We can also use `createSelector` in conjunction with `createEntityAdapter` to create computed selectors from adapter selectors.

---

### Advantages

There are two main reasons to use `createSelector`:

1. To increase efficiency with the use of memoized selectors
2. To create computed memoized selectors

---

### Structure

`createSelector` takes two types of arguments:

1. `input` selectors > can be a single input selector or multiple
2. `output` selector > the last argument is the output selector

The example below creates two normal `input` selectors which are passed in as the first two arguments to `createSelector`.
The return value of these input selector functions are then passed in as the arguments (in the respective order) to the `output` selector. The returned value from the output selector is the value that is returned from `selectTax` when it is called.

If the returned value from the `input` selectors has not changed, the `output` selector function will return the memoized previous value.

```js
export const selectShopItems = (state) => state.shop.items;

export const selectSubtotal = createSelector(selectShopItems, (items) =>
  items.reduce((subtotal, item) => subtotal + item.value, 0)
);

export const selectTaxPercent = (state) => state.shop.taxPercent;

export const selectTax = createSelector(
  selectSubtotal,
  selectTaxPercent,
  (subtotal, taxPercent) => subtotal * (taxPercent / 100)
);
```

---

### RTK Pattern

The below example takes in two input functions one of which is a selector itself. The output function only re-runs if the returned values from the input functions change, otherwise the previous memoized value form the output function is returned.

`postsSlice`

```js
import { createSlice, createSelector } from "@reduxjs/toolkit";

// At the bottom of the file where all the selectors are

// Normal selector
export const selectAllPosts = (state) => state.posts.posts;

// Memoized computed selector
export const selectPostsByUser = createSelector(
  selectAllPosts, // input function (normal selector above)
  (state, userId) => userId, // input function simply returning the passed in id
  (posts, userId) => posts.filter((post) => post.userId === userId) // output function
);
```

`UserPosts`

```js
import { useSelector } from "react-redux";

import { selectPostsByUser } from "../posts/postsSlice";

import { useParams } from "react-router-dom";

// inside component function

const userId = useParams();

const userPosts = useSelector((state) =>
  selectPostsByUser(state, Number(userId))
);
```
