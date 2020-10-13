# Initializing the Store

Redux lets me pass the persistentState as the second argument to the createStore function, and it will override the value specified by the reducers.

```sql
const store = createStore(
  todoApp,
  persistedState
);
```

This is helpful if we want to load some existing data into the app synchronously before it starts.

## From Local Storage

**Load State**

If the serializedState is null it means that the local storage value does exist so I'll return undefined to let the reducers initialize the state instead. However if the serializedState string exists I'm going to use JSON.parse in order to turn it into the state object. Finally in case of any errors I'm going to play it safe and return undefined to let reducers initialize the application.

```sql
export const loadState = () => {
  try {
    const serializedState = localStorage.getItem('state');
    if (serializedState === null) {
      return undefined;
    }
    return JSON.parse(serializedState);
  } catch (err) {
    return undefined;
  }
};
```

**Save State**

```sql
export const saveState = (state) => {
  try {
    const serializedState = JSON.stringify(state);
    localStorage.setItem('state', serializedState);
  } catch (err) {
    // Ignore write errors.
  }
};
```

**Subscribe to store updates in order to save to local storage**

store.subscribe method to add a listener that will be invoked on any state change, and I'm passing the current state of the store to my saveState function.

```sql
store.subscribe(() => {
  saveState(store.getState());
});
```

**Load persisted state to store on app load if not undefined**

```sql
const persistedState = loadState();

const store = createStore(
  todoApp,
  persistedState
);
```

## Refactor the Configuration of the Store into a module

```sql
// configureStore.js
const configureStore = () => {
  const persistedState = loadState();
  const store = createStore(
    todoApp,
    persistedState
  );

  store.subscribe(throttle() => {
    saveState({
      todos: store.getState().todos,
    });
  }, 1000))

  return store;
};

// index.js
import configure from './configureStore';

const store = configureStore();

render(
  <Root store={store} />
  document.getElementById('root')
);

// root.js
import React from 'react';

const Root = ({ store }) => (
  <Provider store={store}>
    <App />
  </Provider>
);
```

## Add React Router to Project with Redux

Put the router inside of the Provider tags wrapping the root

```sql
import { Router, Route } from 'react-router';
import App from './App';

const Root = ({ store }) => (
  <Provider store={store}>
    <Router>
      <Route path='/' component={App} />
    </Router>
  </Provider>
);
```

[Using withRouter() to Inject the Params into Connected Components](https://www.notion.so/Using-withRouter-to-Inject-the-Params-into-Connected-Components-5ae038547882414e81eefd6620f8fe9c)

## Selectors

[Selectors](https://www.notion.so/Selectors-d09f5b8bda2f42d789d29022feea39d3)

## Add Logging to Redux for State changes

```sql
// configureStore.js

if (process.env.NODE_ENV !== 'production') {
    store.dispatch = addLoggingToDispatch(store);
}

const addLoggingToDispatch = (store) => {
  const rawDispatch = store.dispatch;
	if (!console.group) {
    return rawDispatch;
  }
  return (action) => {
    console.group(action.type);
    console.log('%c prev state', 'color: gray', store.getState());
    console.log('%c action', 'color: blue', action); 
    const returnValue = rawDispatch(action);
    console.log('%c next state', 'color: green', store.getState());
    console.groupEnd(action.type);
    return returnValue;
  };
};
```

[Copy of Wrapping Dispatch to log actions](https://www.notion.so/Copy-of-Wrapping-Dispatch-to-log-actions-4c195865ab2e44b5a16a24dfc1f74fe5)

## Adding Middleware

We use the function called `applyMiddleware` that we import from Redux. It accepts the middleware functions as positional arguments. For example, we could have written `promise`, `createLogger`. However, we only want to apply the logger conditionally. If we're not running in the production environment, we will push the logger to the array of middlewares. We use the [ES6 spread operator](https://egghead.io/lessons/ecmascript-6-using-the-es6-spread-operator) to pass every middleware as a positional argument to apply middleware.

The `applyMiddleware` returns something called an **enhancer**. This is the optional last argument to create store. If you want to also supply the `persistentState`, you need to do this before the enhancer.

```jsx
import { createStore, applyMiddleware } from 'redux';

const configureStore = () => {
  const middlewares = [promise];
  if (process.env.NODE_ENV !== 'production') {
    middlewares.push(createLogger());
  }

  return createStore(
    todoApp,
    applyMiddleware(...middlewares)
  );
};
```