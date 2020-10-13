# Components and State

React JS is a user interface component library. React is not a framework, it just a library that renders UI. Your entire application will live within one targeted DOM element. React will handle the rest for you.

Working with the DOM API is hard. The React team recognizes this, so they built a simple engine called the virtual DOM that interacts with the actual DOM for us. We tell the virtual DOM which elements and state (data) to render to the actual DOM, and it will do so. Beyond that, it will “react” when the state (data) in our app changes, and will update the DOM accordingly.

In a process called “reconciliation”, React will detect that the state of the app has changed. Then it will update the virtual DOM, taking note of which nodes have changed due to the state changes. Finally, once it knows which nodes have changed, it will update only those specific nodes on the actual DOM. This takes a lot of pressure off of our browsers and it’s why React is as powerful as it is.

## React Components

A “component” is a pretty loose term to describe a discrete chunk of your site. A header could be a component, for example. Or a footer. Or a hero section, etc.

Function components look like this: 

```jsx
const Example = (props) => {
  // You can use Hooks here!
  return <div />;
}
```

or this:

```jsx
function Example(props) {
  // You can use Hooks here!
  return <div />;
}
```

A basic component could be: 

```jsx
import React from 'react';

const Intro = () => {
  const greeting = "Hi Lambda!";
  return (
    <div>
      <h1>{greeting}</h1>
    </div>
  );
};
```

A few notes on react components:

- It’s important to understand early on that a React component is just a regular JavaScript function.
- When we return what looks like HTML in a React component, what we’re secretly returning is a JavaScript object that describes the kind of HTML we want to make. React is going to figure out how to make it for us later.
- The curly brackets will evaluate any valid JavaScript expression.

Components and props should be named from the component’s own point of view rather than the context in which it is being used.

All React components must act like **pure functions** with respect to their props. A pure functions does not attempt to change their inputs, and always return the same result for the same inputs.

### State

`const [lightOn, setLightOn ] = useState();` is equivalent to:

```jsx
let lightOn;
let setLightOn = (value) => {lightOn = value;};
```

The `useState` hook `const [lightOn, setLightOn ] = useState(false);` works like this:

`lightOn` is a variable the value of which is whatever we passed in to `useState`. In this case, it’s value is the boolean primitive `false`. `setLightOn` is a function that will change the value of `lightOn`. We’ll also note that I could have named `lightOn` and `setLightOn` whatever I wanted. I could’ve named them `peanutButter` and `jelly` if I wanted but that would’ve made it pretty confusing for someone reading my code to understand what they do.

### Conditional Rendering

Conditional rendering is just a fancy name for a very common pattern in React. We don’t want to see both lightbulbs at once. We only want to render one or the other based on some condition.

A ternary operator acts as a single line `if/else` statment. This line:

`{lightOn === false ? <img src={white} /> : <img src={yellow} />}`

It’s saying *“Is the `lightOn` state variable set to `false`? If so render `<img src={white} />`, otherwise render `<img src={yellow} />`*

### Event Listeners

To attach a click listener to a react component, we need to use this camel-casing: onClick. This event listener will take in a callback function.

Be careful with event listener syntax:

```jsx
// Everything is on fire
<div onClick={ setLightOn(false) } className="App">

// Everything is fine
<div onClick={ ()=> setLightOn(false) } className="App">
```

### A basic component that displays a lightbulb

```jsx
import React, { useState } from "react";
import { render } from "react-dom";
import "./styles.css";

const white = "https://image.flaticon.com/icons/png/512/32/32177.png";
const yellow =
  "https://i.pinimg.com/originals/92/94/ba/9294badee7b8f3d93fa9bc6c874641b2.png";

function App() {
  const [lightOn, setLightOn] = useState(true);

  return (
    <div onClick={() => setLightOn(!lightOn)} className="App">
      {lightOn === false ? <img src={white} /> : <img src={yellow} />}
    </div>
  );
}

const rootElement = document.getElementById("root");
render(<App />, rootElement);
```

![Components%20and%20State%2064937123125342f7bb6356d451336f39/Untitled.png](Components%20and%20State%2064937123125342f7bb6356d451336f39/Untitled.png)

[Tutorial: Intro to React - React](https://reactjs.org/tutorial/tutorial.html)

[Thinking in React - React](https://reactjs.org/docs/thinking-in-react.html)

[What Problems Does React.JS Solve? When Must You Select React.JS?](https://scotch.io/@anitashah/what-problems-does-reactjs-solve-when-must-you-select-reactjs)

## Asynchronous nature of useState

While React's `setState` *is* asynchronous (both classes and hooks), and it's tempting to use that fact to explain the observed behaviour, it is not the ***reason why*** it happens.

TLDR: The reason is a [closure](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) scope around an immutable `const` value.

---

### *Solutions:*

- read the value in render function (not inside nested functions):

    ```
    useEffect(() => { setMovies(result) }, [])
    console.log(movies)
    ```

- add the variable into dependencies (and use the [react-hooks/exhaustive-deps](https://reactjs.org/docs/hooks-rules.html#eslint-plugin) eslint rule):

    ```
    useEffect(() => { setMovies(result) }, [])
    useEffect(() => { console.log(movies) }, [movies])
    ```

- use a mutable reference (when the above is not possible):

    ```
    const moviesRef = useRef(initialValue)
    useEffect(() => {
      moviesRef.current = result
      console.log(moviesRef.current)
    }, [])
    ```

[useState set method not reflecting change immediately](https://stackoverflow.com/questions/54069253/usestate-set-method-not-reflecting-change-immediately)