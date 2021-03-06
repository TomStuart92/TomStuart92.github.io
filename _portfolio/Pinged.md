---
title: "Pinged"
excerpt: "A Gmail clone built in React and Firebase."
layout: single
author_profile: false
share: true
header:
  teaser: pinged_inbox.png
sidebar:
  - image: pinged_inbox.png
    image_alt: "logo"
  - github:
    - github_link: "http://github.com/TomStuart92/email_react_app"
      github_name: "Github Repository"
  - examples:
    - example_link: "http://gentle-stream-18629.herokuapp.com"
      example_name: "Heroku Deployed"
      example_class: "fa fa-fw fa-xing"
  - title: "Technologies"
    text: "React, Firebase, Material Design Lite"
  - title: "Background"
    text: "A personal project to build a React application"
gallery:
  - url: pinged_inbox.png
    image_path: pinged_inbox.png
  - url: pinged_sent.png
    image_path: pinged_sent.png
  - url: pinged_create.png
    image_path: pinged_create.png
---
{% include gallery caption="Pinged Gallery." %}

I've written about the technical aspects of this project [here](https://tomstuart92.github.io/ReactJS/).

Having worked through Wes Bos' React for Beginners tutorial (An Example of the finished product is [here](http://catchoftheday.wesbos.com/store/helpless-grumpy-phenomena)), I was inspired to create something on my own. I decided to try to rebuild a single page email application, named Pinged.

Based on Google Inbox and using MDL styling, this inbox dynamically pulls data in from Firebase, and saves outbound emails the same way. I'm really happy with how it turned out, and while there is still a lot of work to do to get it to the quality I'd like, it helped to reinforce all that I've learned about React so far.
