# Basics

React is a declarative, efficient, and flexible Javascript library for building user interfaces. It lets you compose complex UIs from small and isolated pieces of code called "components". 

We use components to tell React what we want to see on the screen. When our data changes, React will efficiently update and re-render our components. 

A component takes in parameters, called `props` (short for "properties") and returns a heirarchy of views to display view the `render` method. 

Here is a basic component: 

```jsx
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}

// Example usage: <ShoppingList name="Mark" />
```

The `render` method returns a description of what you want to see on the screen. React takes the description and displays the result. `render` returns a React element, which is a lightweight description of what to render. Most React developers use a special syntax called "JSX" which makes these structures easier to write. 

- **JSX Basics**

    JSX comes with the full power of Javascript. You can put any Javascript expression within braces inside JSX. Each React element is a Javascript object that you can store in a variable or pass around in your program. 

    The ShoppingList component above only renders built-in DOM components like <div /> and <li />. But you can compose and render custom React components too. For example, we can now refer to the whole shopping list by writing <ShoppingList />. Each React component is encapsulated and can operate independently; this allows you to build complex UIs from simple components.

## State

React components can have state by setting `this.state` in their constructors. `this.state` should be considered as private to a React component that it's defined in. 

```jsx
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button
        className="square"
        onClick={() => this.setState({value: 'X'})}
      >
        {this.state.value}
      </button>
    );
  }
}
```

*Note: All react component classes that have a constructor should start with a `super(props)` call.* 

> By calling this.setState from an onClick handler in the Square’s render method, we tell React to re-render that Square whenever its <button> is clicked. After the update, the Square’s this.state.value will be 'X'

To collect data from multiple children, or to have two child components communicate with each other, you need to declare the shared state in their parent component instead. The parent component can pass the state back down to the children by using props; this keeps the child components in sync with each other and with the parent component.

Lifting state into a parent component is common when React components are refactored — let’s take this opportunity to try it out.

*Note: The DOM <button> element’s onClick attribute has a special meaning to React because it is a built-in component. For custom components like Square, the naming is up to you. We could give any name to the Square’s onClick prop or Board’s handleClick method, and the code would work the same. In React, it’s conventional to use on[Event] names for props which represent events and handle[Event] for the methods which handle the events.*

Since the Square components no longer maintain state, the Square components receive values from the Board component and inform the Board component when they’re clicked. In React terms, the Square components are now controlled components. The Board has full control over them.

### Immutability

There are generally two approaches to changing data. The first approach is to mutate the data by directly changing the data’s values. The second approach is to replace the data with a new copy which has the desired changes. In React you should default to immutability. Immutability provides several advantages:

- **Detecting changes:** Detecting changes in immutable objects is easy. If the immutable object that is being referenced is different than the previous one, then the object has changed.
- **Complex features become simple:** Avoiding direct data mutation lets us keep previous versions of the game's history intact, and reuse them later.
- **Helps determine when to re-render in react:** Immutable data can easily determine if changes have been made which helps to determine when a component requires re-rendering.

## Function Components

Function components are a simpler way to write components that only contain a `render` method and don't have their own state. Instead of defining a class which extends `React.component` we can write a function that takes `props` as input and returns what should be rendered. 

```jsx
function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}
```

- Ternary Operators

    The conditional (ternary) operator is the only JavaScript operator that takes three operands: a condition followed by a question mark (?), then an expression to execute if the condition is truthy followed by a colon (:), and finally the expression to execute if the condition is falsy. This operator is frequently used as a shortcut for the if statement.

    ```jsx
    function getFee(isMember) {
      return (isMember ? '$2.00' : '$10.00');
    }

    console.log(getFee(true));
    // expected output: "$2.00"
    ```