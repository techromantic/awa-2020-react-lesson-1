# A Reactive World

React is a front-end library that helps developers create awesome User Interfaces.

- It is faster than changing the DOM manually using something like JQuery \$(container).append(moreContent) - React renders the DOM based of its own internal representation of page content
- It is component based and composable. An application can manage state at multiple levels, and pass down relevant data to pieces of UI, which can manage their own state.
- Data flows down from parent to children making it hard to create side-effect bugs in the code (where separate or unrelated code effects affects application data) .

## Our Mission Today

Today we'll be sprinting into React. In the Advanced Web Applications course so far you've learned how to scaffold your own data services. Now you'll learn how to wire these data services into a user interface.

## Prerequistes

It would be very helpful if you knew HTML & JavaScript. You should have a decent idea of how the following programming concepts work.

- Functions
- Objects
- Arrays
- Classes (kind of)

## Lets Get Started

Launch the [project](https://repl.it/@techromantic/DaringUsedModel-1#src/App.js). Create a fork of this project by clicking the project name dropdown at the top left, then clicking fork project. This will allow you to edit the code! Click Run at the top of the screen to launch the example.

## Our AppliCATion

We have a whole universe of data services we can use to react apps. Check out the whole list [here](https://github.com/public-apis/public-apis)

What we want to do is to create a simple application that lists the data out for us to view. I've created a meowing example that you can follow. It uses two data services - a cat facts service, and a cat pictures service. After getting a set of cat facts, it picks a random set, and then renders a cat card for each fact. Each cat card is given a cat fact. The cards are also responsible for retrieving a picture from the cat picture data service which it renders within it!

# The Code Explained

The files in the project are simple.

- App.css (contains styles)
- App.js (main application)
- CatCard.js (a container for our cat facts)
-

### App.css

This is our CSS for the entire project. Anytime we want to use a CSS class, we specify it using the 'className' attribute for the corresponding JSX tag.

### App.js

```sh
import React, {useState, useEffect} from 'react';
import './App.css';
import CatCard from './CatCard';
```

- At the top of the file we'll import everything we need to work with react. Notice we grab React, and two helper hooks from the React library. We also grab a style file, and the cat card file. Simple right? The './' signifies we are importing the file in relation to the current file.

```
function App() {
    ...
    return('<p>whats this?</p>')
}
export default App;
```

- Notice that the entire application is a function. The app function does a whole bunch of stuff, then eventually returns something. That something is what gets rendered. Every React component will render what looks like HTML Markup. This markup is called JSX - JavaScriptXML and allows us to write dynamic markup.

```
  const [catFacts, setCatFacts] = useState([]);
```

- Here's an interesting looking line of logic. This sets up 'state' for our app which is a snapshot of the data. We're usiug a React Hook - which you don't need to understand right now. Just know that the first variable in the array refers to the current piece of state (catFacts), and the second is a function that allows you to change that piece of state by passing in a argument to it.
-

```
  const getCatFacts = () => {
    fetch("https://cat-fact.herokuapp.com/facts")
      .then(res => res.json())
      .then(
        (result) => {
          console.log(result);
          var startIndex = Math.random() * 200;
          setCatFacts(result.all.slice(startIndex,startIndex+5));
        },
        (error) => {
          setCatFacts([]);
        }
      )
  }
```

- We created a function called getCatFacts, which uses fetch (all it does is get data from an API), and after getting the data, it converts it to JSON, and then updates our local state using that state method we just mentioned setCatFacts

```
  useEffect(() => {
    if (catFacts.length == 0) getCatFacts();
  })
```

- Here's another one of those weird 'use' things, this is another hook that allows you to to change local data AFTER the component has rendered. There is a component lifecycle in React, and its important to understand WHEN things are happening. Before or after render? useEffect allows the component to render once and then making changes to the state. When react notices the state has changed, it checks to see if any of the JSX depends on the state - and if it does, it rerenders the component. This allows our application to become interactive. In our case, we allow the component to render, and if catFacts is empty, we call the function which gets AND sets the catFacts.

```
    <div className="App">
      <h3> Welcome to Cat Facts! </h3>
      <div className="cat-facts-container">

          { (catFacts.length == 0) &&
            <p> The cats are sleeping right now... </p>
          }
          { (catFacts.length > 0) &&
            catFacts.map((fact) => <CatCard catFact={fact}/>)
          }
      </div>
    </div>
```

- Here is what is returned by our rendered functions. A few things going on here. Notice the blocks that look suspiciously like if statements, with the &&'s. These blocks can refer to our local state (catFacts) and conditionally render JSX based upon it. We check whether there are cat Facts, if there aren't - we render a simple message, OTHERWISE:
- If there are cat facts, we use our catFacts variable and map over it. map allows us to run a function for every element in an arry. For every fact, we render a CatCard, passing in that fact to each specific card. This value we pass in is called a 'prop'.

### CatCard.js

- A lot of the same things are happening here. Let's explain the new stuff, and I'm sure you can figure out the rest.

```
const CatCard = ({catFact}) => {
```

- Don't be scared of the arrow syntax. This is just another way of writing a function. Notice that an object should be passed in as the only parameter. This object should have a single property called catFact. Remember in the render method of App.js we passed in a catFact property? This refers to that. For the specific case of this data, this catFact happens to be another object. You can console.log() the object to understand its structure.

```
  return (
    <div className="cat-card-root">

      <img className="cat-card-img" src={(catPic) ? catPic : 'https://i.pinimg.com/originals/97/e9/42/97e942ce7fc4e9d4ea6d844a382f251f.gif'}/>
      <p> {catFact.text} </p>

    </div>
  )
```

- Okay so here's the final part. We're rendering a cat-card here. You can see we make references to the catPic. What is catPic referring to? Where do we set it? It looks like we're using a ternary to either render the catPic, or something else. Maybe a cat loading gif? Also within the p tag we're rendering the text property of the cat fact. What could this be?

# Your Assignment

Now you have enough to go off of to create your own data-drive applications. Here is your assignment.

- Create an application which uses **2** APIs to retrieve data, and then find a way to display them together. You will need to work with arrays, objects, and functions that manipulate those structures to get something working. Use the list of public APIs, and find an API that doesn't need an API key to make it easier.
