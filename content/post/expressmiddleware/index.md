+++
title = "Express Middleware"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
Middleware means functions that extend software. Used to add features to express. Most code we write, including route handlers, is middleware.

Can be thought of as an array of functions that get executed in the order they are introduced into the server code.

Three types of middleware: 

- **Built-in:** Included in express, but not added to the application automatically
- **Third Party Middleware.** NPM modules we can install and then import using `require()`
    - Morgan
    - CORS — enables cross site communication
    - Helmet — adds a layer of security
- **Custom Middleware:** functions that we write to perform certain tasks

Express middleware is compatible with connect middleware. Connect is a web application framework for Node.js that only provides the middleware layer.

All types of middleware work in the same way. We tell Express about the middleware we want to turn on for our application by making a call to .use() on our server and passing .use() the piece of middleware we want to apply. This line must come after the server has been created by calling express().

## Error handling middleware

We handle errors to tell our users they aren't working with our software properly, or to report a problem so that they know that something is broken or taking a while.

When our application encounters an error while executing middleware code, we can choose to hand over control to error handling middleware by calling next(); with a single argument. 

The convention for that single argument will be an error object like this one: 
`next(new Error("error message"));`

This type of middleware takes four arguments: error, req, res, and next. We pass the first argument when calling next(new Error('error message here')). When the error handling code is finished, we can choose to end the request or call next without arguments to continue to the next regular middleware.

Error handling middleware CAN be placed anywhere in the stack, but it makes the most sense to place it at the end. If the intention is for middleware to handle errors that may occur elsewhere in the queue, then it needs to run after the rest of the middleware has run.

The error middleware will only get called if any other middleware or route handler that comes before it has called next() with an argument that contains an error argument.

```jsx
const express = require('express');
const path = require('path');

const server = express();

server.get('/download', (req, res, next) => {
  const filePath = path.join(__dirname, 'index.html');
  res.sendFile(filePath, err => {
    // if there is an error the callback function will get an error as it's first argument
    if (err) {
      // we could handle the error here or pass it down to error-handling middleware like so:
      next(err); // call the next error-handling middleware in the queue
    } else {
      console.log('File sent successfully');
    }
  });
});

server.use((err, req, res, next) => {
	// error handling middleware, takes in ERROR
  console.error(err);

  res
    .status(500)
    .json({ message: 'There was an error performing the required operation',
						error: err });
});

server.listen(5000);
```

## Custom Middleware

**Example Global logger middleware:**

```jsx
function logger(req, res, next) {
  console.log(
    `[${new Date().toISOString()}] ${req.method} to ${req.url} from ${req.get(
      'Origin'
    )}`
  );

  next();
}

// Apply middleware at the global level (apply to every endpoint)
server.use(logger);
```

next() is a function that tells express your middleware is ready to move on into the next middleware in the queue. Calling next() signals to Express that the middleware has finished, and it should call the next middleware function. If next() is not called and a response is not sent back to the client, the request will hang, and clients will get a timeout error, so make sure to always call next() or use one of the methods that send a response back like res.send() or res.json().

Middleware can modify the req/res objects and can also stops requests by sending responses back. 

**Example Specific Middleware:**

```jsx
function auth(req, res, next) {
  if (req.url === '/mellon') {
    next();
  } else {
    res.send('You shall not pass!');
  }
}

server.get('/mellon', auth, (req, res) => {
  console.log('Gate opening...');
  console.log('Inside and safe');
  res.send('Welcome Traveler!');
});
```