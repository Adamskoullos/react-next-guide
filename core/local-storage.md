# Working with Local Storage

To manage a page reload we can update local storage when local state or store state is updated to keep the user data in local storage in sync with the app.

1. On initial load as the data is pulled from an endpoint and on every user input change we want to sync local storage:

```js
localStorage.setItem("name", JSON.stringify(item));
```

2. On a page reload we want to check if the user is already logged in and re hydrate the state from local storage if they are:

```js
const [items, setItems] = useState(JSON.parse(localStorage.getItem("item")));
```
