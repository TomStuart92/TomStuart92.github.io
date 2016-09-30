---
title: "Bowling Simulator"
excerpt: "A Bowling Simulator built in Javascript"
layout: single
author_profile: false
share: true
header:
  teaser: bowling_mid.png
sidebar:
  - title: "Technologies"
    image: bowling_mid.png
    image_alt: "logo"
    text: "Javascript, Jasmine, jQuery, HTML, CSS, Bootstrap-CSS"
  - title: "Background"
    text: "Week Four Makers Academy Challenge"
gallery:
  - url: bowling_new_game.png
    image_path: bowling_new_game.png
  - url: bowling_mid.png
    image_path: bowling_mid.png
  - url: bowling_finished.png
    image_path: bowling_finished.png
---

Project is hosted on Heroku - [Link](https://aqueous-reef-78045.herokuapp.com/)        
Github Repository - [Link](https://github.com/TomStuart92/bowling-challenge)

For the week five challenge we were tasked with writing a bowling scoring system in Javascript. This was the first challenge done entirely in JS. I used pure Javascript for the back end, Jasmine to test, and jQuery to build to bonus user interface.

My approach to the problem relied heavily on duck typing and polymorphism. In the end this solution, while elegant, placed too much responsibility on the frame classes. This responsibility would have sat better in the game class. However as an exercise in exploring OOD principles in Javascript, it was immensely useful.

{% include gallery caption="This is a sample gallery to go along with this case study." %}

Moving forward I'd like to restructure the program to make the game class responsible for calculating score. The front end would also benefit from the use of a dynamic framework like React to render the scores.
