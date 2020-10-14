+++
title = "React-Router"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
React router is a declarative style routing for React applications. Routes are a way of getting to a destination. A route can specify which components to render on the page, and in what order, as we've seen before. 

You install React Router with: 

`npm install react-router-dom`

and import with 

`import { Route } from 'react-router-dom';`

The Route component declares what components will be mounted based on what URL's the user requests. The best part about this process is that we get a chance to do this in a very “React” way. Lets picture a component Users that will display a list of users in your system when the URL [www.coolestapp.com/users](http://www.coolestapp.com/users) is requested. The Route component takes in a few props; the first is the URL path where the Route component will trigger. Next is the component prop. This is the component that you want React to mount when the URL matches the requested path. So in our case, when /users is requested, the Users component will be mounted.

`<Route path="/users" component={Users} />`

By placing `exact` on a <Route /> component, you are saying that the specific path will only trigger if it matches exactly the path requested. This defaults to false, so by simply including the exact prop on your Route component, it will set it to true and only mount our Home component when the specific path / is requested

`<Route exact path="/" component={Home}/>`

**Passing props with a component to Route**

Instead of using component, use the render prop. render accepts a functional component and that function won’t get unnecessarily remounted like with component. That function will also receive all the same props that component would receive. So you can take those and pass those along to the rendered component.

```jsx
<Route
  path='/dashboard'
  render={(props) => <Dashboard {...props} isAuthed={true} />}
/>
```

## Link

The link component can be included just like any other component in your application. This will produce an anchor tag that will link to a pre-defined component of your choice. Remember, if we set up our routes properly, we’ll be able to use the anchor component to navigate our browsers to the path where a component can be mounted. We can think of our Route component as the boat on the ocean, and the Link as the wind and sails that make that boat move.

```jsx
import { Link } from 'react-router-dom'

<Link to="/about">About</Link>
```

## useParam

The `useParam` hook relies on props to pass new and changing data into the app. Parameters are placeholders in a URL that represent some changing data. The `useParam` hook allows us to create dynamic routes that will render content based on the URL. So, instead of requiring that all routes are written out ahead of time, the URL determines what renders on the page.

In order to use a parameter in routing we need to assign the route with a colon in App.js or wherever else the routes are defined. So, `<Route path=”/employee becomes <Route path=”/:employee. With that simple change we can use the useParam` hook to create dynamic routes.

example: `<Route path='/:handle' component={Profile} />`

[React Router Avengers Example](https://codesandbox.io/s/react-router-avengers-example-87ejl)

## Dynamics Route Example (with params.match)

```jsx
import React, { useState, useEffect } from "react";
import ReactDOM from "react-dom";
import { BrowserRouter, Route, Link } from 'react-router-dom';
import "./styles.css";

const users = [
  { id: 1, name: 'Emily'},
  { id: 2, name: 'Anna'},
  { id: 3, name: 'Rory'},
];

const fetchUser = id => Promise.resolve(
  users.find(user => user.id === id)
);

const User = props => {
  const { id } = props.match.params;
  const [user, setUser] = useState(null);

  useEffect( () => {
    fetchUser(id).then(user => setUser(user));
  }, [id])

  if (!user) return <div>Fetching User...</div>
  return <div>The id is {user.name}</div>
}

export default function App() {
  return (
    <div className="App">
      <BrowserRouter>
        <Link to='/users/1'>User 1</Link>
        <Link to='/users/2'>User 2</Link>
        <Link to='/users/3'>User 3</Link>

        <Route path='/users/:id' component={User} />
      </BrowserRouter>
    </div>
  );
}
```

```jsx
App => 
	# State to hold countries returned by API
	const [countries, setCountries] = useState()
	
	# State to hold the active countries based on User Filtering	
	const [activeCountries, setActiveCountries] = useState()

	# Axios 
	useEffect(() => {
		// Will return array of country objects
    axios
			// Test performance; Request only required fields
      .get("https://restcountries.eu/rest/v2/all")
      .then(res => setCountries(res.data.message))
      .catch(err => console.log(err));
  }, []);
	
	# Region dropdown logic and state 

	# Search (input form) logic and state 

	# Filtering based on dropdown and search stored here

	return (
		<Rounter>
			<NavBar> *Possibly pass colorTheme state?* </NavBar>
			<Switch>
				<Route Path /countries/:country">
					# Pass the country code; possibly? 
					<CountryDetail key={country} countries={activeCountries} />
				</Route> 
				<Route path='/'>
					<Filters />
					<CountryList countries={activeCountries} />
				</Route>
			</Switch>
		</Router>
);

**Child Components** 
**- CountryDetail:** Render a page dedicated to a given country. 
Recieves the country code in the URL
**- CountryList:** Render a list of all countries. Recieves an array of objects
**- Filters:** Accepts user input to filter countries shown. Recieve the 
formValues and callback function. 
```