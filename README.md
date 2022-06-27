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


> __Note__: <br>
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

### __Developer Tools__

The React Devtools extention for [Chrome](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en) and [Firefox](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/) lets you inspect a React component tree with
your browser's developer tools.

The React DevTools let you check the props and the state of your React components.

After installing React DevTools, you can right-click on any element on the page, click “Inspect” to open the developer tools, and the React tabs (“⚛️ Components” and “⚛️ Profiler”) will appear as the last tabs to the right. Use “⚛️ Components” to inspect the component tree.

## __Completing the Game__

We not have the basic block for our tic-tac-toe game. To have a complete game, we now need to alternate
placing "X"s and "O"s on the board, and we need a way to determine a winner.

__Lifting State Up__

Currently, each Square component maintains the game's state. The check for a winner, we'll maintain the value of each
of the 9 squares in one location.

We may think that Board should just ask each Square for the Square's state. Although this
approach is possible in React, we discourage it because the code becomes difficult to understand,
suseptible to bugs, and hard to refactor. Instead, the best approach is to store the game's state in the
parent Board component instead of in each Square. The Board component can tell each Square what to display
by passing a prop, just like we did when we passed a number to each Square.

__To collect date from multiple children, or to have two child components communicate with each other,
you need to declare the shared state in their parent component instead. The parent component can pass the state
back down to the children by using props; this keeps the child components in sync with each other and with the parent component.__

Lifting state into a parent component is common when React components are refactored. 

Add a constructor to the Board and set the Board's initial state to contain an array of 9 nulls
corresponding to the 9 squares:

```JSX
class Board extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            squares: Array(9).fill(null),
        };
    }

    renderSquare(i) {
        return <Square value={i} />;
    }
```

When we fill the board later, the ``this.state.squares`` array will look something like this:
```JSX
[
    'O', null, 'X',
    'X', 'X', 'O',
    'O', null, null,
]
```

The Board's ``renderSquare`` method currently looks like this:

```JSX
renderSquare(i) {
    return <Square value ={i}>;
}
```

We will modify the Board to instruct each individual Square about its current value ``'X'``, ``'O'``, or ``null`` for 
empty squares.

Next, we need to change what happens when a Square is clicked. The Board component now maintains
which squares are filled. We need to create a way for the Square to update the Board's state.
Since state is considered to be private to a component that defines it, we cannot update the Board's state
directly from Square.

Instead, we'll pass down a function from the Board to the Square, and we'll have Square call 
that function when a square is clicked. We'll change the ``renderSquare`` method in Board to:

```JSX
renderSquare(i) {
    return (
        <Square
        value={this.state.squares[i]}
        onClick={() => this.handleClick} 
        />
    )
}
```

> __Note__: <br> 
We split the returned element into multiple lines for readability, and added parentheses so
that JavaScript doesn't inser a semicolon after ``return`` and break our code.

Now we're passing down two props from Board to Square: ``value`` and ``onClick``. The ``onClick``
prop is a function that Square can call when clicked. We'll make the following changes to Square:
- Replace ``this.state.value`` with ``this.props.value`` in Square's ``render`` method.
- Replace ``this.setState()`` with ``this.props.onClick()`` in Squares ``render`` method.
- Delete the ``constructor`` from Square because Square no longer keeps track of the game's state.

After these changes, the Square component looks like this:

```JSX
class Square extends React.Component {
    render() {
        return (
            <button
              className="square"
              onClick={() => this.props.onClick()} 
            >
              {this.props.value}
            </button>
        );
    }
}
```

When a Square is clicked, the ``onClick`` function provided by the Board is called. Here's a review
of how this is achieved:
