---
title: Happy Face/Sad Face - A Tale of React
excerpt: "One mans story of building an application in React"
teaser: reaction.jpg
header:
  overlay_image: reaction.jpg
  overlay_filter: 0.5
---

## Foreword

**We're not taught how to write code at Makers Academy.** Sure we learn a whole load of syntax, paradigms, best practices and design patterns. But that's not really what we're here to learn. What we actually learn is how to learn. In the context of Makers, the real end goal is to be able to approach a language/topic/concept we've never seen before and quickly get comfortable applying it.

I've always been the jump in at the deep end kind of guy, so this weekend I decided to test my skills and look at React. To skip to the punchline:

- I Love React. It's an incredibly powerful tool that feels intuitively right to work in.
- Makers Academy has done what it set out to achieve. I was able to pick up a new framework and have built a fairly descent single page application within a single weekend.

## Introduction

Any good story needs a good introduction. This is not a good story. But before we begin, I just want to level the playing field and make sure everyone is onboard with what React is.

React is a library written in Javascript that allows you to easily create interactive user interfaces. It was originally built by Facebook and Instagram to power their front-ends, but is now open-source. React was created to address the issues associated with building large applications with data that changes over time.

[Andrew Ray has a great introductory blog post on React if you want to understand more about it before diving into the code.](http://blog.andrewray.me/reactjs-for-stupid-people/)

## Chapter One - Setting Up The Story

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

## Chapter Three - On Cloud Nine

Lets talk about data and state next. It's all very well having pretty elements on the page, but the real power of React lives in its ability to dynamically update the page when it receives updated data.

## Chapter Four - The Grand Finale

## Epilogue
