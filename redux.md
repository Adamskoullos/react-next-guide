# Redux Set-Up

- install: `npm install redux react-redux @reduxjs/toolkit`
- Set up store:

```js
/redux
    store.js
    /features
        userReducer.js
        postsReducer.js
```

**store.js**: Initial setup of store file showing how individual files for different slices/pieces of data can be pulled into the store:

```js
import { confirgureStore } from "@reduxjs/toolkit";
import userReducer from "./features/userReducer";
import postsReducer from "./features/postsReducer";

export default configureStore({
  reducer: {
    user: userReducer,
    posts: postsReducer,
  },
});
```

- Wrap the main app with `provider` passing in the store to give all components access to data within the store:

**index.js**:

```js

```

# Redux Basic Workflows

# Redux Toolkit
