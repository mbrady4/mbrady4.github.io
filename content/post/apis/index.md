+++
title = "APIs"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
An **Application Programming Interface** (API) is server software that publishes a set of endpoints that clients can use to manage resources. 

An **endpoint** is a touchpoint that makes communication between two systems possible. For Web APIs, an endpoint is a URL (Uniform Resource Locator) that points to the location of a resource available on the server. 

**Resources** are the things our application cares about. The nouns in the Application domain (e.g., users, products, orders, clients, returns). 

**Simple API**

```jsx
// require the express npm module, needs to be added to the project using "npm install express"
const express = require('express');

// creates an express application using the express module
const server = express();

// configures our server to execute a function for every GET request to "/"
// the second argument passed to the .get() method is the "Route Handler Function"
// the route handler function will run on every GET request to "/"
server.get('/', (req, res) => {
  // express will pass the request and response objects to this function
  // the .send() on the response object can be used to send a response to the client
  res.send('Hello World');
});

server.get('/hobbits', (req, res) => {
  const hobbits = [
    {
      id: 1,
      name: 'Samwise Gamgee',
    },
    {
      id: 2,
      name: 'Frodo Baggins',
    },
  ];

  res.status(200).json(hobbits);
});

// once the server is fully configured we can have it "listen" for connections on a particular "port"
// the callback function passed as the second argument will run once when the server starts
server.listen(8000, () => console.log('API running on port 8000'));
```

To start the server use: `npm run server`

[HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)