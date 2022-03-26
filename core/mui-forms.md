# Forms

```js
<form onSubmit={handleSubmit}></form>
```

### Text Fields

Three main variants:

```js
<TextField id="outlined-basic" label="Outlined" variant="outlined" />
<TextField id="filled-basic" label="Filled" variant="filled" />
<TextField id="standard-basic" label="Standard" variant="standard" />
```

> Some Props:

- **`value`**
- `required`
- `disabled`
- `type`
- `helperText`
- `fullWidth`
- `error`

> Event:

- **`onChange`**

---

### Validation

1. Add the `error` prop to the input setting its value to some state with default `false`
2. Use the state to control the error boolean to handle events and workflows

```js
const [title, setTitle] = useState("");
const [titleError, setTitleError] = useState(false);

const handleSubmit = () => {
  e.preventDefault();
  setTitleError(false);
  if (!title) {
    setTitleError(true);
    return;
  }
};
```

---

### Radios

Below is the component architecture of a typical radio button component.
The `RadioGroup` is given the initial `value` and also the `onChange` property updating the value as the user clicks a radio.

```js
import * as React from "react";
import Radio from "@mui/material/Radio";
import RadioGroup from "@mui/material/RadioGroup";
import FormControlLabel from "@mui/material/FormControlLabel";
import FormControl from "@mui/material/FormControl";
import FormLabel from "@mui/material/FormLabel";

export default function ControlledRadioButtonsGroup() {
  const [value, setValue] = React.useState("female");

  const handleChange = (event) => {
    setValue(event.target.value);
  };

  return (
    <FormControl component="fieldset">
      <FormLabel component="legend">Gender</FormLabel>
      <RadioGroup
        aria-label="gender"
        name="controlled-radio-buttons-group"
        value={value}
        onChange={handleChange}
      >
        <FormControlLabel value="female" control={<Radio />} label="Female" />
        <FormControlLabel value="male" control={<Radio />} label="Male" />
      </RadioGroup>
    </FormControl>
  );
}
```

---

### Checkboxes

The below example uses a hard coded object as state, but this could be passed in.
The `setState` method has two arguments: the first argument is the current state being spread in, the second argument is the item to be updated.

```js
import * as React from "react";
import FormGroup from "@mui/material/FormGroup";
import FormControlLabel from "@mui/material/FormControlLabel";
import Checkbox from "@mui/material/Checkbox";

export default function Checkbox() {
  const [state, setState] = React.useState({
    one: true,
    two: false,
    three: false,
  });

  const handleChange = (event) => {
    setState({
      ...state,
      [event.target.name]: event.target.checked,
    });
  };

  return (
    <FormControl sx={{ m: 3 }} component="fieldset" variant="standard">
      <FormLabel component="legend">Chose Items</FormLabel>
      <FormGroup>
        <FormControlLabel
          control={
            <Checkbox checked={one} onChange={handleChange} name="one" />
          }
          label="One"
        />
        <FormControlLabel
          control={
            <Checkbox checked={two} onChange={handleChange} name="two" />
          }
          label="Two"
        />
        <FormControlLabel
          control={
            <Checkbox checked={three} onChange={handleChange} name="three" />
          }
          label="Three"
        />
      </FormGroup>
    </FormControl>
  );
}
```

We can use icons to star or favourite stuff:

```js
<Checkbox {...label} icon={<FavoriteBorder />} checkedIcon={<Favorite />} />
```

---

### Switches

Basic switch:

```js
import * as React from "react";
import Switch from "@mui/material/Switch";

export default function ControlledSwitches() {
  const [checked, setChecked] = React.useState(true);

  const handleChange = (event) => {
    setChecked(event.target.checked);
  };

  return (
    <Switch
      checked={checked}
      onChange={handleChange}
      inputProps={{ "aria-label": "controlled" }}
    />
  );
}
```

Form Group Switches:

```js
export default function SwitchesGroup() {
  const [state, setState] = React.useState({
    gilad: true,
    jason: false,
    antoine: true,
  });

  const handleChange = (event) => {
    setState({
      ...state,
      [event.target.name]: event.target.checked,
    });
  };

  return (
    <FormControl component="fieldset" variant="standard">
      <FormLabel component="legend">Assign responsibility</FormLabel>
      <FormGroup>
        <FormControlLabel
          control={
            <Switch
              checked={state.gilad}
              onChange={handleChange}
              name="gilad"
            />
          }
          label="Gilad Gray"
        />
        <FormControlLabel
          control={
            <Switch
              checked={state.jason}
              onChange={handleChange}
              name="jason"
            />
          }
          label="Jason Killian"
        />
        <FormControlLabel
          control={
            <Switch
              checked={state.antoine}
              onChange={handleChange}
              name="antoine"
            />
          }
          label="Antoine Llorca"
        />
      </FormGroup>
      <FormHelperText>Be careful</FormHelperText>
    </FormControl>
  );
}
```
