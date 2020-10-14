+++
title = "Component Lifecycle"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
Each time we extend the React Base Class we gain access to what is called the React Component lifecycle. There are a handful of methods provided to us from the React API that allow us to tap into the lifecycle of a component. These methods provide control over when things happen during the component lifecycle.

There are three phases: Mounting (Birth), updating (Growth), and un-mounting (Death).

![Component%20Lifecycle%206aa368a26e5c4ddb87841933b3952e61/Untitled.png](Component%20Lifecycle%206aa368a26e5c4ddb87841933b3952e61/Untitled.png)

**Mounting**

- Constructor function is called and state data initiliazed
- Recieve props and place them on our component as state
- Render is then invokved and our JSX elements are transformed into DOM elements
- After render is called, componentDidMount will be invoked

**Updating**

- Any new props recieved from a parent, will trigger updates to the child.
- this.setState (not directly part of lifecycle but any changes to our state need to go through setState
- setState calles a render by default.
- Invokes componentDidUpdate

**Un-Mounting**

- Destroys event listeners

## view = function(state)

View is a function of state

- **View** is what the users sees and interacts with on our web pages
- **Function/Class** is a component or class that contains or encapsulates our data and view
- **State** is that data that our view will be represented by our view

## Constructor

- Nothing different from what you've learned in ES6 classes
- Used to load in intiial state data
- Will set the component up with the data that it neeeds 'before' it gets mounted to the DOM
- Good for recieving props and translating those props to state. (Passed through super)
- Could be used to bind all class methods to the render function and vis versa (this is no longer necessary with modern syntax)

```jsx
constructor(props) {
	super(props);
	this.state = {
		someStateDate: [1,2,'three']
	}
}
```

## Render

- Required for all class components. The life-blood of every component, how things work.
- When triggered, your UI gets rendered to the DOM
- Involved in the mounting and updating phase

![Component%20Lifecycle%206aa368a26e5c4ddb87841933b3952e61/Untitled%201.png](Component%20Lifecycle%206aa368a26e5c4ddb87841933b3952e61/Untitled%201.png)

## componentDidMount()

- Part of the base React Class.Component
- Part of mounting phase. Invoked after render gets called. Will only be invoked once
- Used for async data loading that you need to have on state during render
- Method is guaranteed to be called only once in the whole lifecycle, immediately afer a component is mounted.

Inside of componentDidMount we can call setState which forces a re-render of our component. This way, any asynchronous actions should be performed inside of our componentDidMount function, especially when it comes to fetching data via HTTP. Data fetching is the de-facto purpose for using componentDidMount within a component because of its position within the component lifecycle.

```jsx
import React from "react";
import ReactDOM from "react-dom";

import "./styles.css";

class App extends React.Component {
  constructor() {
    super();
    this.state = {
      doggos: []
    };
  }
  componentDidMount() {
    fetch("https://dog.ceo/api/breed/hound/images")
      .then(res => res.json())
      .then(dogs => this.setState({ doggos: dogs.message }))
      .catch(err => console.log("noooo"));
  }
  render() {
    console.log(this.state);
    return (
      <div className="App">
        <h1>Hello CodeSandbox</h1>
        <h2>Start editing to see some magic happen!</h2>
        {this.state.doggos.map(doggo => <img src={doggo} key={doggo} />)}
      </div>
    );
  }
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```