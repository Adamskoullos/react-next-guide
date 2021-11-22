# React Guide

Reference guide for getting up and running with React

Toc:

- [Create New Project]()
- [Components and Templates]()
- [Dynamic Values in Templates]()

- [Multiple Components]()
- [Adding Styles]()

- [Click Events]()
- [useState Hook]()
- [React Dev Tools]()
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
