# Basics

JavaScript utilizes a two-pass compiler when executing lines of code that we write. This means that anytime we run JavaScript in the browser, the browser will take two passes over our code. The first pass builds up references to all of our code, declaring variables and functions and the like. The second pass applies values to the references that were found, thus actually running the code.

### **STRICT MODE**

```jsx
> function defineX() { x = 1 return 'ok'
}
defineX()
```

```jsx
Result:
```

```jsx
ReferenceError: x is not defined
```

### **LET**

```jsx
> if (true) { let x = 1
}
x
```

```jsx
Result:
```

```jsx
ReferenceError: x is not defined
```

### **CONST**

```jsx
> const x = 1
x = 2
```

```jsx
Result:
```

```jsx
TypeError: Assignment to constant variable.
```

### **FOR OF LOOPS**

```jsx
> const letters = ['a', 'b', 'c']
const result = []
for (const letter of letters) { result.push(letter)
}
result
```

```jsx
Result:
```

```jsx
['a', 'b', 'c']
```

### **TEMPLATE LITERALS**

```jsx
> `1 + 1 = ${1 + 1}`
```

```jsx
Result:
```

```jsx
'1 + 1 = 2'
```

### **REST PARAMETERS**

```jsx
> function f(...args) { return args
}
f(1, 2, 3)
```

```jsx
Result:
```

```jsx
[1, 2, 3]
```

### **ACCESSORS IN OBJECT LITERALS**

```jsx
> const user = { realName: 'Amir', get name() { return this.realName }, set name(newName) { this.realName = newName },
}
```

```jsx
Result:
```

```jsx
undefined
```

### **BASIC ARRAY DESTRUCTURING**

```jsx
> const letters = ['a', 'b', 'c']
const [a, b, c, d] = letters;
[c, d]
```

```jsx
Result:
```

```jsx
['c', undefined]
```

```jsx
> const letters = ['a', 'b', 'c']
const [a, ...others] = letters
others
```

```jsx
Result:
```

```jsx
['b', 'c']
```

### **COMPUTED PROPERTIES**

```jsx
> const loginCounts = { ['Be' + 'tty']: 7
}
loginCounts.Betty
```

```jsx
Result:
```

```jsx
7
```

### **BASIC OBJECT DESTRUCTURING**

```jsx
> const user = {name: 'Amir', email: 'amir@example.com', age: 36}
const {name, age} = user;
[name, age]
```

```jsx
Result:
```

```jsx
['Amir', 36]
```

```jsx
> const key = 'NaMe'
const {[key.toLowerCase()]: value} = {name: 'Amir'}
value
```

```jsx
Result:
```

```jsx
'Amir'
```

### **PLACES WHERE DESTRUCTURING IS ALLOWED**

```jsx
> function getUserName({name}) { return name
}
getUserName({name: 'Ebony'})
```

```jsx
Result:
```

```jsx
'Ebony'
```

```jsx
> const users = [{name: 'Amir'}, {name: 'Betty'}]
const names = []
for (const {name} of users) { names.push(name)
}
names
```

```jsx
Result:
```

```jsx
['Amir', 'Betty']
```

### **SHORTHAND PROPERTIES**

```jsx
> const name = 'Amir'
const catName = 'Ms. Fluff'
const city = 'Paris'
const age = 36
const user = {name, catName, city, age};
[user.name, user.age]
```

```jsx
Result:
```

```jsx
['Amir', 36]
```

### **NESTED DESTRUCTURING**

```jsx
> const dataPoints = [ [10, 20], [30, 40]
]
const [[x1]] = dataPoints
x1
```

```jsx
Result:
```

```jsx
10
```

```jsx
> const user = { name: 'Amir', address: { city: 'Paris', },
}
const {address: {city}} = user
city
```

```jsx
Result:
```

```jsx
'Paris'
```

### **SHORTHAND METHODS**

```jsx
> const address = { city: 'Paris', country: 'France', addressString() { return `${this.city}, ${this.country}` },
}
address.addressString()
```

```jsx
Result:
```

```jsx
'Paris, France'
```

### **BIND**

```jsx
> const user = {name: 'Amir'}
function userName() { return this.name
}
const userNameBound = userName.bind(user)
userNameBound()
```

```jsx
Result:
```

```jsx
'Amir'
```

### **ARROW FUNCTIONS**

```jsx
> [1, 2, 3].map(x => x * 2)
```

```jsx
Result:
```

```jsx
[2, 4, 6]
```

```jsx
> const user = {name: 'Amir'}
const getName = ({name}) => name
getName(user)
```

```jsx
Result:
```

```jsx
'Amir'
```

```jsx
> const rest = (first, ...rest) => rest
rest(1, 2, 3, 4)
```

```jsx
Result:
```

```jsx
[2, 3, 4]
```

### **CLASSES**

```jsx
> class Cat { constructor(name, vaccinated) { this.name = name this.vaccinated = vaccinated } needsVaccination() { return !this.vaccinated }
}
new Cat('Ms. Fluff', true).needsVaccination()
```

```jsx
Result:
```

```jsx
false
```

### **EXTENDING CLASSES**

```jsx
> class Animal { constructor(name) { this.name = name }
}
class Cat extends Animal { constructor(name) { super(name + ' the cat') }
}
new Cat('Ms. Fluff').name
```

```jsx
Result:
```

```jsx
'Ms. Fluff the cat'
```

### **FUNCTION NAME PROPERTY**

```jsx
> ( function() { return 5 }
).name
```

```jsx
Result:
```

```jsx
''
```

```jsx
> const f = function() { return 5 }
const f2 = f
f2.name
```

```jsx
Result:
```

```jsx
'f'
```

### **ANONYMOUS AND INLINE CLASSES**

```jsx
> new ( class { speak() { return 'yaong' } }
)().speak()
```

```jsx
Result:
```

```jsx
'yaong'
```

### **ACCESSOR PROPERTIES ON CLASSES**

```jsx
> class User { constructor(name) { this.actualName = name } get name() { return `${this.actualName} the user` }
}
new User('Betty').name
```

```jsx
Result:
```

```jsx
'Betty the user'
```

```jsx
> class User { constructor(name) { this.actualName = name } set name(newName) { this.actualName = newName }
}
const user = new User('Amir')
user.name = 'Betty'
user.actualName
```

```jsx
Result:
```

```jsx
'Betty'
```

### **DEFAULT PARAMETERS**

```jsx
> function add(x, y=0) { return x + y
}
[add(1, 2), add(1)]
```

```jsx
Result:
```

```jsx
[3, 1]
```

### **JSON STRINGIFY AND PARSE**

```jsx
> JSON.stringify({a: 2}) === '{"a":2}'
```

```jsx
Result:
```

```jsx
true
```

```jsx
> JSON.parse('{"a": 1, "b" : 2}')
```

```jsx
Result:
```

```jsx
{a: 1, b: 2}
```

```jsx
> const user = { name: 'Amir', toJSON: () => 'This is Amir!'
}
JSON.parse(JSON.stringify(user))
```

```jsx
Result:
```

```jsx
'This is Amir!'
```

### **STRING KEYED METHODS**

```jsx
> const user = { 'name of the ~user~'() { return 'Betty' }
}
user['name of the ~user~']()
```

```jsx
Result:
```

```jsx
'Betty'
```

### **STATIC METHODS**

```jsx
> class User { static getUserName(user) { return user.name }
}
User.getUserName({name: 'Amir'})
```

```jsx
Result:
```

```jsx
'Amir'
```

```jsx
> class User { static get defaultName() { return 'Amir' }
}
User.defaultName
```

```jsx
Result:
```

```jsx
'Amir'
```

### **COMPUTED METHODS AND ACCESSORS**

```jsx
> class User { constructor(name) { this.name = name } get ['user' + 'Name']() { return this.name }
}
new User('Betty').userName
```

```jsx
Result:
```

```jsx
'Betty'
```

### **SYMBOL BASICS**

```jsx
> Symbol('a') === Symbol('a')
```

```jsx
Result:
```

```jsx
false
```

```jsx
> const obj = {}
obj[true] = 1
Object.keys(obj)
```

```jsx
Result:
```

```jsx
['true']
```

```jsx
> const nameSymbol = Symbol('name')
const user = {[nameSymbol]: 'Amir'}
user[nameSymbol]
```

```jsx
Result:
```

```jsx
'Amir'
```

### **ISNAN**

```jsx
> isNaN(undefined)
```

```jsx
Result:
```

```jsx
true
```

```jsx
> Number.isNaN(undefined)
```

```jsx
Result:
```

```jsx
false
```

```jsx
> Number.isNaN(NaN)
```

```jsx
Result:
```

```jsx
true
```

### **BUILTIN SYMBOLS**

```jsx
> const user = { name: 'Amir', [Symbol.toStringTag]: 'Amir'
}
user.toString()
```

```jsx
Result:
```

```jsx
'[object Amir]'
```

### **NEW NUMBER METHODS**

```jsx
> Number.isFinite(-Infinity)
```

```jsx
Result:
```

```jsx
false
```

```jsx
> Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 1)
```

```jsx
Result:
```

```jsx
false
```

```jsx
> 2 ** 8
```

```jsx
Result:
```

```jsx
256
```

### **DEFINING ITERATORS**

```jsx
> const letters = ['a', 'b', 'c']
const iterator = letters[Symbol.iterator]()
iterator.next()
iterator.next()
```

```jsx
Result:
```

```jsx
{done: false, value: 'b'}
```

```jsx
> const letters = ['a', 'b', 'c']
const iterator = letters[Symbol.iterator]()
iterator.next()
iterator.next()
iterator.next()
iterator.next()
```

```jsx
Result:
```

```jsx
{done: true, value: undefined}
```

### **GENERATORS**

```jsx
> function* numbersBelow(maximum) { for (let i=0; i<maximum; i++) { yield i }
}
Array.from(numbersBelow(3))
```

```jsx
Result:
```

```jsx
[0, 1, 2]
```

### **PROBLEMS WITH OBJECT KEYS**

```jsx
> const obj = { [{key1: 'value1'}]: 1, [{key2: 'value2'}]: 2,
}
Object.keys(obj)
```

```jsx
Result:
```

```jsx
['[object Object]']
```

### **ITERATORS**

```jsx
> function* letters() { yield 'a' yield 'b' yield 'c'
}
const [, b, c] = letters();
[b, c]
```

```jsx
Result:
```

```jsx
['b', 'c']
```

```jsx
> const notAnIterator = 5
for (const x of notAnIterator) {
}
```

```jsx
Result:
```

```jsx
TypeError: notAnIterator is not iterable
```

### **MAPS**

```jsx
> const userEmails = new Map([ ['Amir', 'amir@example.com'], ['Betty', 'betty@example.com']
])
userEmails.get('Betty')
```

```jsx
Result:
```

```jsx
'betty@example.com'
```

```jsx
> const emails = new Map()
emails.set('Betty', 'betty.j@example.com')
emails.set('Betty', 'betty.k@example.com')
emails.get('Betty')
```

```jsx
Result:
```

```jsx
'betty.k@example.com'
```

### **SETS**

```jsx
> const names = new Set(['Amir', 'Betty', 'Cindy'])
names.add('Dalili')
names.has('Dalili')
```

```jsx
Result:
```

```jsx
true
```

```jsx
> const names = new Set(['Amir'])
names.add('Betty')
names.add('Betty')
Array.from(names.values())
```

```jsx
Result:
```

```jsx
['Amir', 'Betty']
```

### **TAGGED TEMPLATE LITERALS**

```jsx
> function doubleNumbers(strings, ...values) { let result = '' for (let i=0; i<strings.length; i++) { result += strings[i] if (i < values.length) { result += (2 * values[i]).toString() } } return result
}
doubleNumbers`the numbers ${1} and ${2}`
```

```jsx
Result:
```

```jsx
'the numbers 2 and 4'
```

### **SPREAD**

```jsx
> const numbers = [ 1, ...[2, 3], 4,
]
numbers
```

```jsx
Result:
```

```jsx
[1, 2, 3, 4]
```

```jsx
> const amir = { name: 'Amir', age: 36,
}
const amirWithEmail = { ...amir, email: 'amir@example.com'
}
amirWithEmail
```

```jsx
Result:
```

```jsx
{age: 36, email: 'amir@example.com', name: 'Amir'}
```

### **CLASS SCOPING**

```jsx
> function createGorilla(name) { class Gorilla { constructor() { this.name = name } } return new Gorilla()
}
createGorilla('Michael').name
```

```jsx
Result:
```

```jsx
'Michael'
```

```jsx
> if (true) { class Cat { }
}
new Cat()
```

```jsx
Result:
```

```jsx
ReferenceError: Cat is not defined
```

### **CUSTOMIZING JSON SERIALIZATION**

```jsx
> JSON.parse( JSON.stringify( {age: 36, city: 'Paris', name: 'Amir'}, ['name', 'city'] )
)
```

```jsx
Result:
```

```jsx
{city: 'Paris', name: 'Amir'}
```

```jsx
> JSON.parse( JSON.stringify( {catName: 'Ms. Fluff', city: 'Paris', name: 'Amir'}, (key, value) => { if (key === 'catName') { return undefined } else { return value } } )
)
```

```jsx
Result:
```

```jsx
{city: 'Paris', name: 'Amir'}
```

```jsx
> JSON.parse( `{"name": "Amir", "age": 36}`, (key, value) => { if (key === 'age' && value === 36) { return 'thirty six' } else { return value } }
)
```

```jsx
Result:
```

```jsx
{age: 'thirty six', name: 'Amir'}
```

### **PROPERTY ORDER**

```jsx
> const user = {name: 'Amir', age: 36}
user.email = 'amir@example.com'
// Object keys come back in the order they were assigned.
Object.keys(user)
```

```jsx
Result:
```

```jsx
['name', 'age', 'email']
```

### **SYMBOLS ARE METADATA**

```jsx
> const user = { name: 'Amir', [Symbol.toStringTag]: 'Amir'
}
JSON.parse(JSON.stringify(user))
```

```jsx
Result:
```

```jsx
{name: 'Amir'}
```

### **MAP ITERATORS**

```jsx
> const catAges = new Map([ ['Ms. Fluff', 4], ['Keanu', 2],
])
Array.from(catAges.keys())
```

```jsx
Result:
```

```jsx
['Ms. Fluff', 'Keanu']
```

```jsx
> const catAges = new Map([ ['Keanu', 2], ['Ms. Fluff', 4],
])
Array.from(catAges.values())
```

```jsx
Result:
```

```jsx
[2, 4]
```

```jsx
> const emails = new Map()
emails.set('Amir', 'amir@example.com')
emails.set('Betty', 'betty@example.com')
Array.from(emails)
```

```jsx
Result:
```

```jsx
[['Amir', 'amir@example.com'], ['Betty', 'betty@example.com']]
```

### **SET OPERATIONS**

```jsx
> const set1 = new Set([1, 2, 3])
const set2 = new Set([2, 3, 4])
// Set union: all of the elements in either set.
const unionSet = new Set([...set1, ...set2]);
[unionSet.has(1), unionSet.has(4)]
Array.from(unionSet)
```

```jsx
Result:
```

```jsx
[1, 2, 3, 4]
```

```jsx
> const set1 = new Set([1, 2, 3])
const set2 = new Set([2, 3, 4])
// Set intersection: all of the elements in both sets.
const intersectionSet = new Set( Array.from(set1).filter(n => set2.has(n))
);
Array.from(intersectionSet)
```

```jsx
Result:
```

```jsx
[2, 3]
```

```jsx
> const set1 = new Set([1, 2, 3])
const set2 = new Set([2, 3, 4])
// Set difference: all of the elements in set1 but not set2.
const differenceSet = new Set( Array.from(set1).filter(n => !set2.has(n))
);
Array.from(differenceSet)
```

```jsx
Result:
```

```jsx
[1]
```