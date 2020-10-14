+++
title = "Redux"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
Redux is a predictable state management library for JavaScript applications and is the most popular State container for React applications. The core concepts of Redux are:

- **The Store (Single source of truth):** Everything that changes within your application is represented by a single JavaScript object known as the store. The store contains our state for our application.
- **Application state is immutable:** When the application state changes, we clone the state object, modify the clone, and replace the original state with the new copy. We never mutate the original object, and we never write to our store object.
- **Pure functions change our state:** Given the same input, a pure function returns the same output every time. All functions (Reducers) in Redux must be pure functions. Meaning they take in some state and a description of what changes took place and return a copy of our state.
    - Pure functions are the functions whose returned value depends solely on the values of their arguments.
    - Pure functions do not modify the values passed to them.

In a typical Redux app, there is just a single store with a single root reducing function. As your app grows, you split the root reducer into smaller reducers independently operating on the different parts of the state tree. This is exactly like how there is just one root component in a React app, but it is composed out of many small components.

The main idea is that you describe how your state is updated over time in response to action objects, and 90% of the code you write is just plain JavaScript, with no use of Redux itself, its APIs, or any magic.

## **Redux data flow:**

1. Store sets the state
2. Event occurs in the UI
3. An action that describes the event is dispatched
4. The reducer updates the state tree in a predictable way
5. The UI receives the updated state tree

![Redux%208348c2632a6545a198f82c802e99c33f/Untitled.png](Redux%208348c2632a6545a198f82c802e99c33f/Untitled.png)

Redux core principles:

1. **Single Source of truth:** Whether your app is really simple or a complex application with a lot of UI, and change of state, you are going to represent the whole state of your application as a single Javascript object.
2. **State is read only (immutable):** The second principle is that the state tree is read only. You cannot modify or write to it. Instead, anytime you want to change the state, you need to dispatch an action. 
3. **Changes are made with pure functions:** To describe state mutations, you have to write a function that takes the previous state of the app, the action being dispatched, and returns the next state of the app. This function has to be pure. This function is called the "Reducer."

## Installing Redux to a React Project

```jsx
npm install react-redux redux
```

React-redux gives us a <Provider></Provider> component that wraps around our entire application. We will pass our newly created store to that component as a prop.

**Trivial example of importing and setting up a data store:**

```jsx
import { Provider } from 'react-redux';
import { createStore } from 'redux';

function reducer() {
  return {
    title: 'Hello world! I\'m in the Redux store!',
  }
}

const store = createStore(reducer);

<Provider store={store}>
  <App/>
</Provider>
```

## Connecting a React component to the Data Store

We use the connect function, where we export the component at the bottom of the file. We invoke connect twice (function currying). First with two arguments - a function and an object. Second with just the component we are trying to connect. For now, we’ll pass null and {} into the first invocation.

```jsx
import { connect } from 'react-redux';

export default connect(mapStateToProps, {})(MovieList)
```

`mapStateToProps` function tells connect which pieces of our state we want to bring in to this component. This function takes in state as a parameter, then returns an object where the properties can be passed to props, and the values are retrieved from the store for our component.

```jsx
const mapStateToProps = state => {
  return {
    movies: state.movies,
    moviesToWatch: state.moviesToWatch,
    user: state.user
  }
}
```

![Redux%208348c2632a6545a198f82c802e99c33f/Untitled%201.png](Redux%208348c2632a6545a198f82c802e99c33f/Untitled%201.png)

## Implementing Actions

Actions in redux are packets of information that describe events that have occurred in the UI. In code, an action is simply an object. Actions are dispatched to the reducers, and tell the reducers how to update the state tree.

An action is simply an object with up to two properties - a type property and an optional payload property. Each action MUST have a type property. The type property is a string that explains what interaction just happened. By convention, we use all caps and underscores for types - ie 'LOGIN_USER or TOGGLE_TODO. The payload property is data that goes along with that interaction.

Actions are “dispatched” to our reducer - aka, passed into the reducer function as an argument. When our reducer recieves an action, it will update the state according to the type and payload on the action.

- An action creator is a function that "creates" an action by returning an action object.

```jsx
export const addMovie = movieTitle => {
	return {
		type: ADD_MOVIE;
		payload: payload
	}
};
```

- An action type is created to avoid hidden bugs, and used as the type in the action

```jsx
export const ADD_MOVIE = "ADD_MOVIE";
```

- When the action creator is invoked, and the action is returned, it will be dispatched (under the hood) to the reducer

![Redux%208348c2632a6545a198f82c802e99c33f/Untitled%202.png](Redux%208348c2632a6545a198f82c802e99c33f/Untitled%202.png)

## Reducers

When an action is dispatched, it flows through every reducer in a script.

A reducer in it's simplest form is a function that returns an object. The returned object will represent the state tree. 

- Reducers are pure functions
- Reducers take in the current state tree an action as arguments
- Using a switch case to check the action type of the dispatched action, create an updates state tree based on the action type and the action payload
- Each case in the switch statement returns the new, updated state tree, triggering the UI to re-render with the new data

Reducers take in two arguments, the current state from the Redux store, and the action object, sent via action creator functions. Remember that an action gives us a packet of information as an object with a type and payload field that we can use. The type tells the reducer what to do, and the payload tells the reducer what to update on state.

![Redux%208348c2632a6545a198f82c802e99c33f/Untitled%203.png](Redux%208348c2632a6545a198f82c802e99c33f/Untitled%203.png)

To connect the reducer to the rest of React, you need to import it into the root or `index.js` file and pass it into the `createStore` function. The convention is to call it `rootReducer`

```jsx
import rootReducer from './reducers/reducer';

const store = createStore(rootReducer);
```

[Practice redux reducers](https://codesandbox.io/s/j357oqxwov)

## Implementing Redux in React Workflow

1. Set up "empty" reducer and initial state
2. Set up store and provider
3. Connect components
4. Add events and event handlers in UI
5. Build action creators
6. Write the reducer logic for the actions
7. Rinse and repeat