+++
title = "JS Functions"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
## Function Declarations

Functions are hoisted by the compiler so you can invoke before you define . 

Function declarations are made up of four syntactical parts: (1) The `function` keyword, the name of the function, an optional list of parameters, and the statements inside of the block of code {}

```jsx
add(4,5) 

function add(num1, num2) {
	return num1 + num2;
}
```

## Function Expressions

Not hoisted, you cannot invoke before you define. Usually anonymous. A variable is used to store the function for later use.

```jsx
let multiply = function(num1, num2) {
	return num1 * num2;
}

multiply(2,8)
```

### Arrow Functions

Arrow functions have non-binding of the `this` keyword. Arrow functions shine best with anything that requires this to be bound to the context, and not the function itself. They are short. 

```jsx
let multiply = (num1, num2) => {
	return num1 - num2;
}

// streamlined arrow syntax
const add = (a,b) => a + b;
```

[When (and why) you should use ES6 arrow functions - and when you shouldn't](https://www.freecodecamp.org/news/when-and-why-you-should-use-es6-arrow-functions-and-when-you-shouldnt-3d851d7f0b26/)

Once the curly braces are present, you always need the return statement. 

```jsx
var feedTheCat = (cat) => {
  if (cat === 'hungry') {
    return 'Feed the cat';
  } else {
    return 'Do not feed the cat';
  }
}
```

## Immediately Invoked Function Expression

Does not require an invocation when referenced

```jsx
let talk = (function () {
	return "I was invoked immediately!";
})();

// notice no invocation of talk()
console.log(talk);
```