## RTK createSelector

We can create memoized selectors with either `createSelector` or `createEntityAdapter`. If we are not using `createEntityAdapter` to provide: memoized selectors, a normalized data structure or using the adapter methods to maintain the normalized data structure and only want to use memoized selectors we can use `createSelector`.

### Advantages

There are two main reasons to use `createSelector`:

1. To increase efficiency with the use of memoized selectors
2. To create computed memoized selectors

### Structure

`createSelector` takes two types of arguments:

1. `input` selectors > can be a single input selector or multiple
2. `output` selector > the last argument is the output selector

The example below creates two normal `input` selectors which are passed in as the first two arguments to `createSelector`.
The return value of these input selector functions are then passed in as the arguments (in the respective order) to the `output` selector. The returned value from the output selector is the value is what is returned from `selectTax` when it is called.

If the returned value from the `input` selectors has not changed the `selectTax` selector will return the memoized previous value.

```js
const selectShopItems = (state) => state.shop.items;
const selectTaxPercent = (state) => state.shop.taxPercent;

const selectTax = createSelector(
  selectSubtotal,
  selectTaxPercent,
  (subtotal, taxPercent) => subtotal * (taxPercent / 100)
);
```

Example taken directly from the `Reselect` docs:

```js
import { createSelector } from "reselect";

const selectShopItems = (state) => state.shop.items;
const selectTaxPercent = (state) => state.shop.taxPercent;

const selectSubtotal = createSelector(selectShopItems, (items) =>
  items.reduce((subtotal, item) => subtotal + item.value, 0)
);

const selectTax = createSelector(
  selectSubtotal,
  selectTaxPercent,
  (subtotal, taxPercent) => subtotal * (taxPercent / 100)
);

const selectTotal = createSelector(
  selectSubtotal,
  selectTax,
  (subtotal, tax) => ({ total: subtotal + tax })
);

const exampleState = {
  shop: {
    taxPercent: 8,
    items: [
      { name: "apple", value: 1.2 },
      { name: "orange", value: 0.95 },
    ],
  },
};

console.log(selectSubtotal(exampleState)); // 2.15
console.log(selectTax(exampleState)); // 0.172
console.log(selectTotal(exampleState)); // { total: 2.322 }
```
