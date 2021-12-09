# Context API

Using the `context` API we can create mini stores that contain global sate accessible to child components.

1. Create a `Context` folder to house context files for each group of states that need to be shared and interacted with by a group of components.

- Context/
  - LoginContext.js
  - RoleContext.js

2. Create and export the `LoginContext.js` file:

```js
import { createContext } from "react";

export const LoginContext = createContext({});
```

3. Import the context within the `parent` component (normally the page component) of all the children that need access to the context and wrap the template with the context provider. Create all state within the parent component to be used across children.

Then finally add the `value` attribute to the provider and pass in an object with any properties and functions to be shared among children components.

```js
import { useState } from 'react';
import { LoginContext } from './Context/LoginContext';

const App () => {

const [showProfile, setShowProfile] = useState(false);
const [userName, setUserName] = useState('');

return (
  <div className="App">
    <LoginContext.Provider value="{{userName, setUsername, setShowProfile}}">
      {showProfile ? <Profile /> : <Login />}
    </LoginContext.Provider>
  </div>
)}

export default App;
```

4. Import and use the context within child components

- Import `useContext`
- Import `LoginContext`
- Destruct properties and functions from `useContext`

```js

import { useState, useContext } from 'react';
import { LoginContext } from'../Context/LoginContext';

const Login () => {

  const {setShowProfile, setUsername} = useContext(LoginContext);

  return (
    <div>
      <input
        type="text"
        placeholder="Username"
        onChange={(event) => setUserName(event.target.value)}
      />
      <input
        type="text"
        placeholder="Password"
        onChange={(event) => setShowProfile(true)}
      />
    </div>
  )

}

export default Login;


```
