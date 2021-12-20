# Grid System

- [Grid Flex Patterns](#Grid-Flex-Patterns)
- [Break Points](#Break-Points)
- [Grid](#Grid)
- [Nested Grids](#Nested-Grids)

---

### Grid Flex Patterns

Example:

```js
<Grid
  container
  direction="row"
  justifyContent="space-evenly"
  alignItems="center"
>
```

**direction**:

- `row`
- `row-reverse`
- `column`
- `column-reverse`

**justifyContent**:

- `flex-start`
- `center`
- `flex-end`
- `space-between`
- `space-around`
- `space-evenly`

**alignItems**:

- `flex-start`
- `center`
- `flex-end`
- `stretch`
- `baseline`

### Break Points

- `xs` > 0px
- `sm` > 600px
- `md` > 900px
- `lg` > 1200px
- `xl` > 1536px

### Grid

`spacing` is the space between `items`

```js
<Grid container spacing={2}>
  <Grid item xs={6} md={8}>
    <Item>xs=6 md=8</Item>
  </Grid>
  <Grid item xs={6} md={4}>
    <Item>xs=6 md=4</Item>
  </Grid>
  <Grid item xs={6} md={4}>
    <Item>xs=6 md=4</Item>
  </Grid>
  <Grid item xs={6} md={8}>
    <Item>xs=6 md=8</Item>
  </Grid>
</Grid>
```

Typical example when creating a list of cards from a passed in array:

```js
<Grid container spacing={{ xs: 2, md: 3 }} columns={{ xs: 4, sm: 8, md: 12 }}>
  {Array.from(Array(6)).map((_, index) => (
    <Grid item xs={2} sm={4} md={4} key={index}>
      <Item>xs=2</Item>
    </Grid>
  ))}
</Grid>
```

The above container takes up a different amount of columns depending on screen size. When this is done each item also need to have a specified corresponding breakpoint (as above).

Spacing can be controlled for `rows` and `columns`:

```js
<Grid container rowSpacing={1} columnSpacing={{ xs: 1, sm: 2, md: 3 }}>
```

### Nested Grids

Grids can be both `containers` and `items`:

```js
<Grid container spacing={1}>
  <Grid container item spacing={3}>
    <FormRow />
  </Grid>
  <Grid container item spacing={3}>
    <FormRow />
  </Grid>
  <Grid container item spacing={3}>
    <FormRow />
  </Grid>
</Grid>
```
