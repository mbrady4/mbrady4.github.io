+++
title = "Express"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
Express is a javascript framework that sits on top of the NodeJS Web Server. It's like React for your backend. 

`npm install express`

Express sits on top of the raw http module provided by Node. Adds extra functionality like Routing, Middleware Support and a more eloquent API. Light, un-opinionated and makes your life easier. What you had to do with massive amounts of code, becomes trivial. 

With Express you can:

- Serve single page applications
- **Build RESTful web services that work with JSON**
- Serve static content like HTML files, images, audio files, PDFs, and more
- Power real-time applications using technologies like Web Sockets and WebRTC

## **Core Features**

**Middleware:** 

- Functions that get the `request` and `response,` can perform operations on them, and can either move into the next middleware, or return a response back to the client.
    - E.g., logging requests and security through authentication
- Express middleware stack is essentially an array of functions.
- Middleware can change the request of response but doesn't have to.

**Express Routing**

- A way to select which request handler function is executed based on the URL HTTP method was used.
    - Helps break the application into smaller parts
    - Applications broken up in terms of routers. A single Router could serve up our SPA and another four our API.
    - Each Router can have it's own middleware and routing.

**Convenience Helpers**

- Provide functionality out of the box. Extension methods added to the request and response objects. Examples: `response.redirect(` `response.status()` `response.send()` `request.ip()`

**Views**

- Provide a way to dynamically render HTML on the server and even generate it using other languages. [Not the popular way to do things currently]

**Example**

```jsx
const express = require('express'); // import the express package

const server = express(); // creates the server

// handle requests to the root of the api, the / route
server.get('/', (req, res) => {
  res.send('Hello from Express');
});

// watch for connections on port 5000
server.listen(5000, () =>
  console.log('Server running on http://localhost:5000')
);
```