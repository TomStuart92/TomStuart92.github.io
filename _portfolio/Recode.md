---
title: "Recode"
excerpt: "A code analyser built in the Sinatra framework."
layout: single
author_profile: false
share: true
header:
  teaser: recode_home.png
sidebar:
  - title: "Technologies"
    image: recode_home.png
    image_alt: "logo"
    text: "Sinatra, Capybara/RSpec, Bootstrap-CSS, CSS"
  - title: "Background"
    text: "Week Four Makers Academy Challenge"
gallery:
  - url: recode_home.png
    image_path: recode_home.png
  - url: recode_repos.png
    image_path: recode_repos.png
  - url: recode_files.png
    image_path: recode_files.png
  - url: recode_Stats.png
    image_path: recode_Stats.png
  - url: recode_analysis.png
    image_path: recode_analysis.png
  - url: recode_code.png
    image_path: recode_code.png
---

Project is hosted on Heroku - [Link](https://recode-app.herokuapp.com/)*        
Github Repository - [Link](https://github.com/TomStuart92/recode)

* If you're testing the Heroku Project I suggest using my github username 'TomStuart92' and using the 'RecodeTest' repository which has a nice example file in, complete with a whole load of code smells.  

As part of the Makers Academy course we were tasked with creating development tool. My team settled on building a tool to help developers refactor there code.

{% include gallery caption="This is a sample gallery to go along with this case study." %}

It's integrated through the Github API, and allows you to run analysis for code smells and design anti-patterns on your project files. The current state of the project is very much at an MVP level, and is something I'd love to have a chance to look at further.

We wrote this project in Sinatra, which allowed us to take advantage of the dynamic paradigm of ruby. I'm particularly proud of the way the individual code analysers are built up, which makes the project very extendable.
