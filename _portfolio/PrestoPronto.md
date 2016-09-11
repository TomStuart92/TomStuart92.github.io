---
title: "PrestoPronto"
excerpt: "A Twitter clone built in the Sinatra framework."
layout: single
author_profile: false
share: true
header:
  teaser: pp_home.png
sidebar:
  - title: "Role"
    image: pp_home.png
    image_alt: "logo"
    text: "Designer, Front-End and Back-End Developer"
  - title: "Background"
    text: "Week Four Makers Academy Challenge"
gallery:
  - url: pp_home.png
    image_path: pp_home.png
  - url: pp_sign_up.png
    image_path: pp_sign_up.png
  - url: pp_peeps.png
    image_path: pp_peeps.png
  - url: pp_make_peeps.png
    image_path: pp_make_peeps.png
  - url: pp_comment.png
    image_path: pp_comment.png
---

Project is hosted on Heroku - [Link](https://stark-shelf-40156.herokuapp.com/)        
Github Repository - [Link](https://github.com/TomStuart92/rps-challenge)

As part of the Makers Academy course we were tasked with creating a Rock Paper Scissors game. This solution makes use of the Sinatra framework for a streamlined deployment process, and relies on Capybara/RSpec for testing.

The application supports both single player against a randomised opponent, or two player multiplayer. While outwardly simple, I have used this as an opportunity to focus on best practices including SOLID principles and using TDD to ensure good test coverage. The Model classes in particular are a nice example of limiting public interfaces, duck typing and generally keeping code DRY.

{% include gallery caption="This is a sample gallery to go along with this case study." %}

Overall, I'm happy with how the project turned out. The visual elements could use improving, perhaps by using bootstrap or similar. However I'm happy that the Back-End is sufficiently well developed for a week three project.

Moving forward I hope to build this into a truly multiplayer experience to allow for users to play each other over the internet. This will require me to explore Sinatra's use of websockets.
