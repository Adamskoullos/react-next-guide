# Material UI

Quick reference for the nuts and bolts of MUI

- [Set Up](#Set-Up)
- [Typography Component](#Typography-Component)
- [Button](#Button)
- [Forms](https://github.com/Adamskoullos/react-guide/blob/main/mui-forms.md)
- [Styles](https://github.com/Adamskoullos/react-guide/blob/main/mui-styles.md)
- [Layout & Grid](https://github.com/Adamskoullos/react-guide/blob/main/grid.md)
- [Data Tables & Pagination](https://github.com/Adamskoullos/react-guide/blob/main/tables.md)
- [Card]()
- [Layout Component]()
- [Navigation Drawer]()
- [Lists]()
- [App Bars]()

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

```html
<link
  rel="stylesheet"
  href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap"
/>
```

Or

```js
npm install @fontsource/roboto
```

```js
import "@fontsource/roboto/300.css";
import "@fontsource/roboto/400.css";
import "@fontsource/roboto/500.css";
import "@fontsource/roboto/700.css";
```

1. Clear out `App.css` > To be used for classes that style specific standard html elements
2. Clear out `index.css` and add basic reset:

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Roboto";
}
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

---

### FAB

```js
import * as React from "react";
import Box from "@mui/material/Box";
import Fab from "@mui/material/Fab";
import AddIcon from "@mui/icons-material/Add";
import EditIcon from "@mui/icons-material/Edit";
import FavoriteIcon from "@mui/icons-material/Favorite";
import NavigationIcon from "@mui/icons-material/Navigation";

export default function FloatingActionButtons() {
  return (
    <Box sx={{ "& > :not(style)": { m: 1 } }}>
      <Fab color="primary" aria-label="add">
        <AddIcon />
      </Fab>
      <Fab color="secondary" aria-label="edit">
        <EditIcon />
      </Fab>
      <Fab variant="extended">
        <NavigationIcon sx={{ mr: 1 }} />
        Navigate
      </Fab>
      <Fab disabled aria-label="like">
        <FavoriteIcon />
      </Fab>
    </Box>
  );
}
```

### FAB - Speed Dial

```js
import * as React from "react";
import Box from "@mui/material/Box";
import SpeedDial from "@mui/material/SpeedDial";
import SpeedDialIcon from "@mui/material/SpeedDialIcon";
import SpeedDialAction from "@mui/material/SpeedDialAction";
import FileCopyIcon from "@mui/icons-material/FileCopyOutlined";
import SaveIcon from "@mui/icons-material/Save";
import PrintIcon from "@mui/icons-material/Print";
import ShareIcon from "@mui/icons-material/Share";

const actions = [
  { icon: <FileCopyIcon />, name: "Copy" },
  { icon: <SaveIcon />, name: "Save" },
  { icon: <PrintIcon />, name: "Print" },
  { icon: <ShareIcon />, name: "Share" },
];

export default function ControlledOpenSpeedDial() {
  const [open, setOpen] = React.useState(false);
  const handleOpen = () => setOpen(true);
  const handleClose = () => setOpen(false);

  return (
    <Box sx={{ height: 320, transform: "translateZ(0px)", flexGrow: 1 }}>
      <SpeedDial
        ariaLabel="SpeedDial uncontrolled open example"
        sx={{ position: "absolute", bottom: 16, right: 16 }}
        icon={<SpeedDialIcon />}
        onClose={handleClose}
        onOpen={handleOpen}
        open={open}
      >
        {actions.map((action) => (
          <SpeedDialAction
            key={action.name}
            icon={action.icon}
            tooltipTitle={action.name}
            onClick={handleClose}
          />
        ))}
      </SpeedDial>
    </Box>
  );
}
```
