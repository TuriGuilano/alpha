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

## Do's and Dont's

Try to stay away from actually touching the DOM as much as possible.
React handles that part for us, whenever we want to update a field or a component we update the state.

React uses something called HTML5 Push state, this means React will actually change the urlbar but it actually doesn't refresh your browser. This is why React is so fast because the whole site is already loaded.

## What are Components?

Components are reusable chunks of code. There are different ways of declaring your Components.

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

2. ES6 Class Components

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
In order to get the value from a actual input we use the ref={(input) => {this.storeInput = input }} attribute.

After we also need to bind the ref because it doens't implicitly binds all of the methods to the actual component it self. We can do that by the cunstructor method and running super inside it. This makes sure it first runs the React component and allows us to sprinkle our extra stuff. And then super allows us to bind our own methods to the StorePicker component. NOTE** Its also possible that you bind the method inline, this is a bit more clearer.

Note* es6 classes do not have any commas after them.


```shell
import React from 'react';
import { getNames } from '../helpers';

class StorePicker extends React.Component {
  constructor() {
    super();
    this.goToStore = this.goToStore.bind(this);
  }

  goToStore(event) {
    event.prefentDefault();

  }

  render() {
    return (
      <form onSubmit={this.goToStore.bind(this)}>
        <div>
          <h2>Please enter your name</h2>
          <input type="text" required placeholder="Your name" defaultValue={getNames()} ref={(input) => {this.storeInput = input}} />
          <button type="submit">Check profile</button>
        </div>
      </form>
    )
  }
}

export default StorePicker;
```

## Changing pages in React

With react-router 4 there are two main ways to actually change the page.

1. You can render out a redirect Component.

2. If you want a function that you can run to change the page you can use an imperative API.
.transitionTo

The way that we access our router is via context. When our BrowserRouter of the react-router library is our top level parent we can access this throughout our application. You can do so via the following setup.
Note* You can inspect your component with the react devtools and see the props and context of the component
The context object has a method called transitionTo. which allows us to call this method inside our goToStore method.

So we need to serve the router from the parent with a thing called contextTypes.

```shell
class StorePicker = React.Component {
  goToStore(event) {
    event.preventDefault();
    const storeId = this.storeInput.value;
    this.context.router.transitionTo(`/store/${storeId}`);
  }

  render() {
    return (
      <form onSubmit={this.goToStore.bind(this)}>
        <div>
          <h2>Please enter your name</h2>
          <input type="text" required placeholder="Your name" defaultValue={getNames()} ref={(input) => {this.storeInput = input}} />
          <button type="submit">Check profile</button>
        </div>
      </form>
    )
  }
}

Storepicker.contextTypes = {
  router: React.PropTypes.object
}

export default StorePicker;

```

## State

State is a representation of all of our data in our Application. Each component can have its own state. You
can see state as one big object that holds all of our data related to a piece or all of our application.

In React, we edit the data and React will change the HTML for us. So again, we never actually touch the DOM because React does all of this for us.

So let's say we have an Inventory which displays all of our products, for this example let's say Fish. And every time we add a new fish we want it to be placed into our state. Our state being a list of all of our fishes. We create a fish component as displayed below:

In our root, in this example our App component we need to handle the state, this can also be done in Redux but since we dont use that in this example we will handle our state in our App.js

In order to change update state our App should know what states we have.
We do so by initializing the state in our App.js (see example 2.)

```shell
import React from 'react';

class AddFishForm extends React.Component {
  createFish(event) {
    event.preventDefault();

    const fish = {
      name: this.name.value,
      price: this.price.value,
      status: this.status.value,
      desc: this.desc.value,
      image: this.image.value,
    }
  }

  render() {
    return (
      <form onSubmit={(e) => {this.createFish(e)}}>
        <input ref={(input) => this.name = input} type="text" placeholder="Fish name" />
        <input ref={(input) => this.price = input} type="text" placeholder="Fish price" />
        <select ref={(input) => this.status = input}>
          <option value="available">Fresh</option>
          <option value="available">Sold Out!</option>
        </select>
        <textarea ref={(input) => this.desc = input} placeholder="Fish Desc"></textarea>
        <input ref={(input) => this.image = input} type="text" placeholder="Fish Image" />
        <button type="submit">+ Add Item</button>
      </form>
    )
  }
}

export default AddFishForm;
```  

>Example 2

```shell
import React from 'react';
import Header from './Header';
import Order from './Order';
import Inventory from './Inventory';

class App extends React.Component {
  constructor() {
    [//]: # (run super so the Component initialises otherwise we can't call this.)
    super();
    # (this is also called getInitialState) 
    this.state = {
    fishes: {},
    order: {}
  };    
}

  render() {
    return (
      <div>
        <Header tagline="Fresh Seafood Market" />
        <Order />
        <Inventory />
      </div>
    )
  }
}

export default App;
```
