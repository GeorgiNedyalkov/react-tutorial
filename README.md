# React Tutorial

Building a small game of Tic Tac Toe to learn about React.

## Table of contents

- Setup for the tutorial
- Overview of the fundamentals of Read: components, props and state.
- Completing the game teaches the most common techniques in React development.
- Adding Time Travel will give a deeper insight into the strengths of React.

## Setup Local Environment

Bulding __a new single-page application__ in React.

1. Make sure you have ```Node.js``` installed.
2. Follow the installation instructions for Create React App to make a new project.

To create a project run:
```
npx create-react-app my-app
cd my-app
npm start
```
Create React App doesn't handle backend logic or databases; it just creates a frontend build
pipeline, so you can use it with any backend you want. Under the hood, it uses ```Babel``` and
```webpack``` but you don't need to know anything about them.

3. Delete all files in the ```src/``` folder of the new project.
```
cd src
del *
cd..
```
4. Add a file name ```index.css``` in the ```src/``` folder with this [CSS code](https://codepen.io/gaearon/pen/oWWQNa?editors=0100)
5. Add a file named ```index.js``` in the ```src/``` folder with this [JS code](https://codepen.io/gaearon/pen/oWWQNa?editors=0010)
6. Add these three lines to the top of the ```index.js``` in the ```src/``` forlder:
```JSX
import React from 'react';
import ReactDOM from 'react-dom/client'
import './index.css';
```
Now if you run ```npm start``` in the project folder and opent ```http://localhost:3000``` in
the browse, you should see an empty tic-tac-toe field.

## Overview

What is React? 

React is a declarative, efficient and flexible JavaScript library for building user interfaces.
It let's you compose comples UIs from small and isolated pieces of code called "components".

There are diffent types of components. We'll start with React.Component
```JSX
class ShoppingList extends React.Component {
    render() {
       return (
        <div className="shoppingList">
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
// Example usage: <ShoppingList name="Mark">
```

We use components to tell React what we want to see on the screen. When our data changes, 
React will efficiently update and re-render our components.

ShoppingList is a React component class, or React component type. A component takes in parameters,
calles ```props``` (short for "properties), and returns a hierarchy of views to display via the ```render``` methods.

The ```render``` method returns a description of what you want to see on the screen. React takes the description
and displays the result. In particular, ```render``` returns a React element, which is a lightweight description
of what to render. Most React developers use a special syntax called "JSX" which makes these structures easier to write.
The <div /> syntax is transformend at build time to ```React.createElement('div')```. The above example is equivalent to:
```JSX
return React.createElement('div', {className: 'shopping-list'}i,
    React.createElement('h1', /* ... h1 children ... */),
    React.createElement('ul', /* ... ul children ... */)
);

```

JSX comes with the full power of JavaScript. You can put any JavaScript expressions within the braces inside JSX.
Each React element is a JavaScript object that you can store in a variable or pass around in your program.

The ```ShoppingList``` component above only renders built-in DOM components like ```<div/>``` and ```<li/>```.
But you can compose and render custom React components too. For example, we can now refer to the whole shopping list
by writing ```<ShoppingList />```. Each React component is encapsulated and can operate independently;
this allows you to build complex UIs from simple components.

### __Inspecting the Started Code__

Our starter code has three React components:
- Square
- Board
- Game

The Square component renders a single ```<button>``` and the Board renders 9 squares. The Game component renders a board with a placeholder values which we'll modify later. There are currently 
no interactive components.

### __Passing Data Through Props__

Let's try passing some data from our Board component to our Square component.

In the Board's ```renderSquare``` method, change the code to pass a prop called ```value``` to the Square:
```JSX
class Board extends React.Component {
    renderSquare(i) {
        return <Square value={i} />;
    }
} 
```
Change Square's ```render``` method to show that value by replacing ```{/* TODO */}``` with
```{this.props.value}```:

```JSX
class Square extends React.Component {
    render() {
        return (
            <button className="square">
                {this.props.value}
            </button>
        )
    }
}
```

### __Making an Interactive Component__

Let's fill the Square component with an "X" when we click it. First, change the button tag that is 
returned from the Square component's ```render()``` function to this:
```JSX
class Square extends React.Component {
    render() {
        return (
            <button className="square" onClick={() => {console.log('click'); }}>
                {this.props.value}
            </button>
        )
    }
}
```

As a next step, we want the Square component to "remember" that it got clicked, and fill it with an
"X" mark. To "remember" things, components use __state__. 

React components can have state by setting ``this.state`` in their constructors. ``this.state`` should
be considered as private to a React component that it's defined in. Let's store the current value of Square 
in ``this.state``, and change it when Square is clicked.

First, we'll add a constructor to the class to initialize the state:

```JSX
class Square extends React.Component{
    constructor(props) {
        super(props);
        this.state = {
            value: null,
        }
    };

    render() {
        return {
            <button className="square" onClick={() => console.log('click')}>
                {this.props.value}
            </button>
        };
    }
}

```


> _NOTE:_ <br>
In JavaScript classes, you need to always call ``super`` when defining the constructor of a
subclass. All React component classes that have a ``constructor`` should start with a 
``super(props)`` call.

Now we'll change the Square's ```render``` method to display the current state's value when clicked:

- Replace ``this.props.value`` with ``this.state.value`` inside the ``<button>`` tag.
- Replace the ``onClick={...}`` event handler with ``onClick{() => this.setState({value: 'X'})}``.
- Put the ``className`` and ``onClick`` props on a separate lines for better readability.

```JSX
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

By calling ``this.setState`` from an ``onClick`` handler in the Square's ``render`` method, we tell
React to re-render that Square whenever ``<button>`` is clicked. After the update, the Square's 
``this.state.value`` will be ``'X'``, so we'll see the X on the game board. If you click on any Square,
an ``X`` should show up.

When you call ``setState`` in a component, React automatically updates the child components inside it too.