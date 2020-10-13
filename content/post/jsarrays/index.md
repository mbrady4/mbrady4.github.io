# Arrays

Arrays have a special 0 based indexing (organizational) ability. They allow us to store data in them sequential based on 0. They are ordered values. They are mutable. 

Arrays are very fast at retrieving a specific item. Searching through arrays is not very efficient. 

- Everthing as a string is double quoted
- Typically arrays contain objects
- Nsme/value pairs can be used

The built-in methods all live on the JS Array Prototype. 

[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

```jsx
const hogwarts = ['Harry', 'Hermione', 'Ron'];  
hogwarts[0] === 'Harry';  
hogwarts[1] === 'Hermionie';  
hogwarts[3] === 'Ron';

hogwarts.push('Dumbledore');
```

To add an item to an array, you can use `.push()`

To remove an item from the end you can use `.pop()`

To add something to the front of the array use `.unshift()`

To remove an item from the front you can use `.shift()`

To remove at a specific index, you can use `.splice(1,1)` The second number is how many items to remove (not the stopping index)

get length of the array with `arrayName.length;` 

## .map()

The `map()` method creates a new array populated with the results of calling a provided function on every element in the calling array.

- Returns a new array of elements
- Calls back each element and index and returns each in turn
- Used for manipulating or reshaping data without modifying original array

```jsx
const array1 = [1, 4, 9, 16];

// pass a function to map
const map1 = array1.map(x => x * 2);

console.log(map1);
// expected output: Array [2, 8, 18, 32]
```

The most significant difference between `.forEach` and `.map` is that map returns a new array of elements while in turn passing each element back to the callback.

A `map()` creates an array, so a `return` is expected for all code paths (if/elses).

If you don't want an array or to return data, use `forEach` instead.

## foreach()

The `forEach()` method executes a provided function once for each array element.

```jsx
const array1 = ['a', 'b', 'c'];

array1.forEach(element => console.log(element));

// expected output: "a"
// expected output: "b"
// expected output: "c"
```

## filter()

The `filter()` method creates a new array with all elements that pass the test implemented by the provided function. 

- Returns a new array of elements
- Calls back each element, index and returns each in turn
- Takes a callback that runs a 'truth' test. If true, returns the elements, else ignores.
- Used for filtering out an array of elements by a specific condition

```jsx
const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];

const result = words.filter(word => word.length > 6);

console.log(result);
// expected output: Array ["exuberant", "destruction", "present"]
```

## reduce()

- Returns a new array of elements
- Takes a callback which is a reducer function
- Reducer function takes a previous value and a next value, known as accumulator and currentValue
- Used for manipulating or reshaping data into a single value.

```jsx
const reduceStatePopulations = data.reduce((total, state) => {
  return total += state.population;
}, 0);
```

The `0` above becomes the starting point for reduce. Otherwise it defaults to the first item in the array.

## Remove an item from an Array

```jsx
var ary = ['three', 'seven', 'eleven'];

// Remove item 'seven' from array
var filteredAry = ary.filter(function(e) { return e !== 'seven' })
//=> ["three", "eleven"]

// In ECMA6 (arrow function syntax):
var filteredAry = ary.filter(e => e !== 'seven')
```

[https://gist.github.com/ourmaninamsterdam/1be9a5590c9cf4a0ab42](https://gist.github.com/ourmaninamsterdam/1be9a5590c9cf4a0ab42)