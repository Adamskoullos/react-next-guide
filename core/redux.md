# Redux Set-Up

The below model utilises redux toolkit.

- install: `npm install redux react-redux @reduxjs/toolkit`
- Set up store:

```js
/redux
    store.js
    /features
        userReducer.js
        postsReducer.js
```

**store.js**:

Initial setup of store file showing how individual files for different slices/pieces of data can be pulled into the store:

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

- Import `store.js` into the root `index.js` along with `Provider` from redux
- Wrap the main app with `Provider` passing in the `store` to give all components access to data within the store object:

**index.js**:

```js
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import { store } from "./redux/store";
import { Provider } from "react-redux";

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```

# Slices

The `store` is modularised into separate files/modules which are called `slices`. Each slice contains:

- **State** > Data within the slice with its initial value
- **Reducers** > Contain Actions/methods
- **Actions** > Methods that update the state

The below example straight form the redux toolkit docs shows how a `slice` is structured:

```js
import { createSlice } from '@reduxjs/toolkit'


export const counterSlice = createSlice({
  name: 'counter',
  initialState: {
      count: 0
  }
  reducers: {
    increment: (state) => {
      state.count += 1
    },
    decrement: (state) => {
      state.count -= 1
    },
    incrementByAmount: (state, action) => {
      state.count += action.payload
    },
  },
})

// Action creators are generated for each case reducer function
export const { increment, decrement, incrementByAmount } = counterSlice.actions

export default counterSlice.reducer
```

# Working with slices within components

Within a component we import `useSelector` and `useDispatch` from redux and any slices, in this case we are destructuring `decrement` and `increment` from `counterSlice`

We can then grab the state by using `useSelector` in this case `count`, and also grab the `dispatch` method.

We then `dispatch` an action by passing the `action` in as an argument.

The example below shows how to display state and also how to dispatch an action:

```js
import React from "react";
import { useSelector, useDispatch } from "react-redux";
import { decrement, increment } from "./counterSlice";

export function Counter() {
  const { count } = useSelector((state) => state.counter);
  const dispatch = useDispatch();

  return (
    <div>
      <div>
        <button
          aria-label="Increment value"
          onClick={() => dispatch(increment())}
        >
          Increment
        </button>
        <span>{count}</span>
        <button
          aria-label="Decrement value"
          onClick={() => dispatch(decrement())}
        >
          Decrement
        </button>
      </div>
    </div>
  );
}
```
