# Express Routers

Express Routers are a way to split our application into sub-applications to make it modular and easier to maintain/reason about.

An Express Router behaves like a mini Express application. It can have it’s own Routing and Middleware, but it needs to exist inside of an Express application. Think of routers as organizing Express applications because you write separate pieces that can later be composed together.

Our main file could look like this: 

```jsx
const express = require('express');

const userRoutes = require('./users/userRoutes');
const productRoutes = require('./products/productRoutes');
const clientRoutes = require('./clients/clientRoutes');

const server = express();

server.use('/users', userRoutes);
server.use('/products', productRoutes);
server.use('/clients', clientRoutes);

server.listen(8000, () => console.log('API running on port 8000'));
```

And the userRoutes file could look like this: 

```jsx
/ inside /users/userRoutes.js <- this can be place anywhere and called anything
const express = require('express');

const router = express.Router(); // notice the Uppercase R

// this file will only be used when the route begins with "/users"
// so we can remove that from the URLs, so "/users" becomes simply "/"
router.get('/', (req, res) => {
  res.status(200).send('hello from the GET /users endpoint');
});

router.get('/:id', (req, res) => {
  res.status(200).send('hello from the GET /users/:id endpoint');
});

router.post('/', (req, res) => {
  res.status(200).send('hello from the POST /users endpoint');
});

// .. and any other endpoint related to the user's resource

// after the route has been fully configured, then we export it so it can be required where needed
module.exports = router; // standard convention dictates that this is the last line on the file
```

![Express%20Routers%20f1ebf8cb54ca43ebaa2bff32c9aef3ef/Untitled.png](Express%20Routers%20f1ebf8cb54ca43ebaa2bff32c9aef3ef/Untitled.png)