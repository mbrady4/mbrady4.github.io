# Data Flows

React utilizes a top to bottom strategy when passing data.

### Props

When we want to pass information held on state inside one component to another component, we pass them as props. The important thing to remember is that we never make changes to props data - **props are read-only.** This helps ensure that our data flow remains clean and organized. This way, we know exactly where changes are made to our application.

### Stateful components

A stateful component is one that holds state data, either as an object placed inside the constructor function, or a function component that includes the `.useState` function. 

When data comes into our application, it is loaded and stored on state, either in a centralized component specifically for state management, or a component rendering other components. When data is consumed in multiple components, it is probably best to centralize that data in state in a top-level component. Other data that is specific to a certain component can live locally, just inside that component. Components rendered in a stateful component can receive state data via a props attribute. Here, it can be sent down on the props object to the child component, and there we can access it just like we would with most any other object. However, if we decide we want to make any change to our data we do not change the prop data itself. Instead, we send back what changes we should make to our state holding component; often, stored changes are sent back up to the parent container as enclosed information in a called function.

[nice-dijkstra-59jgo](https://codesandbox.io/s/nice-dijkstra-59jgo?file=/src/index.js)