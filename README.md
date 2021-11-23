# React Guide

Reference guide for getting up and running with React

Toc:

- [Click Events]()
- [useState Hook]()
- [Outputting Lists]()
- [Props]()
- [Reusing Components]()
- [Functions as Props]()
- [useEffect Hook]()
- [Conditional Loading]()
- [Handling Errors]()
- [Custom Hooks]()
- [React Router]()
- [Exact Match Routes]()
- [Router Links]()
- [Route Params]()
- [Reusing Custom Hooks]()
- [Forms (Controlled Inputs)]()
- [Submit Events]()
- [HTTP Requests]()
- [Redirects]()
- [Error Page]()

---

## Click Events

The below example shows the patterns we can use when accessing the native event:

```js
const Home = () => {
  const handleClick = (e) => {
    console.log("Clicked !!! ", e);
  };
  const handleClickAgain = (name, e) => {
    console.log(name, e.target);
  };
  return (
    <div className="home">
      <h2>Home Page</h2>
      <button onClick={handleClick}>Click Me</button>
      <button onClick={(e) => handleClickAgain("Dave", e)}>Click Me</button>
    </div>
  );
};

export default Home;
```

---

## useState Hook

`useState` allows us to make data responsive by triggering the component to re-render every time the `useState` data changes.

```js
import { useState } from "react";

const Home = () => {
  const [name, setName] = useState("Dave");

  const handleClick = (e) => {
    setName("John");
  };

  return (
    <div className="home">
      <h2>Home Page</h2>
      <h3>{name}</h3>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
};

export default Home;
```

---

## Outputting Lists

We can use the `map` method to loop through an array creating the elements for each item. react requires a `key` which takes a unique value for each item:

Basic structure:

```js
<div className="home">
  {blogs.map((blog) => (Template goes here))}
</div>
```

Example:

```js
import { useState } from "react";

const Home = () => {
  const [blogs, setBlogs] = useState([
    { title: "One", body: "This is a blog post", author: "Dave", id: 1 },
    { title: "Two", body: "This is a blog post", author: "Dave", id: 2 },
    { title: "Three", body: "This is a blog post", author: "Dave", id: 3 },
    { title: "Four", body: "This is a blog post", author: "Dave", id: 4 },
  ]);

  return (
    <div className="home">
      {blogs.map((blog) => (
        <div className="post" key={blog.id}>
          <h2>{blog.title}</h2>
        </div>
      ))}
    </div>
  );
};

export default Home;
```

---

## Props

Basic pattern used when passing props down to reusable components:

Parent:

```js
import { useState } from "react";
import BlogList from "./BlogList";

const Home = () => {
  const [blogs, setBlogs] = useState([
    { title: "One", body: "This is a blog post", author: "Dave", id: 1 },
    { title: "Two", body: "This is a blog post", author: "Dave", id: 2 },
    { title: "Three", body: "This is a blog post", author: "Dave", id: 3 },
    { title: "Four", body: "This is a blog post", author: "Dave", id: 4 },
  ]);

  return (
    <div className="home">
      <BlogList blogs={blogs} />
    </div>
  );
};

export default Home;
```

Child:

The child component can either accept the props object if there are many properties to assign or destruct the properties required:

Example 1:

```js
const BlogList = ({ blogs }) => {
  return (
    <div className="blog-list">
      {blogs.map((blog) => (
        <div className="blog-preview" key={blog.id}>
          <h2>{blog.title}</h2>
        </div>
      ))}
    </div>
  );
};

export default BlogList;
```

Example 2 when each property is assigned:

```js
const BlogList = (props) => {
  const blogs = props.blogs;

  return (
    <div className="blog-list">
      {blogs.map((blog) => (
        <div className="blog-preview" key={blog.id}>
          <h2>{blog.title}</h2>
        </div>
      ))}
    </div>
  );
};

export default BlogList;
```
