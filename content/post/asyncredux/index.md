+++
title = "Async Redux"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
## State machines

A state machine is a model of computation or in other words is a box that keeps your state and that state changes over time. A finite state machine has a finite number of states. This type of machine accepts an input and based on that input plus the current state decides what will be the next state.

![Async%20Redux%209617b00d8a4243769aad44040b487bdd/Untitled.png](Async%20Redux%209617b00d8a4243769aad44040b487bdd/Untitled.png)

Having to write such table brings lots of value to your development cycle because it asks the right questions:

- What are all the possible states that this UI may be in?
- What exactly could happen in every of the states?
- If a certain thing happens what is the result?

## Middleware

A common tool used in programming to intercept some process, run a function, then (usually) continue the process. A way to work with the data that is "flowing" through the process before that process finishes

Redux middleware intercepts every action that is dispatched before it gets to the reducer. Middleware can:

- Stop actions
- Forward an action untouched
- Dispatch a different action (or change the action itself)

![Async%20Redux%209617b00d8a4243769aad44040b487bdd/Untitled%201.png](Async%20Redux%209617b00d8a4243769aad44040b487bdd/Untitled%201.png)

### Redux-logger

`redux-logger` is a middleware library. It basically console.log's all actions as they flow through the action creates, before they hit the reducer.

```jsx
npm install redux-logger
```

To impement logger, simply import and pass the `applyMiddleware` function to `createStore()`

```jsx
import { applyMiddleware, createStore } from 'redux';
import logger from 'redux-logger';

const store = createStore(
  reducer,
  applyMiddleware(logger)
);
```

## Redux Thunk

Redux thunk is a middleware that provides the ability to handle asynchronous operations inside our Action Creators. Since the Redux action -> reducer flow is synchronous, we will use Redux Thunk to make the flow asynchronous and make API calls from our action creators.

`thunk` is another word for a function. But it’s not just any old function. It’s a special (and uncommon) name for a function that’s returned by another function.

```jsx
function not_a_thunk() {
  // this one is a "thunk" because it defers work for later:
  return function() {
    console.log('do stuff now');
  };
}
```

Redux thunk modifies the data flow:

1. When an action creator is called `redux-thunk` intercepts and acts on returned data. 
2. If the thing returned is an action, it forwards the action through to the reducer. But if the thing returned is a function (a thunk), then it invokes the thunk and passes the `dispatch` function as an argument to it.
3. The action-creator returned thunk has access to the dispatch function. Thus, we can run an async function, like an API call, and inside the `.then()` we can dispatch an action! 

### Importing Thunk

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore, applyMiddleware } from 'redux';
import { Provider } from 'react-redux';
import thunk from 'redux-thunk';

import rootReducer from './reducers';

import PokemonList from './components/PokemonList';
import './styles.css';

const store = createStore(rootReducer, applyMiddleware(thunk));

function App() {
  return (
    <div className="App">
      <PokemonList />
    </div>
  );
}

const rootElement = document.getElementById('root');
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  rootElement
);
```

### Creating API calls with Redux Thunk

```jsx
// 'actions/index.js'
import axios from 'axios';

export const FETCH_POKEMON_START = 'FETCH_POKEMON_START';
export const FETCH_POKEMON_SUCCESS = 'FETCH_POKEMON_SUCCESS';
export const FETCH_POKEMON_FAIL = 'FETCH_POKEMON_FAIL';

export const getPokemon = () => dispatch => {
  dispatch({ type: FETCH_POKEMON_START });
  axios
    .get('https://pokeapi.co/api/v2/pokemon/')
    .then(res =>
      dispatch({ type: FETCH_POKEMON_SUCCESS, payload: res.data.results })
    )
    .catch(err => dispatch({ type: FETCH_POKEMON_FAIL, payload: err }));
};
```

```jsx
// 'reducers/index.js'
import {
  FETCH_POKEMON_START,
  FETCH_POKEMON_SUCCESS,
  FETCH_POKEMON_FAIL
} from '../actions';

const initialState = {
  pokemon: [],
  error: '',
  isFetching: false
};

function reducer(state = initialState, action) {
  console.log('reducer', action);
  switch (action.type) {
    case FETCH_POKEMON_START:
      return {
        ...state,
        isFetching: true,
        error: ''
      };
    case FETCH_POKEMON_SUCCESS:
      return {
        ...state,
        pokemon: action.payload,
        isFetching: false,
        error: ''
      };
    case FETCH_POKEMON_FAIL:
      return {
        ...state,
        error: action.payload
      };
    default:
      return state;
  }
}

export default reducer;
```

[redux-thunk practice solution](https://codesandbox.io/s/vy6148rx97)