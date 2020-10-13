# Promises

In JavaScript we have the concept of ‘asynchronous’ code. This simply means code that does not run instantly in line. Perhaps the code needs to wait a moment, wait for something to happen, or in the case we will explore today, wait until data comes back from a server.

Asynchronous codes makes it possible for a Javascript engine to do 2 things at the same time. We can use asynchronous code to allow the browser to keep executing code while something else is happening - usually a call to an external API for data

When we use this technique, we create a helper object, called a "promise", to inform the browser that the second, async task has finished.  

A promise is an object with a few properties:

- When we want to run some async code, we create a new Promise, and use that Promise to inform the JavaScript engine that the async function has finished
- When we instantiate a new Promise with the "new" keyword, we pass in a callback function that recieves a "resolve" function and a "reject" function
- If the async function finishes and was successful, we call the resolve() function. If it was unsuccessful, we call the reject() function.
- When a Promise is resolved or rejected we use the promise object's methods `.then()` or `.catch()` to tell the Javascript engine what to do next

Simply put, a Promise is just that, a promise from the object that it will let us know when it has completed (or errored) what we have asked it to do. A promise can exist in one of three states:

- `Pending`: a state where the promise is neither rejected nor fulfilled. (this is the state it is in when we first call it)
- `Fulfilled`: a state where *all’s well* and a resolved value can be used by our code.
- `Rejected`: a state where *something went wrong* and there is an error that needs to be dealt with.

If the promise succeeds, it will return the value as a parameter into a callback passed into .then(). If the promise fails, the callback passed into the .catch() runs, taking an error as its argument.

`.then()` and `.catch()` 

### Example

```jsx
let time = 0;
const timeMachine = () => {
return new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve((time += 1000));
  }, 1000);
});
};

timeMachine()
  .then(newTime => {
    const myTime = newTime / 1000;
    return `${myTime} seconds have passed`;
  })
  .then(newString => {
    console.log(newString); --> OUTPUTS ​​​​​"1 seconds have passed​​​​​"
  })
	.catch(err => {
    console.log(err); --> OUTPUT: ​​​​​[Error: ms is less than 1 second promise rejected!]​​​​​
  });
```

## HTTP

HTTP is a network protocol, a set of rules that govern the way web clients, like a browser, communicate with web servers over the internet. 

`HTTP Methods` provide a common *language* or nomenclature that the client can use to let the server know what operation it wants to perform.

- When a client needs to ask a server for information it should do a GET request specifying a URL that points to the desired resource.
- A POST request is used to ask the server to add or *create* new resources.
- The method used by the client to ask the server to make changes to specific resources is PUT.
- To remove or delete data from the server the client needs to send a DELETE request.

`HTTP Status Codes` are used to indicate if a *request* has been successful or not and why. The server will set the status code for all responses sent to the client.

## axios

axios is a Javascript library. It is used to send HTTP requests to servers. It is not necessary to do this, but it makes things much easier. Because all server requests are asynchronous, axios uses Promises.

To import axios:

`<script src="[https://unpkg.com/axios/dist/axios.min.js](https://unpkg.com/axios/dist/axios.min.js)"></script>`

axios is an object containing many methods, .get being one of them. It takes a string as its first argument. This string is the url of the resource we are requesting:

```jsx
axios.get(url)
```

axios.get will return a Promise to us. This tells us that it is busy getting the data and will return in a moment. As with all promises, we will use .then and .catch to deal with the data.

```jsx
axios.get('http://fakeserver.com/data')
    .then( response => {
        // Remember response is an object, response.data is an array.
        response.data.forEach( item => {
            let button = buttonCreator(item);
            parent.appendChild(button);
        })
    })
    .catch( error => {
        console.log("Error:", err);
    })
```

![Promises%206846ae342682419689c9d78deb5deb19/Untitled.png](Promises%206846ae342682419689c9d78deb5deb19/Untitled.png)

![Promises%206846ae342682419689c9d78deb5deb19/Untitled%201.png](Promises%206846ae342682419689c9d78deb5deb19/Untitled%201.png)

[https://codepen.io/emseibert/pen/eYYyJvy](https://codepen.io/emseibert/pen/eYYyJvy)