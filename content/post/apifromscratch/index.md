+++
title = "Build an API from scratch"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++

## Steps

1. create an empty folder for our Web API, feel free to name it anything you’d like.
2. **CD into the folder** you just created and type `npm init -y` to generate a default `package.json` file. The `-y` flag saves time by answering `yes` to all the questions that the `npm init` command would ask one at a time.
3. open the folder in your favorite text editor.
4. inside the `package.json` file, change `"test": "echo \"Error: no test specified\" && exit 1"` inside the `scripts` object to read: `"start": "nodemon index.js"`. This will let us run our server using `nodemon` by typing `npm start` at the command line/terminal. **Make sure to save the file.**
5. we need to install `nodemon` as a development time dependency only because it is not needed when we deploy our server to production. Type `npm install -D nodemon` and that will add it to the `devDependencies` property in our `package.json` file.
6. create a file to host the server code, we’ll call it `index.js`.
7. add the express npm module: `npm install express`.
8. add the basic code to create our Express server and have a default `/` endpoint we can use to test that our server is responding to requests.

    ```jsx
    const express = require('express');

    const server = express();

    server.use('/', (req, res) => res.send('API up and running!'));

    // using port 9000 for this example
    server.listen(9000, () => console.log('API running on port 9000'));
    ```

9. start the server by typing `npm start`.
10. test it by visiting: [http://localhost:9000](http://localhost:9000/)0 in your browser.