# Testing Endpoints

When building a Web API using Express, we use unit testing to test the application logic and integration testing to test the route handlers and middleware.

We write tests to verify that the API endpoints return the expected values and HTTP status codes. We also write tests to make sure that we’re returning the data using the right format (HTML, XML, JSON) and several other things.

We can use supertest to load an instance of our server, send requests to the different endpoints, and make assertions about the responses. A supertest may look like this: 

1. Save a reference to the server
2. Use `supertest` to make a POST request passing correct data inside the body
3. Check that the server responds with status code 201

Writing such a test verifies that all middleware, including the route handler is working as intended.

**Example Test**

In the below example, three common endpoint tests are shown:

- Does it return the correct status code for the input provided?
- Does it return the data in the expected format?
- Does the data returned, if any, have the right content?

```jsx
/*
- when making a GET request to the `/` endpoint 
  the API should respond with status code 200 
  and the following JSON object: `{ api: 'running' }`.
*/
const request = require('supertest'); // calling it "request" is a common practice

const server = require('./server.js'); // this is our first red, file doesn't exist yet

describe('server.js', () => {
  // http calls made with supertest return promises, we can use async/await if desired
  describe('index route', () => {
    it('should return an OK status code from the index route', async () => {
      const expectedStatusCode = 200;

      // do a get request to our api (server.js) and inspect the response
      const response = await request(server).get('/');

      expect(response.status).toEqual(expectedStatusCode);

      // same test using promise .then() instead of async/await
      // let response;
      // return request(server).get('/').then(res => {
      //   response = res;

      //   expect(response.status).toEqual(expectedStatusCode);
      // })
    });

    it('should return a JSON object from the index route', async () => {
      const expectedBody = { api: 'running' };

      const response = await request(server).get('/');

      expect(response.body).toEqual(expectedBody);
    });

    it('should return a JSON object from the index route', async () => {
      const response = await request(server).get('/');

      expect(response.type).toEqual('application/json');
    });
  });
});
```