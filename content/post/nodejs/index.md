+++
title = "Node.js Basics"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
Node.js is javascript outside of the browser.

Javascript used to be only available within browsers. In 2009, developers were given a choice to use Node outside of the browser. One common pattern is to use Node in the context of building Web Servers. 

We will be using JSON (JavaScript Object Notation) to send information back and forth between applications.

Advantages of Node: 

- **JavaScript on the server:** use the same language and paradigm for both Client (React Applications) and Server. No more context switching.
- **Singled threaded:** removes the complexity of managing multiple threads
- **Asynchronous:** Can take full advantage of the processor its running on. The node process runs on a single CPU.
- **NPM registry:** access to the largest ecosystem of useful libraries in the form of npm modules

Disadvantages:

- **Javascript on the server:** We lose the ability to use the right language for the job
- **Single threaded:** Cannot take advantage of servers with multiple threads
- **Asynchronous:** It is harder to learn for developers that have only worked with languages that default to synchronous operations that block the execution thread
- **NPM library:** Too many packages that do the same thing makes it harder to choose one, and in some cases, may introduce vulnerabilities into our code.

To write a simple (for demo purposes only) web server with `Node.js`:

1. Use Node’s `HTTP` module to abstract away complex network-related operations.
2. Write the single ***request handler*** function that will handle all requests to the server.

The request handler is a function that takes the `request` coming from the client and produces the `response`. The function takes two arguments: 1) an object representing the `request` and 2) an object representing the `response`.

```jsx
const http = require('http'); // built in node.js module to handle http traffic

const hostname = '127.0.0.1'; // the local computer where the server is running
const port = 3000; // a port we'll use to watch for traffic

const server = http.createServer((req, res) => {
  // creates our server
  res.statusCode = 200; // http status code returned to the client
  res.setHeader('Content-Type', 'text/plain'); // inform the client that we'll be returning text
  res.end('Hello World from Node\n'); // end the request and send a response with the specified message
});

server.listen(port, hostname, () => {
  // start watching for connections on the port specified
  console.log(`Server running at http://${hostname}:${port}/`);
});
```