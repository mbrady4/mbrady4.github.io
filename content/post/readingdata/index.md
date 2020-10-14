+++
title = "Reading Data"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++

There are three ways to send data to an existing endpoint:

## 1. `request body` objects

Objects that clients can send with relevant info for the API they're requesting. Pertinent for POST and PUT requests. Whenever you create info like `username` and `password` you send that info on the the body of the request.

```jsx
// add this code right after const server = express();
server.use(express.json());

let hobbits = [
  {
    id: 1,
    name: "Bilbo Baggins",
    age: 111,
  },
  {
    id: 2,
    name: "Frodo Baggins",
    age: 33,
  },
];
let nextId = 3;

// and modify the post endpoint like so:
server.post("/hobbits", (req, res) => {
  const hobbit = req.body;
  hobbit.id = nextId++;

  hobbits.push(hobbit);

  res.status(201).json(hobbits);
});
```

To read data from body:

- Add the line: `server.use(express.json());` after the express application has been created.
- Read the data from the body property that Express adds to the request object. Express takes all the information that the client added to the body and makes it available as a nice JavaScript object.

## 2. Key/value pairs inside the `query string`

We send information on the URL we're requesting. Structured as key/value pairs on the URL string we send to the server. Each pair takes the form of `?key=value` and pairs are separated by an `&`

We can read query strings by looking at the `req.query` property:

```jsx
server.get("/hobbits", (req, res) => {
  // query string parameters get added to req.query
  const sortField = req.query.sortby || "id";
  const hobbits = [
    {
      id: 1,
      name: "Samwise Gamgee",
    },
    {
      id: 2,
      name: "Frodo Baggins",
    },
  ];

  // apply the sorting
  const response = hobbits.sort((a, b) =>
    a[sortField] < b[sortField] ? -1 : 1
  );

  res.status(200).json(response);
});
```

The value of the parameter will be of type array if more than one value is passed for the same key and string when only one value is passed. This means that in the query string ?id=123, [req.query.id](http://req.query.id/) will be a string, but for ?id=123&id=234 it will be an array.

Query string parameters are case sensitive

## 3. Dynamic `route parameters`

We define route parameters by adding it to the URL with a colon (:) in front of it. Express adds it to the .params property part of the request object. The value for a route parameter will always be string, even if the value passed is numeric.

```jsx
server.delete('/hobbits/:id', (req, res) => {
	// `:id` will be dynamically set by whatever comes after `hobbits/`
	const id = req.params.id
}
```

Express routing has support for multiple route parameters. For example, defining a route URL that reads /hobbits/:id/friends/:friendId, will add properties for id and friendId to req.params.
