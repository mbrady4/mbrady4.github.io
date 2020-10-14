+++
title = "Reducer Composition"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
Any time a function does too many things, you want to extract other functions from it, and call them so that every function only addresses a single concern.

In Redux, as a matter of convention, a reducer should accept two arguments, the current state and the action being dispatched, and it should return the next state. For non-root reducers, state can be the relevant part of the state tree

```jsx
// Handles adding a todo
const todo = (state, action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return {
        id: action.id,
        text: action.text,
        completed: false
      };
  }
};

// Root Reducer
const todos = (state = [], action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        todo(undefined, action)
      ];
		case 'TOGGLE_TODO':
		   return stat.map(t => todo(t, action));
		 default:
		    return state;
	}
}
```

In this case, this state refers to the individual todo, and not to the list of todos. Finally, there is no magic in Redux to make it work. We extracted the todo reducer from the todos reducer, so now we need to call it for every todo, and assemble the results into an array.

```jsx
switch (action.type) {
  case 'ADD_TODO':
    return { ... };
  case 'TOGGLE_TODO':
    return stat.map(t => todo(t, action));
  default:
    return state;
}
```

Next, you could refactor the `rootreducer` to be as follows:

```jsx
const todoApp = (state = {}, action) => {
  return {
    todos: todos(
      state.todos,
      action
    ),
    visibilityFilter: visibilityFilter(
      state.visibilityFilter,
      action
    )
  }
}
```

The above rootReducer will pass `undefined` as the `state` of the child reducers because the initial state of the combined reducer is an empty object, so all its fields are `undefined`. This gets the child reducers to return their initial states and populates the state object for the first time.

When an `action` comes in, it calls the reducers with the pass of the state that they manage and the action and combines the results into the new state object.

### With Redux CombineReducers:

```jsx
const { combineReducers } = Redux;
const todoApp = combineReducers({
  todos: todos,
  visibilityFilter: visibilityFilter
});
```

The only argument to combine reducers is an object. This object lets me specify the mapping between the state field names, and the reducers managing them. The return value of the `combineReducer` is called a **Reducer function**, which is pretty much equivalent to the reducer function I wrote by hand previously.

The keys of the object I configure `combinedReducers` with correspond to the fields that the state object is going to manage. The values of the object I have asked to `combineReducer`, are the producers we should call to update the correspondence state fields.

A useful convention is that, you always name reducers after the state keys they manage. Since the key names and the value names are the same with this convention, you can omit the values in the object provided to combineReducers:

```jsx
const todoApp = combineReducers({
  todos,
  visibilityFilter
});
```