# Classes

The class keyword is what we call “syntactic sugar” on top of the object built into JavaScript and the object’s prototype system. The class keyword is not a new way of achieving object-oriented inheritance in JavaScript.

> Remember that classes in JavaScript are just special functions.

```jsx
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
} 

const newRect = new Rectangle(400, 800);
console.log(newRect);

Logs out: 
​​​​​Rectangle { height: 400, width: 800 }
```

The `extend` keyword will abstract away any of the `[class.call](http://class.call)` syntax that would be used without classes. `super()` tells a parent's constructor to be concerned with the child's attributes and abstracts away the `object.create(this, class)` syntax.

```jsx
class Animal { 
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(this.name + ' makes a noise.');
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name);
  }

  speak() {
    console.log(this.name + ' barks.');
  }
}
```

Functions declared inside the class will be methods on the prototype not the object itself.