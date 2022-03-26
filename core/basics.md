# Basics

Toc:

- [Click Events](#Click-Events)
- [Conditional Styling](#Conditional-Styling)
- [useState Hook](#useState-Hook)
- [Outputting Lists](#Outputting-Lists)
- [Props](#Props)
- [Reusing Components](#Reusing-Components)
- [Functions as Props](#Functions-as-Props)
- [useEffect Hook](#useEffect-Hook)
- [Conditional Loading and Error handling](#Conditional-Loading-and-Error-handling)
- [Custom Hooks](#Custom-Hooks)
- [Reusing Custom Hooks](#Reusing-Custom-Hooks)
- [Forms (Controlled Inputs)](#Forms) & Requests
- [Search](#Search)
- [Redirects](#Redirects)
- [Error Page](#Error-Page)
- [JSON Server](#JSON-Server)

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

## Conditional Styling

```js
style={(item.checked) ? {textDecoration: 'line-through'} : null}
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

---

## useEffect Hook

The `useEffect` hook is a function that fires every time the dom updates. It fires once on initial page/component load and then each time the dom is updated.

```js
import { useState, useEffect } from "react";
```

```js
useEffect(() => {
  // Code to run
});
```

### Dependencies of useEffect

We can control at a more granular level when the `useEffect` hooks fires by adding an array as the second argument:

1. If we only want the hook to fire after the initial page/component load and not on subsequent updates, we add an empty array as the second argument:

```js
useEffect(() => {
  // Code to run
}, []);
```

2. If we only want `useEffect` to fire when specific properties change we can add those properties to the array:

```js
useEffect(() => {
  // Code to run
}, [dataOne, dataTwo]);
```

### Fetching data with useEffect

1. set `useEffect` to null or an empty array:

```js
const [blogs, setBlogs] = useState(null);
```

2. we cannot use `async`/`await` directly within `useEffect`, the simple example below uses `fetch` and omits error handling:

```js
useEffect(() => {
  fetch("http://localhost:8000/blogs")
    .then((res) => {
      return res.json();
    })
    .then((data) => {
      setBlogs(data);
    });
}, []);
```

3. Then to prevent errors from data being initially null we wrap any code with conditional logic to prevent rendering until the property has a value:

```js
return (
  <div className="home">
    {blogs && (
      <BlogList blogs={blogs} title="All Blogs" handleDelete={handleDelete} />
    )}
  </div>
);
```

### useEffect Cleanup

Should the user click away from a page whilst a request is still in process we can programmatically `abort` the request to prevent errors:

1. Create an `AbortController()`
2. Add as the value of `signal` as the second argument in the fetch request
3. return from useEffect an anonymous function that calls the `abort()` method on any lingering requests
4. Add catch logic for the abort event and handle

```js
useEffect(() => {
  // 1.
  const abortController = new AbortController();
  // 2.
  fetch("http://localhost:8000/blogs", { signal: abortController.signal })
    .then((res) => {
      if (!res.ok) {
        throw Error("Unable to fetch the data");
      }
      return res.json();
    })
    .then((data) => {
      setBlogs(data);
      setIsLoading(false);
    })
    .catch((error) => {
      if (error.name !== "AbortError") {
        setIsLoading(false);
        setErrorMessage(error.message);
      }
    });
  // 3.
  return () => abortController.abort();
}, []);
```

---

## Conditional Loading and Error handling

```js
const Home = () => {
  const [blogs, setBlogs] = useState(null);
  const [isLoading, setIsLoading] = useState(true);
  const [errorMessage, setErrorMessage] = useState(errorMessage);

  useEffect(() => {
    fetch("http://localhost:8000/blogs")
      .then((res) => {
        if (!res.ok) {
          throw Error("Unable to fetch the data");
        }
        return res.json();
      })
      .then((data) => {
        setBlogs(data);
        setIsLoading(false);
      })
      .catch((error) => {
        setIsLoading(false);
        setErrorMessage(error.message);
      });
  }, []);

  return (
    <div className="home">
      {errorMessage && <div>{errorMessage}</div>}
      {isLoading && <div>Loading...</div>}
      {blogs && <BlogList blogs={blogs} title="All Blogs" />}
    </div>
  );
};

export default Home;
```

---

## Custom Hooks

We can create our own custom hooks, it is required that they start with `use`. The example below extracts the fetch request into a reusable hook that can be used to make fetch requests of different data: `useFetch.js`

```js
import { useEffect, useState } from "react";

const useFetch = (endpoint) => {
  const [data, setData] = useState(null);
  const [isLoading, setIsLoading] = useState(true);
  const [errorMessage, setErrorMessage] = useState("");

  useEffect(() => {
    fetch(endpoint)
      .then((res) => {
        if (!res.ok) {
          throw Error("Unable to fetch the data");
        }
        return res.json();
      })
      .then((data) => {
        setData(data);
        setIsLoading(false);
      })
      .catch((error) => {
        setIsLoading(false);
        setErrorMessage(error.message);
      });
  }, [endpoint]);

  return { data, isLoading, errorMessage };
};

export default useFetch;
```

Then within the component it is used we import `useFetch` pass in the endpoint and destruct the properties that are returned:

```js
import BlogList from "./BlogList";
import useFetch from "./useFetch";

const Home = () => {
  const { data, isLoading, errorMessage } = useFetch(
    "http://localhost:8000/blogs"
  );

  return (
    <div className="home">
      {errorMessage && <div>{errorMessage}</div>}
      {isLoading && <div>Loading...</div>}
      {data && <BlogList blogs={data} title="All Blogs" />}
    </div>
  );
};

export default Home;
```

## Reusing Custom Hooks

The below example uses `useFetch` within the `BlogDetails` component rather than the data being passed in. In this case the `id` params is used when grabing the blog details within the component:

```js
import { useParams } from "react-router";
import useFetch from "./useFetch";

const BlogDetails = () => {
  const { id } = useParams();
  const {
    data: blog,
    isLoading,
    errorMessage,
  } = useFetch(`http://localhost:8000/blogs/${id}`);

  return (
    <div>
      {isLoading && <div>Loading...</div>}
      {errorMessage && <div>{errorMessage}</div>}
      {blog && (
        <article>
          <h2>{blog.title}</h2>
          <p>Written by {blog.author}</p>
          <div>{blog.body}</div>
        </article>
      )}
    </div>
  );
};

export default BlogDetails;
```

---

## Forms

### Controlled Inputs

1. create local state for each input, or assign incoming data to local state
2. Add the `value` and `onChange` attribute to each input

```js
import { useState } from "react";

const NewBlog = () => {
  const [title, setTile] = useState("");
  const [body, setBody] = useState("");
  const [author, setAuthor] = useState("");

  return (
    <div className="create">
      <h2>Add a new blog</h2>
      <form>
        <label>Tile:</label>
        <input
          type="text"
          value={title}
          onChange={(e) => setTile(e.target.value)}
          required
        />
        <label>Body:</label>
        <textarea
          required
          value={body}
          onChange={(e) => setBody(e.target.value)}
        ></textarea>
        <label>Author:</label>
        <select value={author} onChange={(e) => setAuthor(e.target.value)}>
          <option value=""></option>
          <option value="dave">Dave</option>
          <option value="joan">Joan</option>
        </select>
        <button>Add Blog</button>
      </form>
    </div>
  );
};

export default NewBlog;
```

### Posting form data and redirecting user

```js
const NewBlog = () => {
  const navigate = useNavigate();

  const [title, setTile] = useState("");
  const [body, setBody] = useState("");
  const [author, setAuthor] = useState("");
  const [isLoading, setIsLoading] = useState(false);

  const handleSubmit = (e) => {
    e.preventDefault();
    setIsLoading(true);

    if (!title || !body || !author) {
      return;
    }

    const newBlog = {
      title,
      body,
      author,
    };

    fetch("http://localhost:8000/blogs", {
      method: "POST",
      headers: { "Content-type": "application/json" },
      body: JSON.stringify(newBlog),
    }).then((res) => {
      setIsLoading(false);
      navigate("/");
    });
  };

  return (
    <div className="create">
      <h2>Add a new blog</h2>
      <form onSubmit={handleSubmit}>
        <label>Tile:</label>
        <input
          type="text"
          value={title}
          onChange={(e) => setTile(e.target.value)}
          required
        />
        <label>Body:</label>
        <textarea
          required
          value={body}
          onChange={(e) => setBody(e.target.value)}
        ></textarea>
        <label>Author:</label>
        <select value={author} onChange={(e) => setAuthor(e.target.value)}>
          <option value=""></option>
          <option value="dave">Dave</option>
          <option value="joan">Joan</option>
        </select>
        {isLoading && <button disabled>Submitting...</button>}
        {!isLoading && <button>Add Blog</button>}
      </form>
    </div>
  );
};

export default NewBlog;
```

---

### Deleting Blogs

```js
import { useNavigate, useParams } from "react-router";
import useFetch from "./useFetch";

const BlogDetails = () => {
  const navigate = useNavigate();
  const { id } = useParams();
  const {
    data: blog,
    isLoading,
    errorMessage,
  } = useFetch(`http://localhost:8000/blogs/${id}`);

  const handleDelete = (e) => {
    fetch(`http://localhost:8000/blogs/${id}`, {
      method: "DELETE",
    }).then((res) => {
      navigate("/");
    });
  };

  return (
    <div>
      {isLoading && <div>Loading...</div>}
      {errorMessage && <div>{errorMessage}</div>}
      {blog && (
        <article className="blog-details">
          <h2>{blog.title}</h2>
          <p>Written by {blog.author}</p>
          <p>{blog.body}</p>
          <button onClick={handleDelete}>Delete</button>
        </article>
      )}
    </div>
  );
};

export default BlogDetails;
```

---

## Search

Using a form input to filter a list of items. The below example creates the `search` state within the parent component which has both the `SearchItems` and `ItemsList` child components. All `state` properties and `setState` functions are defined in the parent component and passed down to the children. This means that events do not need to be emitted up as the functions passed down have access to parent reactive state.

Example below is only showing part of the component code and omits where the `data` has come from.

Parent:

```js
const APP = () => {
  const [search, setSearch] = useState('');
  const [items, setItems] = useState(data);

  return (
    <SearchItems search={search} setSearch={setSearch} />
    <ItemsList
      items={items.filter(item => (item.item).toLowerCase().includes(search.toLowerCase()))}
      handleCheck={handleCheck}
      handleDelete={handleDelete}
    />
  );
};

export default APP;
```

Child:

Every time the user updates the inner text within the input the value of `search` updates.

```js
const SearchItems = () => {
  return (
    <form onSubmit={(e) => e.preventDefault()}>
      <label htmlFor='search'>Search</label>
      <input
        id='search'
        type='text'
        role='searchbox'
        placeholder='Search Items'
        value={search}
        onChange={(e) => setSearch(e.target.value)}
      />
    <form/>
  )
}

```

---

## Error Page

1. Create Error page:

```js
import { Link } from "react-router-dom";

const ErrorPage = () => {
  return (
    <div className="not-found">
      <h2>That page cannot be found</h2>
      <Link to="/">Home</Link>
    </div>
  );
};

export default ErrorPage;
```

2. Add catch all route within `Router`, **As the last `Route`**:

```js
<Route path="*" element={<ErrorPage />} />
```

## JSON Server

```
npx json-server --watch data/db.json --port 8000
```
