# React Router v6

- []()

## React Router (v6)

```
npm install react-router-dom
```

1. Import router into `App.js`: `import { BrowserRouter as Router, Route, Routes } from 'react-router-dom'`

2. Wrap the template within App.js with the `Router`

3. Then wrap all routes with `Routes`

4. Add each `Route`

**Note**:

- The below example shows how each route contains the path and also the element/component and this allows us to also pass props into the page component
- All routes match exactly by default
- Paths and links are relative to their parent route, we can omit the `'/'` if we want the relative path

```js
import { BrowserRouter as Router, Route, Routes } from "react-router-dom";
import Navbar from "./Navbar";
import Home from "./Home";

function App() {
  return (
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
  );
}

export default App;
```

Navbar link:

```js
 <Link to="/" >
 <Link to="/new" >

```

### handling Redirects

To handle old routes that have been removed we can redirect the user by using `Navigate`, this is imported from `react-router-dom` and used in the below example:

```js
import {
  BrowserRouter as Router,
  Route,
  Routes,
  Navigate,
} from "react-router-dom";
import Navbar from "./Navbar";
import Home from "./Home";
import NewBlog from "./NewBlog";

function App() {
  return (
    <Router>
      <div className="App">
        <Navbar />
        <div className="content">
          <Routes>
            <Route path="/" element={<Home someProp={true} />} />
            <Route path="/new" element={<NewBlog someProp={true} />} />
            <Route path="/fake" element={<Navigate replace to="/" />} />
          </Routes>
        </div>
      </div>
    </Router>
  );
}

export default App;
```

### Nested Routes

### Route Params

Wrap a list item or card with the `Link` element (importing it from `react-router-dom`). use template literals to add the dynamic route params:

```js
<Link to={`/blogs/${blog.id}`}>
  <h2>{blog.title}</h2>
  <p>Written by {blog.author}</p>
</Link>
```

Then within the component, import `useparams` and destruct the the params used from the params object. from there use the params to grab the item:

```js
import { useParams } from "react-router";

const BlogDetails = () => {
  const { id } = useParams();

  // code to grab item with id from store or request

  return (
    <div>
      <h1>Blog Details Page - {id}</h1>
    </div>
  );
};

export default BlogDetails;
```

---
