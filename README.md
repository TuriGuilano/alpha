# Learn React course

>Topics that will be highlighted

* What are Components
* Different Component types
* JSX
* CSS in React
* Passing dynamic data via Props
* Stateless functional Components
* Routing with React router
* Helper and Utility functions
* Working with React events
* All about React router
* Understanding State
* Loading data into state onClick
* Displaying state with JSX

## What are Components?

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

## Passing dynamic data via Props

Props are similar to attributes of HTML tags. So if you want to pass information to a tag, you do it via props.

So this example displays our app component (see example 1). Here we define the prop of our header component, named tagline. After doing so, we can call this tagline inside our Header component(see example 2). We can do so by calling the props of the component.


>Example 1.

```shell
class App extends React.Component {
  render() {
    return (
      <div>
        {/*  */}
        <Header tagline="My header Component"  />
      </div>
    );
  }
}

export default App;
```
>Example 2.

class Header extends React.Component {
  render() {
    >If we would console.log(this); it would give us the element,
    inside the element we will find an object called props, here all the props are stored
    of this particular element.

    return (
      <header className="top">
        <h1>Catch of the day</h1>
        <h2 className="tagline">{this.props.tagline}</h2>
      </header>
    );
  }
}

export default Header;
