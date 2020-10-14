+++
title = "Callbacks"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
**Callback functions are just functions that are passed into other functions as arguments.** 

> A callback is simply a function that is passed into another function.
That function is then invoked inside the outer function and does some action.

Functions are first class citizens (can store function as a variable and pass reference around to other variables). Functions can be passed as a parameter to another function.Functions are a Type in Javascript. Functions are just a set of instructions to complete a task. 

```jsx
const elements = ['earth', 'wind', 'fire'];

function showFirst(array, callback) {
  callback(array[0]);
}

function showLength(list, cb) {
  cb(list.length);
}

showFirst(elements, (firstItem) => {
  console.log(firstItem);
})

showLength(elements, (lengthOfList) => {
  console.log(lengthOfList);
})

const newArray = elements.map((el, index) => {
  return `${el} ${index}`; 
});

console.log(newArray);
console.log(elements);
```