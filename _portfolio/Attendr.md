---
title: "Attendr"
excerpt: "A events driven IOS dating app developed in Swift with a Node backend."
layout: single
author_profile: false
share: true
header:
  teaser: attendr.png
sidebar:
  - title: "Technologies"
    image: attendr.png
    image_alt: "logo"
    text: "Swift, xCode, Firebase, NodeJS, PostgreSQL"
  - title: "Background"
    text: "Makers Academy final project built over two weeks."
gallery:
  - url: attendr_loading.png
    image_path: attendr_loading.png
  - url: attendr_login.png
    image_path: attendr_login.png
  - url: attendr_events.png
    image_path: attendr_events.png
  - url: attendr_details.png
    image_path: attendr_details.png
  - url: attendr_matches.png
    image_path: attendr_matches.png
  - url: attendr_chat.png
    image_path: attendr_chat.png
---

Backend is hosted on Heroku - [Link](https://www.attendr-server.herokuapp.com)       
iOS App Demo - [Link](https://www.youtube.com/watch?v=a6o0Nbv4VA8)

Backend Github Repository - [Link](https://github.com/TomStuart92/attendr-server)   
Frontend Github Repository - [Link](https://github.com/TomStuart92/attendr)

For our final project we developed an events driven dating application in Swift. You can read my development diary for the project [here](https://tomstuart92.github.io/FinalProject/).

In two weeks we built a commercially viable MVP, in a totally new language and paradigm (Swift) as well as building the backend infrastructure needed to support it.

A simplified model of the application is shown below:

![App Model]({{ site.url }}/images/attendr_model.png)

The backend of the application was built across a NodeJs server hosted on Heroku. This server provided the brains of the application and was responsible for all business logic. It ran automated tasks to pull in events data from external sources, and stored all user details, event RSVPs and matches. The Node Server had the RESTful routes needed for the operation of our Application:

![App Routes]({{ site.url }}/images/attendr_server_routes.png)

The Node server was complemented by Firebase for the in app chat, allowing us to easily add realtime communication to our application.

The Frontend of the application was built in Swift, and is solely responsible for the presentation logic. While I personally would have liked to use React Native for this project, the team settled on Swift as it seemed to have a shallower learning curve than either React Native or Java.  

{% include gallery caption="Gallery" %}

Overall, the project was a really useful challenge to gauge how much we have learnt in the last twelve weeks. At the end of the day, I feel the quality of the work is pretty impressive given that we had two weeks to build the whole system in a environment and language we'd never used before. TDD in xCode remains a massive headache and it's something I'd want to face into head on the next time I take on a challenge like this. The quality of the code is also fairly low, and is in need of a good cleanup refactor, which is high on my list of things to do. 
