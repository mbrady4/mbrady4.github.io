# Unit Testing

Automated testing minimizes the risk of bugs finding their way into production code. Testing is NOT optional. Testing should be a part of every developer’s workflow. A feature is not done until it is fully tested!

### **When to Test**

When trying to determine whether to manually test a feature, or to automate testing, consider the following guidelines:

- Automated testing should be built for any test that is executed often. Internally at Lambda we say “1..2.. automate!” meaning, you should only do a task 2 times manually before you are required to build out a system to automate that test or task for you. It isn’t always relevant, but a catchy motto to consider.
- Automated testing should be built for any difficult manual task - if it requires too much work, or too many steps, that even a 2nd manual test would be a large task, automate it from the start.
- Automated testing should be built for any test that, if it fails, will dramatically affect the use of a product or health of a company. For example, you’d always want to build a test to check for security of credit card information on a payment processing website.

On the flip side, if you’re trying something new, or if test requirements are changing a lot, you can probably hold off on creating an automated test.

## react-testing-library

**Dependencies**

`npm i -D jest-dom react-testing-library`

Designed with the user in mind, testing components via DOM nodes, similar to how a user would interact with the front end of a website.

```jsx
// import dependencies
import React from "react";

// Throughout the examples we assume that you import Jest DOM in create-react-app, and suggest that you always do this, rather than importing it every time.

// import react-testing methods
import { render } from "@testing-library/react";

// add greeting
import Greeting from "./Greeting";

test("renders greeting on Greeting component", async () => {
  // Arrange
  // Act
  // Assert
});
```

The render method renders a React element into a virtual DOM and returns utility functions for testing the component.

```
test("renders greeting on Greeting component", async () => {
  // Arrange
  const { getByText } = render(<Greeting />);
  // Act
  const greeting = getByText(/hello lambdalorians!/i);
  // Assert
  expect(greeting).toBeInTheDocument();
});
```

**Unit testing uses Regex:** One should notice that “hello lambdalorians” is written with / instead of "s. This is regex syntax and is commonly used in testing. the i designates our text as case insensitive so even though we have the string “hello lambdalorians” written, our test will pass even if “hElLo LamBdAlOriAns” is displayed in the browser.

```jsx
import React from "react";

import { render, fireEvent } from "@testing-library/react";
import Counter from "./Counter";

test("increments count when increment button is clicked", async () => {
  // Arrange
  const { getByText } = render(<Counter />);
  // Act
  const count = getByText(/0/i);
  // get the button node
  const button = getByText(/increment/i);
  // simulate a user click
  fireEvent.click(button);
  // Assert
  expect(count).toHaveTextContent("1"); //passes with 1 because we expect it to be 1 after a button click
  expect(count).not.toHaveTextContent("0");
});
```