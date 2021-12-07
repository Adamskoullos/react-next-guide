# Hooks

- [useState](#useState)
- [useReducer](#useReducer)
- [useEffect](#useEffect)
- [useRef](#useRef)
- [useLayoutEffect](#useLayoutEffect)
- [useImperativeHandle](#useImperativeHandle)#
- [useContext](#useContext)
- [useMemo](#useMemo)
- [useCallback](#useCallback)

---

# useState

`useState` allows us to make data responsive by triggering the component to re-render every time the `useState` data changes.

```js
import { useState } from "react";

const Home = () => {
  const [name, setName] = useState("Dave");

  const handleClick = (e) => {
    setName("John");
  };

  return (
    <div className="home">
      <h2>Home Page</h2>
      <h3>{name}</h3>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
};

export default Home;
```

---

# useReducer

`useReducer` works in a similar way to `useState` in that when the value changes, the dom is rerendered. However where `useState` handles a single piece of data, `useReducer` can handle multiple pieces of data. This is useful if a single function updates multiple properties.

The pattern looks like this:

1. Import: `import { useReducer } from 'react';`
2. Create the `reducer` function.
3. Use the reducer to create the state within the component and also access the reducer via the `dispatch()` method within components to update properties and rerender the dom

```js
import { useReducer } from "react";

// state is an object
const reducer = (state, action) => {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1, showText: state.showText };
    case "toggleShowText":
      return { count: state.count, showText: !state.showText };
    default:
      return state;
  }
};
// Inside component function
// Second argument within useReducer is the state object with initial property values
const [state, dispatch] = useReducer(reducer, { count: 0, showText: false });
// Inside jsx template
<button
  onClick={() => {
    dispatch({ type: "increment" });
    dispatch({ typr: "toggleShowText" });
  }}
>
  Update Dom
</button>;
```

---

# useEffect

The `useEffect` hook is a function that fires every time the dom updates. It fires once on initial page/component load and then each time the dom is updated.

```js
import { useState, useEffect } from "react";
```

```js
useEffect(() => {
  // Code to run
});
```

### Dependencies of useEffect

We can control at a more granular level when the `useEffect` hooks fires by adding an array as the second argument:

1. If we only want the hook to fire after the initial page/component load and not on subsequent updates, we add an empty array as the second argument:

```js
useEffect(() => {
  // Code to run
}, []);
```

2. If we only want `useEffect` to fire when specific properties change we can add those properties to the array:

```js
useEffect(() => {
  // Code to run
}, [dataOne, dataTwo]);
```

### Fetching data with useEffect

1. set `useEffect` to null or an empty array:

```js
const [blogs, setBlogs] = useState(null);
```

2. we cannot use `async`/`await` directly within `useEffect`, the simple example below uses `fetch` and omits error handling:

```js
useEffect(() => {
  fetch("http://localhost:8000/blogs")
    .then((res) => {
      return res.json();
    })
    .then((data) => {
      setBlogs(data);
    });
}, []);
```

3. Then to prevent errors from data being initially null we wrap any code with conditional logic to prevent rendering until the property has a value:

```js
return (
  <div className="home">
    {blogs && (
      <BlogList blogs={blogs} title="All Blogs" handleDelete={handleDelete} />
    )}
  </div>
);
```

### useEffect Cleanup

Should the user click away from a page whilst a request is still in process we can programmatically `abort` the request to prevent errors:

1. Create an `AbortController()`
2. Add as the value of `signal` as the second argument in the fetch request
3. return from useEffect an anonymous function that calls the `abort()` method on any lingering requests
4. Add catch logic for the abort event and handle

```js
useEffect(() => {
  // 1.
  const abortController = new AbortController();
  // 2.
  fetch("http://localhost:8000/blogs", { signal: abortController.signal })
    .then((res) => {
      if (!res.ok) {
        throw Error("Unable to fetch the data");
      }
      return res.json();
    })
    .then((data) => {
      setBlogs(data);
      setIsLoading(false);
    })
    .catch((error) => {
      if (error.name !== "AbortError") {
        setIsLoading(false);
        setErrorMessage(error.message);
      }
    });
  // 3.
  return () => abortController.abort();
}, []);
```

---

# useRef

1. import

```js
import { useRef } from "react";
```

2. Create a const

```js
const inputRef = useRef(null);
```

3. Attach the `ref` to an element

```js
<input type="text" ref={inputRef} />
```

4. Grab properties from the element:

- `inputRef.current.value`
- `inputRef.current.value = ''`
- `inputRef.current.focus()`

# useLayoutEffect

`useEffectLayout` works in the same way as `useEffect` however `useEffectLayout` is triggered earlier within the component lifecycle, before the page is rendered, where `useEffect` is triggered after the page is rendered or after the component has mounted the dom.

# useImperativeHandle

# useContext

# useMemo

# useCallback
