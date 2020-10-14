+++
title = "Reducers"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
Redux is built on the programming principle of immutability. This means  that the Redux store cannot be mutated, which will save us from running into more bugs and weird side effects, and will make it easier to reason about our code. 

**Advantages of immutability**

An immutable value, after it has been created, can never change. Benefits of making your state immutable include:

- **Predictability:** Mutation hides change, which can create (unexpected) side effects. This can lead to some nasty bugs in our code. When we enforce immutability, we can keep our application architecture and mental model simple, which makes it easier to reason about the application. Simply put, it is very easy to predict how the state object will change based on certain actions/events. Without immutability, our state object can be changed or updated in unpredictable ways, causing weird behavior and/or bugs.
- **Mutation Tracking:** Immutability makes it really easy to see if anything has changed. For example when we change the state in Redux, our components props will update. We can check our previous props against our new props to know what change occurred, and know how to handle those changes.

Value Vs. Reference In Javascript

Variables that are assigned a non-primitive value are given a reference to that value. That reference points to the object’s location in memory. The variables don’t actually contain the value.

- Javascript has 5 data types that are passed by ***value***: `Boolean`, `null`, `undefined`, `String`, and `Number`. We’ll call these **primitive types**.
- Javascript has 3 data types that are passed by ***reference***: `Array`, `Function`, and `Object`. These are all technically Objects, so we’ll refer to them collectively as **Objects**.

A function that takes in an Object, however, can mutate the state of its surrounding scope. If a function takes in an array reference and alters the array that it points to, perhaps by pushing to it, variables in the surrounding scope that reference that array see that change. After the function returns, the changes it makes persist in the outer scope. This can cause undesired side effects that can be difficult to track down.

Many native array functions, including `Array.map` and `Array.filter`, are therefore written as pure functions. They take in an array reference and internally, they copy the array and work with the copy instead of the original. This makes it so the original is untouched, the outer scope is unaffected, and we’re returned a reference to a brand new array. We refer to functions that don’t affect anything in the outside scope as pure functions.

```jsx
function changeAgePure(person) {
    var newPersonObj = JSON.parse(JSON.stringify(person));
    newPersonObj.age = 25;
    return newPersonObj;
}
var alex = {
    name: 'Alex',
    age: 30
};
var alexChanged = changeAgePure(alex);
console.log(alex); // -> { name: 'Alex', age: 30 }
console.log(alexChanged); // -> { name: 'Alex', age: 25 }
```

[Explaining Value vs. Reference in Javascript](https://codeburst.io/explaining-value-vs-reference-in-javascript-647a975e12a0)

**Redux and Immutability**

Redux has a single immutable state tree (referred to as the store) where all state changes are explicitly handled by dispatching actions. Dispatched actions are processed by a reducer that accepts the previous state and the action and returns the next state of your application. It is easy to predict how the state tree is going to change based on actions that are dispatched. It is also easy to predict which action will be dispatched based on some event or interaction. This all leads to very predictable state management.

**Reducer**

```jsx
const initialState = { count: 0 }
const reducer = (state, action) => {
  switch(action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 }
    default:
      return state;
  }
}

reducer(initialState, { type: 'increment' });
reducer(initialState, { type: 'decrement' });
```

You can also add a payload or data to as an input on the action object:

```jsx
const initialState = { name: 'Donald Duck' }
const reducer = (state, action) => {
  if (action.type === 'changeName') {
    // how do we know what to change the name to? The action payload!
    return { name: action.payload }
  }
}

reducer(initialState, { type: 'changeName', payload: 'Mickey Mouse' });
```

## userReducer

The useReducer hook is an alternative to useState (useState actually uses useReducer hook under the hood). It is preferable when you have complex logic that you have to deal with in a component, or when you find yourself with a lot of state properties (more than 3) in a single component. useReducer, takes in a reducer function (that we build), as well as a value for the initialState. Then it returns both the current state and a dispatch method in an array, just like useState does.

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

**pure functions**

1. no side effects

2. always returns the same result when given the same inputs

**reducer definition**

1. pure function

2. that takes 2 objects and returns 1

3. for useReducer (and redux) the first object is a state object, the second is an action object

4. the reducer will return a new state object based on the action applied to the passed in state

**action definition**

1. an object that has a required 'type' key

2. the action object will have an optional 'payload' key

**Full example:**

```jsx
import React, { useReducer } from 'react'

const initialState = { count: 0 }
// Initial count is established

// We will use the same reducer we created in the previous section
function reducer(state, action) {
  switch (action.type) {
    case 'INCREASE':
      return { count: state.count + 1 }
    case 'DECREASE':
      return { count: state.count - 1 }
    default:
      return state
  }
}

// Create a functional component
function Counter() {
  // Use the useReducer hook by destructuring its two properties: state, and dispatch and pass in the reducer and the initialState to the useReducer function
  const [state, dispatch] = useReducer(reducer, initialState)

  // Return JSX that displays the count for the user
  // Note the two button elements which allow the user to increase and decrease the count.  Each of them contains an onClick event that dispatches the desired action object, with its given type.  Each action, when fired, is dispatched to the reducer and the appropriate logic is applied.
  return (
    <>
      {/* Note, we have access to the current state and the dispatch method from the useReducer hook, so we can utilize them to display the count as well as couple the dispatching of the actions from the appropriate buttons.*/}
      <div className="count">Count: {state.count}</div>
      <button onClick={() => dispatch({ type: 'INCREASE' })}>+1</button>
      <button onClick={() => dispatch({ type: 'DECREASE' })}>-1</button>
    </>
  )
}
```

[The Reducer Pattern - webpt15](https://codesandbox.io/s/the-reducer-pattern-webpt15-se2pk)