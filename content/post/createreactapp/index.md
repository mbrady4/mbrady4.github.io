# Create React App

The React team built a tool called Create React App that sets up our entire app for us. It gives us all the “boilerplate,” or necessary configuration and setup code, that we need for a React app so we can start building the app, and not spend hours on configurations.

npx is a utility that was introduced in npm v 5.2.0. Here, animals is the name of the React project you want to build:

`npx create-react-app animals --use-npm`

CRA gets to work installing all necessary files, folders, and dependencies using npm. It will even initialize a git repository and perform an initial commit. Once the process completes (it might take a while!) you should see a Happy hacking! success message in your terminal.

## Starting the React App

```jsx
// cd into the project folder
cd animals

// start up the project
npm start
```

Other commands you can run include:

- `npm test` will launch tests with the testing library `jest`.
- `npm run build` will launch the bundler and transcompiler to produce a minified bundle appropriate for deployment.
- `npm run eject` will unhide all the configuration and dependencies that CRA-generated react apps use under the hood. No way back from this, so don’t do it unless you are sure you need to! Ejecting is usually NOT necessary for toy apps or small prototypes, but often inevitable when building real-world apps.

[Create a New React App - React](https://reactjs.org/docs/create-a-new-react-app.html#create-react-app)

## Handling Dependencies

We can use npm to install any other JavaScript packages as dependencies besides `react` and `react-dom` (which you’ll find already among the project’s dependencies inside `package.json`).

Example of installing and uninstalling the `moment` library as a project dependency:

`npm install moment
npm uninstall moment`

Example of installing and uninstalling the `chai` library as a **dev** project dependency:

`npm install -D chai
npm uninstall chai`

**Dev** dependencies are packages you as a developer need for performing specific operations like spinning up the app in your laptop, creating a bundle, or running tests. **Non-dev** or “regular” dependencies are the packages imported and consumed directly by your application code.

## File Structure

![Create%20React%20App%202d5423c712d54448976ae40968375e70/Untitled.png](Create%20React%20App%202d5423c712d54448976ae40968375e70/Untitled.png)

The main `index.js` file in the `src`, which is mounting our `App` component to the `root div` found in the `index.html` file which lives in `public`. Notice how we’re importing in `App.js` from inside of `index.js`.

Inside of `src/App.js`, we can see a class component. This approach is helpful because, in a typical React app, this main `App.js` component is used to hold onto state and sort of acts as the data control center for the rest of our application. Of course, you can build out many other components but `app`’s main purpose is to be the one single component that we pass to `ReactDOM.render` to compose our application.

## ReactStrap

`npm install reactstrap bootstrap`

Notice that we’re installing BOTH Reactstrap and Bootstrap here. Reactstrap is the component library that uses Bootstrap under the hood. So you’ll need both. To get the stylesheet included to your React Application, you can either use the script tag option or import the bootstrap library into your `index.js` file.

`import 'bootstrap/dist/css/bootstrap.min.css';`

## Styled Components

To get started with styled-components we run `npm install styled-components`.

```jsx
// div
const StyledDiv = styled.div``;

// paragraph
const StyledP = styled.p``;

// section
const StyledSection = styled.section``;

// headers h1 - h6
const StyledHeading = styled.h1``;

// a
const StyledA = styled.a``;

// etc.....
```

### Syntax for Styled Component

```jsx
// Button.js

import React from "react";
import styled from "styled-components";

const Button = styled.button`
  padding: 6px 10px; 
  margin: 5px;
  border: none;
  border-radius: 3px;
  color: white; 

  ${props => (props.type === "primary" ? `background: #2196f3` : null)}
  ${props => (props.type === "success" ? `background: #4caf50` : null)}
  ${props => (props.type === "warning" ? `background: #fdd835` : null)}
  ${props => (props.type === "danger" ? `background: #fdd835` : null)}
`;

export default Button;

// index.js
import React from "react";
import ReactDOM from "react-dom";
import styled from "styled-components";
import Button from "./Button";

const WrapperDiv = styled.div`
  font-family: sans-serif;
  text-align: center;
`;

const BlueH1 = styled.h1`
  color: blue;
`;

function App() {
  return (
    <WrapperDiv>
      <BlueH1>Styled Componenets</BlueH1>
      <Button type="primary">Primary</Button>
      <Button type="success">Success</Button>
      <Button type="danger">Danger</Button>
      <Button type="warning">Warning</Button>
    </WrapperDiv>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

## Overriding styles with child componenets

```jsx
import React from 'react';
import styled from 'styled-components';

const Button = styled.button`
  color: palevioletred;
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid palevioletred;
  border-radius: 3px;
`;

// A new component based on Button, but with some override styles
const TomatoButton = styled(Button)`
  color: tomato;
  border-color: tomato;
`;

function SomeComponent() {
  return (
  <div>
    <Button>Normal Button</Button>
    <TomatoButton>Tomato Button</TomatoButton>
  </div>
);

export default SomeComponent;
```

![Create%20React%20App%202d5423c712d54448976ae40968375e70/Untitled%201.png](Create%20React%20App%202d5423c712d54448976ae40968375e70/Untitled%201.png)