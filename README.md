# React Guide

Reference guide for getting up and running with React

Toc:

- [Click Events](#Click-Events)
- [useState Hook](#useState-Hook)
- [Outputting Lists](#Outputting-Lists)
- [Props](#Props)
- [Reusing Components](#Reusing-Components)
- [Functions as Props](#Functions-as-Props)
- [useEffect Hook](#useEffect-Hook)
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

---

## Reusing Components

The below example is passing in a different `title` string in each component and the second component is passing in a filtered array:

```js
import { useState } from "react";
import BlogList from "./BlogList";

const Home = () => {
  const [blogs, setBlogs] = useState([
    { title: "One", body: "This Paul's blog post", author: "Paul", id: 1 },
    { title: "Two", body: "This Dave's blog post", author: "Dave", id: 2 },
    { title: "Three", body: "This Joan's blog post", author: "Joan", id: 3 },
    { title: "Four", body: "This Dave's blog post", author: "Dave", id: 4 },
  ]);

  return (
    <div className="home">
      <BlogList blogs={blogs} title="All Blogs" />
      <BlogList
        blogs={blogs.filter((post) => post.author === "Dave")}
        title="Filtered Blogs"
      />
    </div>
  );
};

export default Home;
```

---

## Functions as Props

The parent component below defines the `handleDelete()` function which uses the `setBlogs()` method from `useState` which mutates the blogs array. The `handleDelete()` function is passed down to the BlogList component:

```js
import { useState } from "react";
import BlogList from "./BlogList";

const Home = () => {
  const [blogs, setBlogs] = useState([
    { title: "One", body: "This Paul's blog post", author: "Paul", id: 1 },
    { title: "Two", body: "This Dave's blog post", author: "Dave", id: 2 },
    { title: "Three", body: "This Joan's blog post", author: "Joan", id: 3 },
    { title: "Four", body: "This Dave's blog post", author: "Dave", id: 4 },
  ]);

  const handleDelete = (id) => {
    const newBlogs = blogs.filter((blog) => blog.id !== id);
    setBlogs(newBlogs);
  };

  return (
    <div className="home">
      <BlogList blogs={blogs} title="All Blogs" handleDelete={handleDelete} />
      <BlogList
        blogs={blogs.filter((post) => post.author === "Dave")}
        title="Filtered Blogs"
        handleDelete={handleDelete}
      />
    </div>
  );
};

export default Home;
```

The below child component accepts the passed down function via the props object and uses it within an anonymous function via a click event, passing in the items id. Because the function was defined within the parent, parent properties/methods within its definition are accessible by the function, in this case `setBlogs` which allows us to directly mutate the blogs array within the parent:

```js
const BlogList = (props) => {
  const blogs = props.blogs;
  const title = props.title;
  const handleDelete = props.handleDelete;

  return (
    <div className="blog-list">
      <h1>{title}</h1>
      {blogs.map((blog) => (
        <div className="blog-preview" key={blog.id}>
          <h2>{blog.title}</h2>
          <p>Written by {blog.body}</p>
          <button onClick={() => handleDelete(blog.id)}>delete post</button>
        </div>
      ))}
    </div>
  );
};

export default BlogList;
```
