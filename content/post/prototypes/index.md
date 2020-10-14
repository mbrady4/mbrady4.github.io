+++
title = "Prototypes"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
All objects in Javascript have a prototype property by default. This property is used as an object to attach methods and other properties that can be delegated down to other child functions/objects. 

## Constructors

A constructor function is a way to build objects. Constructors are object creator functions.

```jsx
function Animal(object) {
  this.name = object.name;
}
```

A constructor function “constructs” objects. You can think of it as a template. The function itself needs to take in an object literal of some sort so that it can map that object literal’s properties to a new object that will be returned once instantiated.

```jsx
// This is a constructor function
function Person(attributes) {
  this.age = attributes.age;
  this.name = attributes.name;
  this.homeTown = attributes.homeTown;
  this.speak = function () {
    return `Hello, my name is ${this.name}`;
  };
}

// This creates a new Person 
const fred = new Person({
  age: 35,
  name: 'Fred',
  homeTown: 'Bedrock'
});
```

When `new` is called, the constructor function can essentially create a context for a this object. Then what gets returned from that constructor function is that particular this object with the new properties added to it.

In the above example, the fred object will inherit the `speak` function from the Person object.

## Prototypes

The prototype is the mechanism by which all JavaScript objects inherit from one another. You can think of the prototype as an object that objects use to hold onto values that can be passed down to other objects.

To refactor the above example to use Prototypes you would do the following: 

```jsx
function Person(attributes) {
  this.age = attributes.age;
  this.name = attributes.name;
  this.homeTown = attributes.homeTown;
}

Person.prototype.speak = function () {
  return `Hello, my name is ${this.name}`;
};
```

Now that we have added the speak function to the prototype of Person, it will no longer be on the object fred. The Person prototype wholly owns speak. Person is now able to pass down speak to each instance of Person without creating a new property on any new objects.

### Inheritance with Prototypes

Here we create a Child constructor. Notice we are using the call() method to bind this to Person.

```jsx
function Child(childAttributes) {
  Person.call(this, childAttributes); // binding this to Person
  this.isChild = childAttributes.isChild; // this will be a special attribute to Child
}
```

The problem with Child is that it doesn’t necessarily know about the Person prototype yet. We have to manually tell Child about Person using Object.create().

```jsx
Child.prototype = Object.create(Person.prototype);
```

## In-depth example from training kit

```jsx
function Fruit(attrs) {
  this.type = attrs.type;
  this.name = attrs.name;
  this.isRipe = attrs.isRipe;
  this.calories = attrs.calories;
}

Fruit.prototype.shipped = function(destination) {
  console.log(`Shipping ${this.name} to ${destination}`);
};

Fruit.prototype.calculateCals = function() {
  console.log(`Calories in ${this.name} are ${this.calories * 200}`);
};

function Banana(bananaAttrs) {
  Parent.call(this, bananaAttrs);
  this.doMonkeysLikeIt = bananaAttrs.doMonkeysLikeIt;
}

Banana.prototype = Object.create(Fruit.prototype);

Banana.prototype.checkIfMonkeysLikeIt = function() {
  if(this.doMonkeysLikeIt) {
    return true;
  } else {
    return false;
  }
};

function Kiwi(kiwiAttrs) {
  Fruit.call(this, kiwiAttrs);
  this.isFuzzy = kiwiAttrs.isFuzzy;
}

Kiwi.prototype = Object.create(Fruit.prototype);

Kiwi.prototype.checkIfFuzzy = function() {
   if(this.isFuzzy) {
    return true;
  } else {
    return false;
  }
}

const newBanana = new Banana({
  doMonkeysLikeIt: true,
  type: 'Tree',
  name: 'Banana',
  isRipe: false,
  calories: 0.1
});

const newKiwi = new Kiwi({
  isFuzzy: true,
  type: 'Tree',
  name: 'Kiwi',
  isRipe: false,
  calories: 0.7
});

console.log(newBanana.shipped('Alaska'));
console.log(newKiwi.shipped('Colorado'));
console.log(newBanana.checkIfMonkeysLikeIt());
console.log(newKiwi.checkIfFuzzy());
console.log(newBanana.calculateCals());
console.log(newKiwi.calculateCals());
```