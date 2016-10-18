---
title: OverReactions - A Tale of React
excerpt: "One mans story of building an application in React"
teaser: reaction.jpg
header:
  overlay_image: reaction.jpg
  overlay_filter: 0.5
---

## Foreword

**We're not taught how to write code at Makers Academy.** Sure we learn a whole load of syntax, paradigms, best practices and design patterns. But that's not really what we're here to learn. What we actually learn is how to learn. In the context of Makers, the end goal is to be able to approach a language/topic/concept we've never seen before and quickly get comfortable applying it.

I've always been the jump in at the deep end kind of guy, so this weekend I decided to test my skills and look at React. To skip to the punchline:

- I Love React. It's an incredibly powerful tool that feels intuitively right to work in.
- Makers Academy has done what it set out to achieve. I was able to pick up a new framework and have built a fairly descent single page application within a single weekend.

## Introduction

Any good story needs a good introduction. This is not a good story. But before we begin, I just want to level the playing field and make sure everyone is onboard with what React is.

React is a library written in Javascript that allows you to easily create interactive user interfaces. It was originally built by Facebook and Instagram to power their front-ends, but is now open-source. React was created to address the issues associated with building large applications with data that changes over time.

[Andrew Ray has a great introductory blog post on React if you want to understand more about it before diving into the code.](http://blog.andrewray.me/reactjs-for-stupid-people/)

## Chapter One - Setting Up The Story

There are some excellent guides out there for setting up a new React project. [I'd recommend this article on code mentor](https://www.codementor.io/reactjs/tutorial/beginner-guide-setup-reactjs-environment-npm-babel-6-webpack), but there are many more out there. You'll need NPM installed, but otherwise a lot of the rest are optional extras. Things like JSX (used for having inline HTML in Javascript functions) aren't strictly needed but will make your life much easier.

While we're talking setup, let me quickly mention React DevTools. This is an add-on for chrome that adds another tab to the chrome DevTools. For those that are unaware, right-clicking on the page in chrome gives you an option to inspect the page. This brings up a console with lots of options for interacting with the underlying HTML/CSS/JS.

Installing the React DevTools adds another tab which lets you interact with the React that is rendering the page. It also displays components state and props which we'll explore later on.

[You can install them here:](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en). Once you have them, I'd recommend heading over to a Facebook/Instagram and having a play around before we begin.


## Chapter Two - Meeting the Main Characters

Alright, we're ready to begin. The corner stone of any React application is its components. Components are custom HTML blocks which we can add data to and render onto the domain page at a time of our choosing.

Lets look at a really simple component. A component that will just output a H3 header to let the user know something wasn't found.

```javascript
import React from 'react';

class NotFound extends React.Component {
  render(){
    return(
      <h3>Not Found</h3>
    )
  }
}

export default NotFound;
```

Components are classes which extend the React.Component class. You can add as many methods as you like to a component but every component needs to have at least one method; the `render` method. This is the method that tells React how to insert the component into the page.

In this example we're using JSX, which is a preprocessor step that allows us to add XML syntax to JavaScript. For our purposes this means we can do inline HTML in our classes. While you can use React without JSX they work really nicely together.

When we want to render this component on the page we simply call it as a HTML tag: `<NotFound/>` which will render out `<h3> Not Found </h3>` to our page.

Clearly this is a bit of a boring component, but the same logic applies as the complexity scales.

## Chapter Three - Mad Props

Alright we can build some basic components. But if we think of a few examples of built in HTML elements, there's something we're missing:

```HTML
<a href="link"></a>
<img src="source" alt="alternative text" />
<form class="form_class" action="index.html" method="post"></form>
```

All these tags take in additional information supplied as arguments in the tag. In React we can give a similar thing to our own elements. We call any information passed down to an element it's props. Lets say we want to create a form element that takes arguments for its default values:

```javascript
import React from 'react';

class AddItemForm extends React.Component {
  render(){
    return(
      <form className="form-class">
        <input type="text" value={this.props.desc}/>
        <input type="text" value={this.props.price}/>
        <button type="submit">Add Item</button>
      </form>
    )
  }
}

export default AddItemForm;
```

We access the data passed down through `this.props...`. Then when we want to render the element we simply pass down the data as arguments to our element:

```HTML
<AddItemForm desc="A Beautiful Vase" price=100/>
```

It's not just data we can pass down though. As functions in JavaScript are objects we can pass them down as well. This is useful in the next chapter when we look into state.

## Chapter Four - On Cloud Nine

Lets talk about data and state next. It's all very well having pretty elements on the page, but the real power of React lives in its ability to dynamically update the page when it receives updated data.

Data in React is called state. We can assign state to any of our components and it will persist. I've found it a good idea to keep you state in a single component to make dynamic rendering much easier. Say we have an application component that is responsible for rendering our single page app. We add state to out components via its constructor. Let's add an order state which will hold the current items in our order:

```javascript
class App extends React.Component {
  constructor(){
    super();
    this.state = {
      order: {}
    };
  }
}
```

However, given that our app runs entirely in the clients browser, we have no way to persist the order. We'll have to make use of an AJAX call to save our data. While we could use our own API, lets instead make use of Firebase. Firebase is a cloud database provided by Google. We can save our data straight from our front-end to the cloud.

To link our state up to the cloud we use the Rebase package. First we create a base component which holds our connection details:

```javascript
import Rebase from 're-base';

const base = Rebase.createClass({
  apiKey: //Your Key,
    authDomain: //Your Domain,
    databaseURL: //Your Database URL,
});

export default base;
```

Then whenever we want to save we can just use `base.syncState`. In the snippet below we've used the `componentWillMount` event hook which is called just before the component is rendered. We give it an empty render function as every element needs one.

```javascript
class App extends React.Component {
  constructor(){
    super();
    this.state = {
      order: {}
    };
  }

  componentWillMount(){
    this.ref = base.syncState(`${this.props.params.user_ID}/order`
      ,{
      context: this,
      state: 'order'
    });
  }

  render(){
    return ()
  }
}
```

So we have state saved in our application that is synced up to a remote database. But what happens when we want a component to make changes to this state? Lets say we have a component with a button that adds an item to our order:

```javascript
import React from 'react';

class AddItemToOrderForm extends React.Component {
  render(){
    return(
      <li className="item">
        <h2>{this.props.desc}</h2>
        <h3>{this.props.price}</h3>
        <button onClick=???</button>
        </li>
    )
  }
}

export default AddItemToOrderForm;
```
We get our item price and description from the props passed in. But what do we add to the buttons onClick method. It needs to take the information from the form and pass it back up to the Application to be saved in the state.

The answer lies in our ability to pass around functions as props. In our application component we will define the method that adds the data into state, and then pass it down to the rendered form:

```javascript
class App extends React.Component {
  constructor(){
    super();
    this.addToOrder = this.addToOrder.bind(this);
    this.state = {
      order: {}
    };
  }

  // componentWillMount(){
  //   this.ref = base.syncState(`${this.props.params.user_ID}/order`
  //     ,{
  //     context: this,
  //     state: 'order'
  //   });
  // }

  addToOrder(item){
    const order = {...this.state.order};
    const timestamp = Date.now();
    order[`item-${timestamp}`] = item;
    this.setState({order})
  }

  render() {
    return (
      <AddItemToOrderForm addToOrder={this.addToOrder} />
    )
  }
}
```

This is quite involved lets so lets look at each step in turn:

1- We create a function that adds a given item to the order state. Because this all takes place in the clients browser, we need to be careful about preserving state correctly. It's therefore considered good practice to take a copy of the current state which is what we do with `{...this.state.order}`. Once we have our copy, we add in a timestamped copy of the new item and then set the state equal to our new state object.

2- Because of the implementation of React (meaning for reasons I haven't fully comprehended!); if you define custom methods within a component they are not bound to the component by default. Instead we need to use the `bind` method, which we do in the constructor.

3- We then pass the method we defined down to the AddItemToOrderForm component, so we can access it in the props.

We are now in a position to complete our component:

```javascript
import React from 'react';

class AddItemToOrderForm extends React.Component {
  render(){
    return(
      <li className="item">
        <h2>{this.props.desc}</h2>
        <h3>{this.props.price}</h3>
        <button onClick={()=>this.props.addToOrder(this.props.desc)}</button>
        </li>
    )
  }
}

export default AddItemToOrderForm;
```

We can access the method we defined on the app component here, and we pass it the items description to be added to the state object! This pattern of defining functions, and then passing them down via props is really common based on what I've seen in React so far.

## Epilogue

We now have everything we need to build a simple one page React App. A great example of such an app is the subject of Wes Bos, React for Beginner tutorial. [It's hosted  here](http://catchoftheday.wesbos.com/).

It's a great exercise for anyone interested to install the React DevTools and go have a play around. There is nothing more complicated that the features we looked at above, but when brought together it makes for a clean user interface.

Notice how the page doesn't refresh between requests. React has a built in router that allows the app to handle browser redirects on the client machine. This allows for true single page applications.

I've loved what I've seen of React so far. Sure it has it's idiosyncrasies like any programming language, and no doubt frustrations as you delve deeper in. But the system of building of components makes sense, and the power of having a single source of truth for all the data in your app can't be overstated.

I'm looking forward to diving deeper into this powerful framework and seeing just what I can use it for!
