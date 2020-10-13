# Immutable State Changes

## Add item to array

```jsx
const addCounter = (list) => {
	return [...list, newItem]l
};
```

## Remove Item from array

```jsx
const removeCounter = (list, index) => {
return [
	...list.slice(0, index),
	...list.slice(index + 1)
	];
};
```

## Change value within an array

```jsx
const incrementCounter = (list, index) => {
	return [
		...list.slice(0, index),
		list[index] + 1, 
		...list.slice(index + 1)
	];
};
```

## Change a property on an object

```jsx
const toggleTodo = (todo) => {
	return Object.assign({}, todo, {
		completed: !todo.completed
	});
};
```

## Toggle Property on a specific object in an array of objects

```jsx
case 'TOGGLE_TODO':
	return state.map(todo => {
	  if (todo.id !== action.id) {
	    return todo;
	  }
	
	  return {
	    ...todo,
	    completed: !todo.completed
	  };
	});
```

## Filter an array

```jsx
case 'SHOW_COMPLETED':
  return todos.filter(
    t => t.completed
  );
case 'SHOW_ACTIVE':
  return todos.filter(
    t => !t.completed
  );
```

## Simple Reducer and Action Test

```jsx
// Reducer
const todos = (state = [], action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        {
          id: action.id,
          text: action.text,
          completed: false
        }
      ];
    default:
      return state;
  }
};

const testAddTodo = () => {
  const stateBefore = [];
  const action = {
    type: 'ADD_TODO',
    id: 0,
    text: 'Learn Redux'
  };
  const stateAfter = [
    {
      id: 0,
      text: 'Learn Redux',
      completed: false
    }
  ];
  
  deepFreeze(stateBefore);
  deepFreeze(action);
  
  expect(
    todos(stateBefore, action)
  ).toEqual(stateAfter);
};

testAddTodo();
console.log('All tests passed.') || displayInPreview('All tests passed.');
```

## Deep Freeze Function

```jsx
// Function exported from deep-freeze lib
function deepFreeze (o) {
  if (o===Object(o)) {
    Object.isFrozen(o) || Object.freeze(o)
    Object.getOwnPropertyNames(o).forEach(function (prop) {
      prop==='constructor'||deepFreeze(o[prop])
    })
  }
  return o
}
```

[substack/deep-freeze](https://github.com/substack/deep-freeze)