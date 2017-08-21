* Learn React course

** Topics that will be highlighted

[x] What are Components
[x] Different Component types
[ ] JSX
[ ] CSS in React
[ ] Passing dynamic data via Props
[ ] Stateless functional Components
[ ] Routing with React router
[ ] Helper and Utility functions
[ ] Working with React events
[ ] All about React router
[ ] Understanding State
[ ] Loading data into state onClick
[ ] Displaying state with JSX

** What are Components?

Components are reusable chunks of code. There are different types of Components.
1. React.createClass
This is the oldest way of creating components. Not recommended!

```shell
const firstComponent = React.createClass({
  render() {
    return (
      <div>
        <h1>My first Component!</h1>
      </div>
    );
  }
});
```

2. Class Components

```shell
export class firstComponent extends React.Component {
  render() {
    return (
      <div>
        <h1>My first Component!</h1>
      </div>
    );
  }
}
```

3. Stateless functional Components
A stateless functional component is just a simple function that returns a react element

```shell
  const MyComponent = () => {
    return (
      <div>
        <h1>My Component</h1>
      </div>
    );
  }
```
