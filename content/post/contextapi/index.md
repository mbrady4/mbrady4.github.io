# Context API

> In a typical React application, data is passed top-down (parent to child) via props, but this can be cumbersome for certain types of props (e.g. locale preference, UI theme) that are required by many components within an application. Context provides a way to share values like these between components without having to explicitly pass a prop through every level of the tree.
We use the Context API when we have global data that lots of components share (things like user or theme), or when we have to pass data through intermediate components. The API can help keep your state relatively clean.

One caveat here is that it can make components harder to reuse. Another architectural pattern you may want to look at is component composition.

The Context API allows us to create what's known as a Context Object. This object gives us two important components to work with a `ContextObject.Provider` and a `ContextObject.Consumer`.

```jsx
import { createContext } from 'react';

const ContextObject = createContext();

// usually we'll name the object by the data it will hold - ie UserContext, or MoviesContext, etc...
```

The Provider method accepts a single prop called value, the value prop is used to provide our data across our app.

```jsx
<ContextObject.Provider value={dataToPassDown}>
  <NestedComponent />
  <OtherNestedComponent />
</ContextObject.Provider>
```

The context Consumer consumes and returns the value provided by the Provider.

```jsx
import React, { useContext } from "react";
import { UserContext } from "../contexts/UserContext";

function User() {
	const user = useContext(UserContext);

	return (
		<div className="profile">
			{user.lastName}, {user.firstName}
		</div>
	);
}

export default User;
```

[Context API Solution](https://codesandbox.io/s/context-api-solution-2y0cu?file=/src/index.js:148-459)

[Context API - Starter](https://codesandbox.io/s/context-api-starter-2kzpj)