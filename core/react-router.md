# React Router v6

```js
import {
  BrowserRouter as Router,
  Routes,
  Route,
  Navigate,
  Link,
  Outlet,
  useParams,
  NavLink,
} from "react-router-dom";
```

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

The `Route` element can be self closing or have an opening and closing tag, which works well when there are parent and child routes.

**Note**: Absolute paths use a prefixed `/` and relative paths do not.

```js
<Routes>
  <Route path="/" element={<Home someProp={true} />} />
  <Route path="/courses" element={<Courses />}>
    <Route path="javascript" element={<JavaScript />}>
      <Route path="oop" element={<Oop />} />
      <Route path="functional" element={<Functional />} />
    </Route>
    <Route path="solidity" element={<Solidity />} />
  </Route>
</Routes>
```

### Route Params and Dynamic Routes

Dynamic paths within a route are prefixed with `:`. In the example below we are calling the params `:id`:

```js
<Routes>
  <Route path="/" element={<Home someProp={true} />} />
  <Route path="/blogs/:id" element={<BlogDetails />} />
</Routes>
```

Then we can wrap a list item or card with the `Link` element (importing it from `react-router-dom`). Use template literals to add the dynamic route params:

```js
<Link to={`/blogs/${blog.id}`}>
  <h2>{blog.title}</h2>
  <p>Written by {blog.author}</p>
</Link>
```

Then within the dynamic component, import `useParams` and destruct the the params used. From there use the params to grab the item:

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

### Outlet

If we want to use sub-pages or tabs within a parent page we can use `Outlet` a kind of slot. To do this we need to nest the child routes `:id` within the parent route:

```js
<Routes>
  <Route path="/" element={<Home someProp={true} />}>
    <Route path="/blogs/:id" element={<BlogDetails />} />
  </Route>
</Routes>
```

Within the parent component we can add `Outlet` where we want the child component to be displayed:

```js
import { Link, Outlet } from "react-router-dom";

const BlogList = (props) => {
  const blogs = props.blogs;
  const title = props.title;

  return (
    <div className="blog-list">
      <h1>{title}</h1>
      {blogs.map((blog) => (
        <div className="blog-preview" key={blog.id}>
          <Link to={`/blogs/${blog.id}`}>
            <h2>{blog.title}</h2>
            <p>Written by {blog.author}</p>
          </Link>
        </div>
      ))}
      <Outlet />
    </div>
  );
};

export default BlogList;
```

In the example above rather than have the blog post open in its own page it is displayed within the parent page via the `Outlet` element.

---

## NavLink

The `NavLink` has access to the `isActive` property which can be used to directly adjust styling;

```js
<NavLink
  style={({ isActive }) => {
    return {
      color: isActive ? "green" : "grey",
    };
  }}
>
  Page
</NavLink>
```

## useNavigate

We can use `Navigate` to use click events on elements and turn them into navigation links. The `Navigation` element has two arguments:

1. The path to navigate to
2. Data to be passed on to the component

```js
import { Link, Outlet, useNavigate } from "react-router-dom";

const BlogList = (props) => {
  const blogs = props.blogs;
  const title = props.title;

  const navigate = useNavigate();

  return (
    <div className="blog-list">
      <h1>{title}</h1>
      <button
        onClick={() => {
          navigate("/page", { propOne: 1 });
        }}
      >
        Page
      </button>
    </div>
  );
};

export default BlogList;
```
