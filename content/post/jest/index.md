+++
title = "Jest"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
### Why unit tests?

Not writing tests can be thought of as taking a loan with a very high interest rate. The quick payoff is being able to write spaghetti code unhindered. The costs long term can become enormous and surpass hundreds of times over any marginal time gains obtained by not writing tests. Eventually, entire teams of developers can get so bogged down in bugs and regressions that the product can't move forward. 

There are usually many unit tests in a codebase, and because these tests are meant to be run often, they need to run fast. Unit tests are fast, they’re simple to write and execute, and they’re the preferred tool for test driven development (TDD) and behavior driven development (BDD). They are regularly used by developers to test correctness in units of code (usually functions).

Another important consideration with testing is that you should try to avoid unnecessary preconditions. If your test relies on outside dependencies or other tests running first, you should factor to isolate the test (much like a pure function).

### Test Driven Development

Test driven development is the process of writing tests before code. In theory, when you start with the end (the tests) in mind, you can write much higher quality code.

There’s really an endless amount of assertions we could check, but for test driven development it helps to think of the most-likely scenarios where the unit could fail.

![Jest%20da03d8446678415cbd2ce5a28f589607/Untitled.png](Jest%20da03d8446678415cbd2ce5a28f589607/Untitled.png)

## What Jest is

Jest is a test runner and command line interface npm package. It was originally made by Facebook and is included out-of-the-box with create-react-app. Jest is a very general purpose testing tool, and it works best with React applications, though it works with other frameworks as well. In addition to the types of tests we’ve seen, Jest can run asynchronous tests, snapshot testing, and produce coverage reports.

### Watch Mode

Instead of running tests manually, Jest has a built-in feature called watch mode that will run tests automatically as files change. Jest detects these changes automatically and only runs the tests pertaining to the changes.

### Install Jest

1. **Install `jest` with npm.** We first need to install Jest as a developent dependency. As soon as we do, Jest dependencies will show up in our `package.json` file.

`npm install -D jest`

1. **Add test script**. In `package.json` we’ll need to indicate that we’re using jest for testing. This can be done by simply adding `"test": "jest --watch",` to your “scripts” object.
2. **Run Tests**. We can start Jest by typing `npm test` in a terminal window at the root of the project. However, since there are no tests written, it will return an error “No tests found” because we haven’t actually written any tests yet, so let’s move on.
3. **Create test files.** By convention, Jest will find your tests in two ways: 1) by placing `.js` files inside a folder called `__tests__` or 2) by ending the name of a file in `.test.js` or `.spec.js`. Technically, you could give the `__tests__` folder a different name, but then you’d need to manually change where Jest looks for test files.

### Jest Globals

- the `it` global is a method you pass a function to; that function is executed as a block of tests by the test runner.
- the `describe` is optional for grouping a number of related `it` statements; this is also known as a `test suite`.
- While we’re brainstorming we can use `it.todo()` to capture ideas for future tests and fill up the test details one test at a time later. This will indicate to indicate to our test runner that we don’t want to run that test yet, but to keep note of it instead.
- `beforeAll()` runs once before the first test
- `beforeEach()` runs before the tests, good for setup code
- `afterEach()` runs after the tests, good for clean up
- `afterAll()` runs once after the last test
- `it.skip()` skips the test
- `it.only()` isolates a test

### Basic test

```jsx
import { hello } from "./App";
//arrange
describe("hello", () => {
  //act
  it("should output hello world!", () => {
    //assert
    expect(hello()).toBe("hello world!");
  });
});
```

### Example of a comprehensive test

```jsx
// name function based on test
const removeSalaries = salaries => {
  //create an empty array
  const higherSalaries = [];
  //use conditional logic to add salaries to the new array
  for (x in salaries) {
    if (salaries[x] < 50000) {
      higherSalaries.append(salaries[x]);
    }
  }
  // return new array
  return higherSalaries;
};

describe('removeSalaries', () => {
    it('should return an array of shorter length', () => {
        const salaries = [50000, 45000, 60000];
        expect(removeSalaries(salaries).toHaveLength(1);
    });
    it('should return numbers', () => {
        expect(removeSalaries(salaries).toContainType)
    });
    it('should remove all salaries less than 50,000', () => {
        const salaries = [50000, 45000, 60000];
        const expected = [60000];
        expect(removeSalaries(salaries).arrayContaining(expected);
    });
    it('should allow for type coercion', () => {
        const salaries = ["50000", "45000", "60000"];
        const expected = [60000];
        expect(removeSalaries(salaries).arrayContaining(expected);
    });
    it('should not remove 50,000', () => {
        const salaries = [50000, 60000]
        expect(removeSalaries(salaries).toContain([50000]))
    });
})
```