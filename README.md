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
* Lifecycle hooks

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
   A stateless functional component is just a simple function that returns a react element.
   So whenever you have a component that only does one thing (only need the method render),
   it doenst make sence to use a full react component. E.g a Header

```shell
  const MyHeaderComponent = (props) => {
    return (
      <header>
        <h1>My header component</h1>
        <h2>{props.tagline}</h2>
      </header>
    );
  }

  export default MyHeaderComponent;
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

``` shell

_If we would console.log(this); it would give us the element,
inside the element we will find an object called props, here all the props are stored
of this particular element._

class Header extends React.Component {
  render() {
    return (
      <header className="top">
        <h1>Catch of the day</h1>
        <h2 className="tagline">{this.props.tagline}</h2>
      </header>
    );
  }
}

export default Header;
```

## Routing with React router

In our index.js we want to handle all the routing. The routing will be done via react-router.
We will be needing browserrouter, match and miss from the react-router package.
The way this works is we have Matches, if the Match is exactly pattern="/" we render a specific component.
So based on the url, we can render out components with the exactly pattern. You can use this throughout your application, multiple levels deep.

```shell
import React from 'react';
import { render } from 'react-dom';
import { BrowserRouter, Match, Miss } from 'react-router';

import './css/style.css';
import App from './components/App';

import StoreSelector from './components/StoreSelector';
import NotFound from './components/NotFound';

const Root = () => {
  return (
    <div>
      <BrowserRouter>
        <Match exactly pattern="/" component={StoreSelector} />
        <Match pattern="/store/:storeId" component={App} />
        <Miss component={notFound} />
      </BrowserRouter>
    </div>
  )
}

render(<Root/>, document.querySelector('#main'));
```

>Named exports

When we have functions that are not big enough to be a module but just small helper functions we can create a helpers.js file. Here we can export each function that we create. By using these functions we can simply import the function name. E.g we want to display a randon default name in the input field, so we import a function that creates a random name.

```shell
import React from 'react';
import { getNames } from '../helpers';

class StorePicker extends React.Component {
  render() {
    return (
      <form>
        <div>
          <h2>Please enter your name</h2>
          <input type="text" required placeholder="Your name" defaultValue={getNames()} />
          <button type="submit">Check profile</button>
        </div>
      </form>
    )
  }
}

export default StorePicker;
```

## Working with react events
Events in React are exactly the same as events in regular Javascript. The only difference with React events is that they wrap them in this cross-browser wrapper called SyntheticEvent. This does some stuff under the hood, makes sure it works cross-browser for you.

Notice that events are done inline. Also notice that render methods are bound to the actual class that you are in. So when you use this* its bound to the class that you are in.

The following example gets us the value that is being typed in and change the url.
Note* es6 classes do not have any commas after them.


```shell
import React from 'react';
import { getNames } from '../helpers';

class StorePicker extends React.Component {
  goToStore(event) {
    event.prefentDefault();

  }

  render() {
    return (
      <form onSubmit={this.goToStore}>
        <div>
          <h2>Please enter your name</h2>
          <input type="text" required placeholder="Your name" defaultValue={getNames()} />
          <button type="submit">Check profile</button>
        </div>
      </form>
    )
  }
}

export default StorePicker;
```
