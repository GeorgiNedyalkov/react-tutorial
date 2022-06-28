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

### __Lifting State Up__

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

1. The ``onClick`` prop on the built-in DOM ``<button>`` component tells React to set up a click event
listener. 
2. When the button is clicked, React will call the ``<onClick>`` event handler that is defined in Square's
3. The event handler calls ``this.props.onClick()``. The Square's ``onClick`` prop was specified by the Board.
4. Since the Board passed ``onClick={() => this.handleClick(i)}`` to Square, the Square calls the Board's
``handleClick(i)`` when clicked.
5. We have not defined the ``handleClick()`` method yet, so our code crashes. If you click a square now,
you should see a red error saying something like "this.HandleClick is not a function".


> __Note__: <br> 
The DOM ``<button>`` element's ``onClick`` attribute has a special meaning in to React because it is a 
built-in component. For custom components, like Square, the naming is up to you. We could give any
name to the Square's ``onClick`` prop or Board's ``handleClick`` method, and the code would work the same.
In React, it's conventional to use ``on[Event]`` names for props which represent events and ``handle[Event]``
for the methods which handle the events.

Let's add ``handleClick`` to the Board class:

```JSX
handleClick(i) {
    const squares = this.state.squares.slice();
    square[i] = 'X'j
    this.setState({squares: squares});
}
```

We can once again fill the squares in the board. However, now the state is stored in the Board component
instead of individual Square components. When the board's state changes, the Square components re-render
automatically. Keeping the state of all squares in the Board component will allow it to determine
the winner in the future. 

Since the Square components no longer maintain state, the Square components receive values from the Board
component and inform the Board component when they're clicked. In React terms, the Square components are now
__controlled components__. The Board has full control over them.

Note how in ``handleClick``, we call ``slice()`` to create a copy of the ``squares`` array to modify
instead of modifying the existing array. We will explain later why.

### __Why immutability is important__

There are generally two approaches to changing data. The first approach is to _mutate_ the data
by directly changing the data's values. The second approach is to replace the data with a new copy
which has the desired changes.

#### Data Change with Mutation
```JS
var player = {score: 1, name: 'Jeff'};
player.score = 2;
// Now player is {score: 2, name: 'Jeff'}
```

#### Data Change without Mutation
```JS
var player = {score: 1, name: 'Jeff'};

var newPlayer = Object.assign({}, player, {score: 2});
// Now player is unchanged, but newPlayer is {score: 2, name: 'Jeff'}

// Or if you are using object spread syntax, you can write:
// var newPlayer = {...player, score: 2};
```

The end result is the same but by not mutating (or changing the underlying data) directly, we
gain several benefits described below.

#### Comlpex Features Become Simple

Immutability makes complex features much easier to implement. Later we will implement a "time travel" feature
that allows us to review the tic-tac-toe game's history and "jump back" to previous moves. This functionality
isn't specific to games - an ability to undo and redo certain actions is a common requirement in applications. 
Avoiding direct data mutation lets us keep previous versions of the game's history intact, and reuse them later.

#### Detecting Changes

Detecting changes in mutabable objects is difficult because they are modified directly. This 
detection requires the mutable object to be compared to previous copies of itself and the entire
object tree to be traversed.

Detecting changes in immutable objects is considerred easier. If the immutable object that is being
referenced is different than the previous one, then the object has changed.

#### Determining When to Re-Render in React

The main benefit of immutability is that it helps you build _pure components_ in React. Immutable
data can easily determine if changes have been made, which helps to determine when a component requires
re-rendering. 

### Functional Components

We'll now change the Square to be a __functional component__.

In React, __functional components__ are a simpler way to write components that only contain a
``render`` method and don't have their own state. Instead of defining a class which extends
``React.Component``, we can write a function that takes ``props`` as input and returns what should
be rendered. Function components are less tedious to write than classes, and many components can
be expressed this way. 

Replace the Square class with this function:

```JSX
function Square(props) {
    return (
        <button className="square" onClick={props.onClick}>
          {props.value}
        </button>
    );
}
```
We have changed ``this.props`` to ``props`` both times it appears. 

> _Note_ <br>
When we modified the Square to be a function component, we also changed ``onClick={() => this.props.onClick()}``
to a shorter ``onClick={props.onClick}`` (note the lack of parenthesis on _both_ sides).

### __Taking Turns__

We now need to fix an obvious defect in our tic-tac-toe game: the "O"s cannot be marked on the board.

We'll set the first move to be "X" by default. We can set this default by modifying the initial state
in our Board constructor:

```JSX
class Board extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
           squares: Array(9).fill(null),
           xIsNext: true,
        }
    }
}
```
Each time a player moves, ``xIsNext`` (a boolean) will be flipped to determine which player goes
next and he game's state will be saved. We'll update the Board's ``handleClick`` function to flip the
value of ``xIsNext``:

```JSX
handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
        squares: squares,
        xIsNext: !this.state.xIsNext,
    })
}
```

With this change, "X"s and "O"s can take turns. 

Let's also change the "status" text in Board's ``render`` so that it displays which player
has the next turn:

```JSX
render() {
    const status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');

    return (
        // the rest has not changed
    )
}
```

### __Declaring a Winner__

Now that we show which player's turn is next, we should also show when the game is won and
tehre are no more turns to make. Copy this helper function and paste it at the end of the file:

```JSX
function calculateWinner(square) {
    const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
    ];
    for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i];
        if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
            return squares[a];
        }
    }
    return null;
}
```

Give an array of 9 squares, this function will check for a winner and return ``'X'``, ``O``, or ``null``
as appropriate.

We will call ``calculateWinner(squares)`` in the Board's ``render`` function to check if a player
has won. If a player has won, we can display text such as "Winner: X" or "Winner: O". We'll
replace the ``status`` declaration in Board's ``render`` function with this code:

```JSX
render() {
    const winner = calculateWinner(this.state.squares);
    let status;
    if (winner) {
        status = 'Winner: ' + winner;
    } else {
        status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return {
        // the rest has not changed
    }
}
```
We can now change the Board's ``handleClick`` function to return early by ignoring a click if someone
has won the game or if a Square is already filled. 

```JSX
handleClick(i) {
    const squares = this.state.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
        return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
        squares: squares,
        xIsNext: !this.state.xIsNext,
    });
}
```

We now have a working tic-tac-toe game.




## __Adding Time Travel__

Let's make it possible to "go back in time" to the previous moves in the game. 

### __Storing a History of Moves__

If we mutated the ``squares`` array, implementing a time travel would be difficult.

However, we used ``slice()`` to create a new copy of the squares array after every move, and treated it as immutable. 
This will allow us to store every past version of the ``squares`` array, 
and navigate between the turns that have already happened.

We'll store the past ``squares`` arrays in another array called ``history``. The ``history`` array represents
all board states, from the first to the last move, and has a shape like this:

```JSX
history = [
  // Before first move
  {
    squares: [
      null, null, null,
      null, null, null,
      null, null, null,
    ]
  },
  // After first move
  {
    squares: [
      null, null, null,
      null, 'X', null,
      null, null, null,
    ]
  },
  // After second move
  {
    squares: [
      null, null, null,
      null, 'X', null,
      null, null, 'O',
    ]
  },
  // ...
]

```
Now we need to decide which component should own the ``history`` state.

### Lifting State Up, Again

We'll want the top-level Game component to dispaly the list of past moves. It will need access to
the ``history`` to do that, so we will place the ``history`` state in the top-level Game component.

Placing the ``history`` state into the Game component lets us remove the ``squares`` state from its
child Board component. Just like we lifted the Square state to the Board component, now we are lifting
up from Board into the top-level Game component. This gives the Game component full control over the Board's
data, and lets is intstruct the Board to render previous turns from the history.

First, we'll set up the initial state for the Game component within its constructor:

```JSX
class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      history: [{
        squares: Array(9).fill(null),
      }],
      xIsNext: true,
    };
  }
```

Next, we'll have the Board component receive ``squares`` and ``onClick`` props from the Game component.
Since we now have a single click handler in Board for many Squares, we'll need to pass the location of each
Square into the ``onClick`` handler to indicate which Square was clicked. Here are the required steps
to transform the Board component:
- Delete the ``constructor`` in Board.
- Replace ``this.state.squares[i]`` with ``this.props.squares[i]`` in Board's ``renderSquare``.
- replace ``this.handlecClick(i)`` with ``this.props.onClick(i)`` in Board's ``renderSquare``.

We'll update the Game component's ``render`` function to use the most recent history entry to deremine
and display the game's status:

```JSX
  render() {
    const history = this.state.history;
    const current = history[history.length - 1];
    const winner = calculateWinner(current.squares);
    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      <div className="game">
        <div className="game-board">
          <Board
            squares={current.squares}
            onClick={(i) => this.handleClick(i)}
          />
        </div>
        <div className="game-info">
          <div>{status}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
```

Since the Game component is now rendeing the game's status, we can remove the corresponding code
from the Board's ``render`` method. After refactoring, the Board's ``render`` function looks like this:
```JSX
  render() {
    return (
      <div>
        <div className="board-row">
          {this.renderSquare(0)}:w
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
```
Finally, we need to move the ``handleClick`` method from the Board component to the Game component.
We also need to modify ``handleClick`` because the Game component's state is structured differently.
Within the Game's ``handleClick`` method, we concatenate new history entries onto ``history``.

> __Note__ <br>
Unlike the array ``push()`` method you might be more familiar with, the ``concat()`` method 
doesn't mutate the original array.

At this point, the Board component only needs the ``renderSquare`` and ``render`` methods.
The game's state and the ``handleClick`` method should be in the Game component.

### __Showing the Past Moves__

Since we are recording the tic-tac-toe game's history, we can now display it to the player as a 
list of past moves.

We learned earlier that React elements are first-class JavaScript objects; we can pass them around
in our applications. To render multiple items in React, we can use an array of React elements.

In JavaScript, arrays have a ``map() method`` that is commonly used for mapping the data to other 
data, for example:
```JSX
const numbers = [1, 2, 3];
const doubled = numbers.map(x => x * 2); // [2, 4, 6]
```
Using the ``map`` method, we can map our history of moves to React elements representing buttons
on the screen, and display a list of buttons to "jump" to past moves.

Let's ``map`` over the ``history`` in the Game's ``render`` method.
```JSX
  render() {
    const history = this.state.history;
    const current = history[history.length - 1];
    const winner = calculateWinner(current.squares);

    const moves = history.map((step, move) => {
      const desc = move ?
        'Go to move #' + move :
        'Go to game start';
      return (
        <li>
          <button onClick={() => this.jumpTo(move)}>{desc}</button>
        </li>
      );
    });

    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      <div className="game">
        <div className="game-board">
          <Board
            squares={current.squares}
            onClick={(i) => this.handleClick(i)}
          />
        </div>
        <div className="game-info">
          <div>{status}</div>
          <ol>{moves}</ol>
        </div>
      </div>
    );
  }
```

As we iterate through ``history`` array, ``step`` variable refers to the current ``history`` element
value, and ``move`` refers to the current ``history`` element index. We are only interested in ``move``
here, hence ``step`` is not getting assigned to anything. 

For each move in the tic-tac-toe game's history, we create a list item ``<li>`` which contains a
button ``<button>``. The button has a ``onClick`` handler which calls a method called ``this.jumpTo()``.
We haven't implemented the ``jumpTo()`` method yet. For now, we should see a list of the moves that 
have occurred in the game and a warning in the developer tools console that says:

> __Warning__: Each Child in an array or iterator should have a unique "key" prop. Check the render method of "Game"

### __Picking a Key__

When we render a list, React stores some information about each rendered list item. When we
update a list, React needs to determine what has changed. We could have added, removed, re-arranged,
or updated the list's items.

Imagine transitioning from
```JSX
<li>Alexa: 7 tasks left</li>
<li>Ben: 5 tasks left</li>
```
to
```JSX
<li>Ben: 9 tasks left</li>
<li>Claudia: 8 tasks left</li>
<li>Alexa: 5 tasks left</li>
```

In addition to the updated counts, a human reading this would probably say that we swapped
Alexa and Ben's ordering and inserted Claudia between Alexa and Ben. However, React is a 
computer program and does not know what we intended. Because React cannot know our
intentions, we need to specify a _key_ property for each list item to differentiate each list item
from its siblings. One option would be to use the strings ``alexa, ben, claudia``. If we were
displaying data from a database, Alexa, Ben, and Claudia's database IDs could be used as keys.

```JSX
<li key={user.id}>{user.name}: {user.taskCount} task left </li>
```
When a list is re-rendered, React takes each list item's key and searches the previous list's items
for a matching key. If the current list has a key that didn't exist before, React creates a
component. If the current list is missing a key that existed in the previous list, React destroys
the previous component. If two key match, the corresponding component is moved. Keys tell React about
the identity of each component which allows react to maintain state between re-renders. If a 
component's key changes, the component will be destroyed and re-created with a new state.

``Key`` is a special and reserved property in React (along with ``ref``, a more advanced feature).
When an element is created, React extracts the ``key`` property and stores the key directly on the
returned element. Even though ``key`` may look like it belongs in ``props, key`` cannot be referenced
using ``this.props.key``. React automatically uses ``key`` to decide which components to update.
A component cannot inquired about its ``key``.

__It's strongly recommended that you assign proper keys whenever you build dynamic lists.__
If you don't have an appropriate key, you may want to consider restructuring your data so that you do.

If no key is specified, React will present a warning and use the array index as a key by default.
Using the array index as a key is problematic when trying to re-order a list's items or 
inserting/removing list items. Explicitly passing ``key={i}`` silences the warning but has the same
problems as array indices and is not recommended in most cases. 

Keys do not need to be globally unique; they only need to be unique between components and their siblings.

### __Implementing Time Trave__

In the tic-tac-toe game's history, each past move has a unique ID associated with it: it's the
sequential number of the move. The moves are never re-ordered, deleted, or inserted in the middle,
so it's safe to use the mvoe index as a key.

In the Game component's ``render`` method, we can add the key as ``li key={move}>`` and React's warning 
about keys should disappear:
```JSX
    const moves = history.map((step, move) => {
      const desc = move ?
        'Go to move #' + move :
        'Go to game start';
      return (
        <li key={move}>
          <button onClick={() => this.jumpTo(move)}>{desc}</button>
        </li>
      );
    });
```

Clicking any of the list item's buttons throws an error because the ``jumpTo`` method is undefined.
Before we implement ``jumpTo``, we'll add ``stepNumber`` to the Game's component's state to indicate
which step we're currently viewing.

```JSX
class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      history: [{
        squares: Array(9).fill(null),
      }],
      stepNumber: 0,
      xIsNext: true,
    };
  }
```

```JSX
  handleClick(i) {
    // this method has not changed
  }

  jumpTo(step) {
    this.setState({
      stepNumber: step,
      xIsNext: (step % 2) === 0,
    });
  }

  render() {
    // this method has not changed
  }
```
Notice in ``jumpTo`` method, we haven't updated ``history`` property of the state. That is because
state updates are merged or in more simple words React will update only the properties mentioned
in ``setState`` method leaving the remaining state as is. 

The ``stepNumber`` state we've added reflects the move displayed int he user now. After we make a new move,
we need to update ``stepNumber`` by adding ``stepNumber: history.length`` as part of the ``this.setState``
argument. This ensures we don't get stuck showing the same move after a new one has been made.

We will also replace reading ``this.state.history`` with ``this.state.history.slice(0, this.state.stepNumber + 1)``.
This ensures that if we "go back in time" and then make a new move from that point, we throw away all the "future"
history that would now be incorrect.

```JSX
  handleClick(i) {
    const history = this.state.history.slice(0, this.state.stepNumber + 1);
    const current = history[history.length - 1];
    const squares = current.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      history: history.concat([{
        squares: squares
      }]),
      stepNumber: history.length,
      xIsNext: !this.state.xIsNext,
    });
  }
```

Finally, we will modify the Game component's ``render`` method from always rendering the last move to rendering
the currently selected move according to ``stepNumber``:

```JSX
  render() {
    const history = this.state.history;
    const current = history[this.state.stepNumber];
    const winner = calculateWinner(current.squares);

    // the rest has not changed
```

If we click on any step in the game's history, the tic-tac-toe board should immediately update to show what the
board looked like after that step occured.