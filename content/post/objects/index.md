# Objects

Objects are super powerful. In fact, you may hear this a lot: **Everything in JavaScript is an object.** Objects are used throughout almost every single part of the JavaScript Programming language.

Objects in JavaScript are used as a way to store data and give the programmer access to that data when needed.

This ability to store data and call it is known as a property. Properties are organized as key:value pairs.

```jsx
const student = {
	name: "Jane",
	study: function(topic) {
		console.log('${this.name} likes to study ${topic}');
	}
	address: {
            line1: 'westwish st',
            line2: 'washmasher',
            city: 'wallas',
            state: 'WX'
  }
}

// Dot notation
student.study("code")
// Bracket notation
student["code"]

// create object methods outside of the object literal
student.play = function(acitvity) {
	console.log('${this.name} likes to study ${activity}');
}
```

**It’s imperative to remember that an object’s values are mutable by default.** Meaning, even though myPersonalObject is set to a const, its values can be changed. This is because myPersonalObject is set pointing to itself as the object, but we can change the props on an object all we want.

Object literals gain all the methods available to objects:  

[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)

```jsx

// Object Methods: entries
console.log(Object.entries(student));

// Access entry data
console.log(Object.entries(student)[1]);

// Access Keys only
console.log(Object.keys(student));

// Access Values only
console.log(Object.values(student));
```