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

ES6 classes do not have any commas after them.

## What are Components?

Components are reusable chunks of code. There are different ways of declaring your Components.

1. React.createClass
This is the oldest way of creating components. Not recommended!

```js
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

```js
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

3. Stateless functional Components.
   A stateless functional component is just a simple function that returns a react element.
   So whenever you have a component that only does one thing (only need the method render),
   it doenst make sence to use a full react component. E.g a Header

```js
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

This example displays our app component (see example 1). Here we define the prop of our header component, named tagline. After doing so, we can call this tagline inside our Header component(see example 2). We can do so by calling the props of the component.


>Example 1.

```js
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

``` js

If we would log *this* it would give us the element.
Inside the element we will find an object called props, here all the props are stored
of this particular element. We can call those props via this.props.tagline.

Whenever you run Javascript in JSX remember to run your code inside the { brackets }.

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
We will be importing a couple of modules from the react-router package, { BrowserRouter, Match, Miss }.

The way this works is we have Matches, if the url matches our pattern we render a specific component.
You can use this throughout your application, multiple levels deep.

```js
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

When we have functions that are not big enough to be a module we can create a helpers.js file. Here we can create, store and export each function that we create. To call a function from the helpers.js file we simply import the function in our component.

```js
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

In the following example we use refs to get the input from a form field.

After we received the input we also need to bind the input because it does not implicitly binds all of the methods to the actual component it self.

There are multiple ways to achieve this, via the constructor method, and via inline binding.
If you use the constructor method you alway run super() before you bind your ref. Super makes sure
the React class component is first initialized and then bids the ref.

In the example below we also set the default URL to a random value via a function that we call from our helpers.js file.

```js
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
2. If you want your page changes to be handled by a function you can use an imperative API .transitionTo

The way that we access our router is via context object.
We have to make sure our BrowserRouter of the react-router library is our top level parent so we can access this throughout our application. This can be done by the following setup:

```js
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

In React, we only edit the state. React will change the HTML for us. So again, we never actually touch the DOM because React does all of this for us.

So let's say we have an Inventory which displays all of our products, for this example let's say Fish. And every time we add a new fish we want it to be placed into our state. Our state being a list of all of our fishes. We create a fish component.

In our root, in this example our App component, we need to handle the state, this can also be done in Redux but since we dont use Redux for this example we will handle our state in our App.js

In order to change, update state our App should know what states we have.
We can let our App now by initializing our state

```js
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

```js
import React from 'react';
import Header from './Header';
import Order from './Order';
import Inventory from './Inventory';

class App extends React.Component {
  constructor() {
    # (run super so the Component initialises otherwise we can't call this.)
    super();
    # (Bind the method to the actual component itself)
    this.addFish = this.addFish.bind(this);
    # (this is also called getInitialState)
    this.state = {
    fishes: {},
    order: {}
    };    
  }
  # (How do we get the fish from AddFishForm to our App.js?)
  # (Make a method addFish, takes 1 argument)
  # (Update our state)
  # (Set state)
  addFish(fish) {
    # (Take a copy of the state -> takes every item from our object and spread it into this object)
    const fishes = {...this.state.fishes};
    # (Make new key & add in our new fish)
    const timestamp = Date.now();
    fishes[`fish-${timestamp}`] = fish;
    # (Set state)
    this.setState({ fishes: fishes });
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

## Loading data into our state

The method that we need to load sample data can only live where our state lives. That is in our App.js.
So we bind an event on the button which will be named loadSamples() and configure our App.js with this function as well.

```js
import React from 'react';
import Header from './Header';
import Order from './Order';
import Inventory from './Inventory';

class App extends React.Component {
  constructor() {
    # (run super so the Component initialises otherwise we can't call this.)
    super();
    # (Bind the method to the actual component itself)
    this.addFish = this.addFish.bind(this);
    this.loadSamples = this.loadSamples.bind(this);
    # (this is also called getInitialState)
    this.state = {
    fishes: {},
    order: {}
    };    
  }

  addFish(fish) {
    # (Take a copy of the state -> takes every item from our object and spread it into this object)
    const fishes = {...this.state.fishes};
    # (Make new key & add in our new fish)
    const timestamp = Date.now();
    fishes[`fish-${timestamp}`] = fish;
    # (Set state)
    this.setState({ fishes: fishes });
  }

  loadSamples() {
    this.setState({
      fishes: sampleFishes
    });
  }

  render() {
    return (
      <div>
        <Header tagline="Fresh Seafood Market" />
        <Order />
        <Inventory addFish={this.addFish} loadSamples={this.loadSamples} />
      </div>
    )
  }
}

export default App;
```

And then in our Inventory component we do the following

```js
import React from 'react';
import AddFishForm from './AddFishForm';

class Inventory extends React.Component {
  render() {
    return (
      <div>
        <h2>Inventory</h2>
        <AddFishForm addFish={this.props.addFish}/>
        <button onClick={this.props.loadSamples}>Load Sample Fishes </button>
      </div>
    )
  }
}

export default Inventory;
```

## Displaying State with JSX

In order to display our state onto the page we need to create a fish component that we can loop over.
In our App.js we import our Fish component. Inside our app we have the following code

```js
import Fish from '../components/Fish';

class App extends React.Component {
  render() {
    return (
      <div>
        <Header />
        <ul className="list-of-fishes">
          {
            Object
              .keys(this.state.fishes)
              # (here we iterate over the array that we got due to our Object.keys(this.state.fishes))
              # (It returns an Array with keys, so we can so for each key we make render a <Fish /> Component with)
              # (an unique key, and as details we pass it all the data of this specific fish, key*)
              .map(key => <Fish key={key} details={this.state.fishes[key]} />)
          }
        </ul>
      </div>
    )
  }
}

```


```js
import React from 'react';

class Fish extends React.Component {
  render() {
    const { details } = this.props
    return (
      <li className="menu-fish">
        <img src={details.image} alt={details.name} />
        <h3>
          {details.name}
        </h3>
        <button>Add To Order</button>
      </li>
    )
  }
}

export default Fish;
```
