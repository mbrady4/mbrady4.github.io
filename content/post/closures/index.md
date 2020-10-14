+++
title = "Closures"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
Closure basically refers to the statement "What data do I currently have access in my program?"

Functions in javascript create there own scope and every function is a closure in Javascript. 

When a function is declared and created, a new scope is also created. Any variables declared within that function’s scope will be enclosed in a lexical/private scope that belongs to that function. Also, it is important to remember that **functions look outward for context. If some variable isn’t defined in a function’s scope, the function will look outside the scope chain and search for a variable being referenced in the outer scope.**

```jsx
const foo = 'bar';
function returnFoo () {
  return foo;
}
returnFoo();
```

> Closures are particularly useful when you start a task and want to specify something that happens when task is done with stuff that is available to you when you begin the task.