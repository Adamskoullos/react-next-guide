# Styles

- [makeStyles]()
- [Custom Theme]()
- [Custom Fonts]()

## makeStyles

Using the below pattern we can create one or multiple objects of classes that can be pulled into and used in any component.

1. **Create a styles component**:

- Import `makeStyles`:

```js
import { makeStyles } from "@mui/styles";
```

- Create `useStyles` classes object:

```js
const useStyles = makeStyles({
  btn: {
    fontSize: 50,
    background: "green",
    "&:hover": {
      background: "yellow",
    },
  },
});
```

2. **Use `useStyles` within a component**:

- Import `useStyles`:

```js
import useStyles from "./styles/useStyles";
```

- Assign styles to a const:

```js
const classes = useStyles();
```

- Use classes:

```js
<button className={classes.btn}>Submit</button>
```

---

## Custom Theme

- [Default Theme Config](https://mui.com/customization/default-theme/#explore);
- [Colors](https://mui.com/customization/color/#picking-colors);

We can customise any part of the default theme and then make the updated theme available throughout the application by using `ThemeProvider`.
The `colors` link above allows us to use the MUI color objects instead of singular colors which provides all button effects out of the box:

**color object structure**:

- secondary:
  - light
  - main
  - dark
  - contrastText

```js
import { ThemeProvider, createTheme } from "@mui/material/styles";
```

The below example shows how defaults (primary and secondary) can customised. Primary is assigned a singular color and secondary is assigned an MUI color object:

```js
const theme = createTheme({
    palette: {
        primary{
            main: '#fefefe',
        },
        secondary: red // Color object
    }
});
```

The above could be a separate component or directly in the App component where we will wrap the whole App with `ThemeProvider` giving all components access to the customised theme:

```js
<ThemeProvider theme={theme}>
  <Router>
    <div className="App">
      <Navbar />
      <div className="content">
        <Routes>
          <Route path="/" element={<Home someProp={true} />} />
          <Route path="/new" element={<NewBlog someProp={true} />} />
        </Routes>
      </div>
    </div>
  </Router>
</ThemeProvider>
```

---

## Custom Font

1. Create the import within Google Fonts including any fonts and weights required
2. Add the import to the `index.css` within the root of the project
3. Add the font to the `custom Theme` so it can be used within the application

```js
const theme = createTheme({
    palette: {
        primary{
            main: '#fefefe',
        },
        secondary: red // Color object
    },
    typography: {
        fontFamily: 'FontName',
        fontWeightLight: 400,
        fontWeightRegular: 500,
        fontWeightMedium: 600,
        fontWeightBold: 700,
    }
});
```
