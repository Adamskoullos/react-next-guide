# Material UI

Quick reference for the nuts and bolts of MUI

- [Set Up](#-Set-Up)
- [Typography Component](#-Typography-Component)
- [Button](#-Button)
- [Styles]()
- [System (Utilities)]()
- [Grid]()

---

## Set Up

```js
npm install @mui/material @emotion/react @emotion/styled
```

**SVG Icons**:

```js
npm install @mui/icons-material
```

**Fonts**: Add any Google fonts directly in the head or install via `Fontsource` with npm

```js
npm install @fontsource/roboto
```

```js
import "@fontsource/roboto/300.css";
import "@fontsource/roboto/400.css";
import "@fontsource/roboto/500.css";
import "@fontsource/roboto/700.css";
```

---

## Typography Component

```js
import { Typography } from "@mui/material";
```

The default tag is a `p` tag, we can style the tag by adding a variant and override what is rendered in the dom by adding a `component` prop. Without the `component` prop the `variant` value is what is rendered to the dom.

```js
<Typography variant="h1" component="div" gutterBottom>
  h1 Heading
</Typography>
```

> Props:

**Variants**:

- `h1`
- `h2`
- `h3`
- `h4`
- `h5`
- `h6`
- `subtitle1`
- `subtitle2`
- `body1` > P
- `body2` > P
- `button`
- `caption`
- `overline`

**align**: > `align="center"`

- `center`
- `justify`
- `left`
- `right`

**gutterBottom**: > Boolean

**noWrap**: > Boolean

**Paragraph**: > Boolean, if true element will be a paragraph element

**sx**: > Gives access to theme styling

---

## Button

```js
<Button variant="text">Text</Button>
<Button variant="contained">Contained</Button>
<Button variant="outlined">Outlined</Button>
```

```js
<ButtonGroup color="primary" variant="contained">
  <Button>One</Button>
  <Button>Two</Button>
  <Button>Three</Button>
</ButtonGroup>
```

> Props:

- `color`
- `size` > small, medium, large
- `fullWidth` > Boolean
- `endIcon`
- `startIcon`

### Upload Button

```js
<Input accept="image/*" id="contained-button-file" multiple type="file" />
<Button variant="contained" component="span">
Upload
</Button>
```

### Buttons with Icons

```js
import * as React from "react";
import Button from "@mui/material/Button";
import DeleteIcon from "@mui/icons-material/Delete";
import SendIcon from "@mui/icons-material/Send";
```

```js
<Button variant="outlined" startIcon={<DeleteIcon />}>
  Delete
</Button>
<Button variant="contained" endIcon={<SendIcon />}>
  Send
</Button>
```

### Icon buttons

```js
import IconButton from "@mui/material/IconButton";
import DeleteIcon from "@mui/icons-material/Delete";
```

```js
<IconButton aria-label="delete" disabled color="primary">
  <DeleteIcon />
</IconButton>
```

### Loading Buttons

```js
import LoadingButton from "@mui/lab/LoadingButton";
```

```js
<LoadingButton loading variant="outlined">
  Submit
</LoadingButton>
```
