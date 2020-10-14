+++
title = "Routing"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
Routing refers to how an application’s endpoints (URIs) respond to client requests. It is a way to map incoming requests to servers from clients to the appropriate request handler function. Based off of the URL and type of HTTP method used. Simplifies the architecture of your servers by keeping things organized and clean. Everything has a purpose. Routing helps us define our CRUD Operations within our Server side applications. 

You define routing using methods of the Express app object that correspond to HTTP methods; for example, app.get() to handle GET requests and app.post to handle POST requests.

These routing methods specify a callback function (sometimes called “handler functions”) called when the application receives a request to the specified route (endpoint) and HTTP method. In other words, the application “listens” for requests that match the specified route(s) and method(s), and when it detects a match, it calls the specified callback function.

Express supports methods that correspond to all HTTP request methods: get, post, and so on.

```jsx
// this request handler executes when making a GET request to /about
server.get('/about', (req, res) => {
  res.status(200).send('<h1>About Us</h1>');
});

// this request handler executes when making a GET request to /contact
server.get('/contact', (req, res) => {
  res.status(200).send('<h1>Contact Form</h1>');
});
```

There is a special routing method, app.all(), used to load middleware functions at a path for all HTTP request methods. For example, the following handler is executed for requests to the route “/secret” whether using GET, POST, PUT, DELETE, or any other HTTP request method supported in the http module.

```jsx
app.all('/secret', function (req, res, next) {
  console.log('Accessing the secret section ...')
  next() // pass control to the next handler
})
```

## Route Paths

Route paths, in combination with a request method, define the endpoints at which requests can be made. Route paths can be strings, string patterns, or regular expressions.

## Route parameters

Route parameters are named URL segments that are used to capture the values specified at their position in the URL. The captured values are populated in the `req.params` object, with the name of the route parameter specified in the path as their respective keys.

```jsx
Route path: /users/:userId/books/:bookId
Request URL: http://localhost:3000/users/34/books/8989
req.params: { "userId": "34", "bookId": "8989" }
```

Note: Since the hyphen (-) and the dot (.) are interpreted literally, they can be used along with route parameters:

```jsx
Route path: /flights/:from-:to
Request URL: http://localhost:3000/flights/LAX-SFO
req.params: { "from": "LAX", "to": "SFO" }
```

## Route Handlers

Multiple callback functions that behave like middleware can be used to handle a request. 

Route handlers can be in the form of a function, an array of functions, or combinations of both, as shown in the following examples.

You can chain route handlers together: 

```jsx
var cb0 = function (req, res, next) {
  console.log('CB0')
  next()
}

var cb1 = function (req, res, next) {
  console.log('CB1')
  next()
}

app.get('/example/d', [cb0, cb1], function (req, res, next) {
  console.log('the response will be sent by the next function ...')
  next()
}, function (req, res) {
  res.send('Hello from D!')
})
```

## Response Methods

The methods on the response object (res) can send a response to the client and terminate the request-response cycle. If none of these methods are called from a route handler, the client request will be left hanging: 

![Routing%206fb2aae1af6c46f6843c4d20fc1ebccb/Untitled.png](Routing%206fb2aae1af6c46f6843c4d20fc1ebccb/Untitled.png)

## express.Router

A router instance is a complete middleware and routing system; for this reason, it is often referred to as a "mini-app". The following example creates a router as a module, loads a middleware function in it, defines some routes, and mounts the route module on a path in the main app. 

## Handling routes requested that don't exist

Must be placed directly before the `app.listen` otherwise will override other route handlers.

```jsx
app.get('/*',function(req,res)
{
    req.send("I am Foo");
 });
```