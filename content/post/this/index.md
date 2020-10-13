# this

This is a pronoun to use in place of an object. Gives you the object's context. Nothing to do with where the function is written, but where and when the function is called.

It is impossible to know what ‘this’ points to, without some execution context, or knowledge of where a function is being called. The first place to start with the this keyword is to understand where a function is called. If you know the function’s context, you may be able to deduce what the ‘this’ keyword is pointing to.

1. **Global Binding:** When in the global scope, the value of "this" will be the window/console object.

```jsx
function sayName(name) {
  console.log(this);
  return name;
}
sayName("D'Artagnan");
```

2. **Implicit Binding:** Whenever a function is called by a preceding dot, the object left of the dot gets 'this'. 

```jsx
const myObj = {
  greeting: 'Hello',
  sayHello: function(name) {
    console.log(`${this.greeting} my name is ${name}`);
    console.log(this);
  }
};
myObj.sayHello('Ryan');
```

3. **New Binding:** Whenever a constructor function is used, this refers to the specific instance of the object that is created and returned by the constructor function. 

```jsx
function CordialPerson(greeter) {
  this.greeting = 'Hello ';
  this.greeter = greeter;
  this.speak = function() {
    console.log(this.greeting + this.greeter);
    console.log(this);
  };
}

const jerry = new CordialPerson('Newman');
const newman = new CordialPerson('Jerry');

jerry.speak();
newman.speak();
```

4. **Explicit Binding:** Whenever we use JavaScript’s call or apply method, this is explicitly defined. We can override how we set CordialPerson constructor objects by taking the object-oriented approach. We do so by calling them explicitly with new functions, .call and .apply