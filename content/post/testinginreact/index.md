# Testing in React

A function in testing may have inconvenient dependencies on other objects. To isolate the behavior of the function, it’s often desirable to replace the other objects with mocks that simulate the behavior of the real objects. Replacing objects is especially useful if the real objects are impractical to incorporate into the unit test. Simpler mocks that implement only enough behavior to execute test code are sometimes called “stubs”.

For example, we can stub out (create) a fake version of a helper function called `uuid` that will replace the real one during the execution of the test. Outside of the test block, at the top level of the test file, place the following code:

```jsx
jest.mock("uuid", () => () => "abcde");
```

As the first argument to jest.mock(), we pass the path to the module we want to replace. As the second argument, we pass a callback that returns whatever it is we want the faked thing to be. We wish for uuid to become a silly stub function that always returns the same string: uuid() // "abcde".

```jsx
//import libraries
import React from "react";
import { render, fireEvent, waitFor } from "@testing-library/react";
import { fetchDoggos as mockFetchDoggos } from "../api/fetchDoggos";
import Doggos from "./Doggos";

//set up test
jest.mock("../api/fetchDoggos");

test("renders dog images from API", async () => {
  mockFetchDoggos.mockResolvedValueOnce({
    message: [
      "https://images.dog.ceo/breeds/hound-afghan/n02088094_1003.jpg",
      "https://images.dog.ceo/breeds/hound-afghan/n02088094_1007.jpg",
      "https://images.dog.ceo/breeds/hound-afghan/n02088094_1023.jpg"
    ]
  });

  const { getByText, getAllByTestId } = render(<Doggos />);

  const fetchDoggosButton = getByText(/fetch doggos/i);
  fireEvent.click(fetchDoggosButton);

  // add new assertion
  expect(mockFetchDoggos).toHaveBeenCalledTimes(1);

  await waitFor(() => expect(getAllByTestId(/doggo-images/i)).toHaveLength(3));
});
```

## What to test

![Testing%20in%20React%20ff42e91eb85841ac954bf7434c8cc392/Untitled.png](Testing%20in%20React%20ff42e91eb85841ac954bf7434c8cc392/Untitled.png)

**6. Repeat**

![Testing%20in%20React%20ff42e91eb85841ac954bf7434c8cc392/Untitled%201.png](Testing%20in%20React%20ff42e91eb85841ac954bf7434c8cc392/Untitled%201.png)

[Which Query should you use?](https://www.notion.so/c2e9a971e04c41b1921c06dd5835c7f6)

Throw means it function will throw an error. 

[JSON to JavaScript object literal | Online Tool](https://json-to-js.com/)