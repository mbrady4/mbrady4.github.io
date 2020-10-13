# Component Side Effects

A side effect is anything that affects something outside the scope of the function being executed. Fetching data from an API, timers, logging, and manually manipulating the DOM are all examples of side effects. There are two categories of side effects in React components - those that don’t require cleanup and those that do require cleanup.

A React component without side effects is called a pure component. A component is considered pure if it always renders the same output for the same state and props. Similarly, a side effect is something that can cause a component to return a different output for the same state and props.

## The effect Hook

The effect hook tells React that a component needs to run, or execute, some side effect. This hook takes in two parameters. The first is a callback function where we can run the side effect. The second is an array that specifies elements that we want the effect hook to stay in sync with (either state or prop changes).

```jsx
// Making API calls
const [user, setUser] = useState();
const [error, setError] = useState();
useEffect(() => {
  fetchUserData(userId)
    .then(res => setUser(res.data.user))
    .catch(err => setError(err.response.message));
});

// Manipulating the DOM
const [count, setCount] = useState();
useEffect(() => {
  document.title = `Count is: ${count}`;
});

useEffect(() => {
  console.log("The component has mounted.");
}, []);
```

Note: Even with a dependency array added to the effect hook, it will fire when the component mounts, and then only fire when the given dependencies change.

**Fetching data from a WEB API**

```jsx
function App() {
  // Initialize state to hold the image URL
  const [dogPic, setDogPic] = useState("");
  useEffect(() => {
    // This axios GET request will return a single image
    axios
      .get("https://dog.ceo/api/breeds/image/random")
      // Which we then set to state
      .then(res => setDogPic(res.data.message))
      // Always include error handling
      .catch(err => console.log(err));
  }, []);

  return (
    <div className="App">
      <h1>We Love Puppers</h1>
      <img src={dogPic} alt="a random dog" />
    </div>
  );
}
```

**Example #2:** 

```jsx
function App() {
  const [data, setData] = useState({ hits: [] });
  const [query, setQuery] = useState("react");

  useEffect(() => {
    const fetchData = () => {
      axios
        .get("https://hn.algolia.com/api/v1/search?query=" + query)
        .then(res => setData(result.data));
    };

    fetchData();
  }, [query]);

  return (
    <>
      <input value={query} onChange={e => setQuery(e.target.value)} />
      <ul>
        {data.hits.map(item => (
          <li key={item.objectID}>
            <a href={item.url}>{item.title}</a>
          </li>
        ))}
      </ul>
    </>
  );
}
```

```jsx
import React, { useState, useEffect } from "react";
import ReactDOM from "react-dom";

function App() {
  const [count, setCount] = useState(0);
  const [isHappy, setIsHappy] = useState(true);

  // Runs after React makes a change to the DOM
  useEffect(() => {
    // request data from API
    // scheudle a timer
    console.log(`Count is ${count} and you see me every time.`);
  });

  // Use separate UseEffect functions for each thing you want to do
  useEffect(() => {
    // request data from API
    // scheudle a timer
    console.log(`Another effect`);
  });

	// Only trigger when count is changed
	useEffect(() => {
	    // request data from API
	    // scheudle a timer
	    console.log(`Count is ${count} and you see me every time.`);
	  }, [count]);

  return (
    <div>
      <h1>count: {count}</h1>
      {
        isHappy 
        ? <h3>So Happy!</h3> 
        : <h3>Not so Happy..</h3>
      }
      <button onClick={e => setCount(count+1)}>Increment Count</button>
      <button onClick={e => setIsHappy(true)}>Increment Happy</button>
      <button onClick={e => setIsHappy(false)}>Make Unhappy :(</button>
    </div>
  )
}

const rootElement = document.getElementById("root");
ReactDOM.render(
    <App />,
  rootElement
);
```

### Clean Ups

Imagine that you write an effect function for a component that doesn’t get cleaned up, then your user navigates back and forth to the component ten times. You now have 10 effect functions all running the same effect over and over again. You’ve increased the power and memory needed to run that function 10x’s! Ahhh! That does not sound good, right? It’s not! This is what we call a memory leak.

To clean up an effect, we return a function from the effect hook's callback function, like this:

```jsx
useEffect(() => {
  // We write our desired effect as before.
  console.log("The Effect Hook has run.");
  // Returning a function will tell React that you want this
  // code to run when the component unmounts or re-renders
  return () => console.log("The Effect Hook has been cleaned up.");
});
```

**Example:**

```jsx
const App = () => {
  const [position, setPosition] = useState({ x: 0, y: 0 });

  useEffect(() => {
    const setFromEvent = e => setPosition({ x: e.clientX, y: e.clientY });
    window.addEventListener("mousemove", setFromEvent);

    // the function returned here will remove, or "clean up", the event listener
    return () => {
      window.removeEventListener("mousemove", setFromEvent);
    };
  }, []);

  return (
    <div>
      {position.x}:{position.y}
    </div>
  );
};
```