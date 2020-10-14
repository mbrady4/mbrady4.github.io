+++
title = "Concurrency"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
### **SETTIMEOUT**

```jsx
> const array = []
array.push('before')
setTimeout(() => array.push('it worked'), 2000)
array.push('after')
array
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
['before', 'after', 'it worked']
```

### **SETTIMEOUT AND FUNCTIONS**

```jsx
> function doAsyncWork() { const array = [] setTimeout(() => array.push('it worked'), 1) return array
}
const array = doAsyncWork()
array
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
['it worked']
```

### **CONCURRENT SETTIMEOUTS**

```jsx
> setTimeout(() => console.log('cat'), 1000)
setTimeout(() => console.log('dog'), 2000)
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
cat
dog
```

### **SEQUENTIAL SETTIMEOUTS**

```jsx
> const array = []
setTimeout(() => { setTimeout(() => { array.push('it worked') }, 500)
}, 500)
array
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
['it worked']
```

### **PROMISE THEN**

```jsx
> Promise.resolve(5) .then(n => n * 2) .then(n => n.toString())
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
{fulfilled: '10'}
```

### **PROMISES ARE ASYNCHRONOUS**

```jsx
> console.log('before')
Promise.resolve('some value') .then(() => { console.log('then1') }) .then(() => { console.log('then2') })
console.log('after')
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
before
after
then1
then2
```

### **OMITTING PROMISE VALUES**

```jsx
> Promise.resolve()
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
{fulfilled: undefined}
```

### **WHAT'S INSIDE A PROMISE?**

```jsx
> JSON.parse(JSON.stringify(Promise.resolve(5)))
```

```jsx
Result:
```

```jsx
{}
```

### **REJECTING PROMISES**

```jsx
> Promise.reject(new Error('user does not exist'))
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
{rejected: 'Error: user does not exist'}
```

```jsx
> Promise.reject(['an', 'array'])
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
{rejected: ['an', 'array']}
```

### **PROMISE CONSTRUCTOR**

```jsx
> new Promise(resolve => resolve(5)) .then(n => n * 2)
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
{fulfilled: 10}
```

### **PROMISE CONSTRUCTOR IS SYNCHRONOUS**

```jsx
> const array = []
array.push('before')
new Promise(resolve => { array.push('constructed')
})
array.push('after')
array
```

```jsx
Result:
```

```jsx
['before', 'constructed', 'after']
```

### **PROMISES RESOLVING TO PROMISES**

```jsx
> const promise1 = Promise.resolve(5)
const promise2 = Promise.resolve(promise1)
promise2
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
{fulfilled: 5}
```

```jsx
> const promise1 = Promise.reject(new Error('404 not found'))
const promise2 = Promise.resolve(promise1)
promise2
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
{rejected: 'Error: 404 not found'}
```

### **SAVING THE RESOLVE FUNCTION FOR LATER**

```jsx
> let resolve
const promise = new Promise(r => { resolve = r })
resolve(3)
promise
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
{fulfilled: 3}
```

### **CATCHING PROMISE REJECTIONS**

```jsx
> Promise.resolve() .then(() => { throw new Error('this will reject!') }) .catch(() => 'we caught it')
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
{fulfilled: 'we caught it'}
```

### **RUNNING PROMISES CONCURRENTLY**

```jsx
> function getUser(id) { const users = [ {id: 1, name: 'Amir'}, {id: 2, name: 'Betty'}, {id: 3, name: 'Cindy'}, ] const user = users.find(user => user.id === id) return new Promise(resolve => resolve(user))
}
Promise.all([ getUser(2), getUser(3),
]).then(users => users.map(user => user.name))
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
{fulfilled: ['Betty', 'Cindy']}
```

### **CLEARTIMEOUT**

```jsx
> const array = []
const timer1 = setTimeout(() => { array.push("timer 1") }, 1000)
const timer2 = setTimeout(() => { array.push("timer 2") }, 1000)
clearTimeout(timer2)
array
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
['timer 1']
```

### **RECOVERING FROM REJECTION**

```jsx
> Promise.resolve() .then(() => { throw new Error('this will reject!') }) .catch(() => 'oh no') .then(theString => theString.length)
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
{fulfilled: 5}
```

### **PROMISE.ALL**

```jsx
> Promise.all([ Promise.resolve(1), new Promise(resolve => resolve(2)), Promise.resolve(2).then(n => n + 1), new Promise(resolve => setTimeout(() => resolve(4), 500)),
])
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
{fulfilled: [1, 2, 3, 4]}
```

### **PROMISE STATES**

```jsx
> const stateHistory = []
let state = 'pending'
let resolve
// Create a promise and store its resolve function.
new Promise(r => { resolve = r })
.then(() => { state = 'fulfilled'
})
// Resolve the promise after 1.5 seconds
setTimeout(resolve, 1500)
// Check the promise's state once per second.
setTimeout(() => { stateHistory.push(state) }, 0)
setTimeout(() => { stateHistory.push(state) }, 1000)
setTimeout(() => { stateHistory.push(state) }, 2000)
stateHistory
```

```jsx
Result  after waiting up to 3000 ms:
```

```jsx
['pending', 'pending', 'fulfilled']
```