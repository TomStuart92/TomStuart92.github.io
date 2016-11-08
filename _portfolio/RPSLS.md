---
title: "Rock Paper Scissors "
excerpt: "Single and multiplayer game built in the Sinatra framework."
layout: single
author_profile: false
share: true
header:
  teaser: rps_single.png
sidebar:
  - image: rps_single.png
    image_alt: "logo"
  - github:
    - github_link: "https://github.com/TomStuart92/rps-challenge"
      github_name: "Github Repository"
  - examples:
    - example_link: "https://stark-shelf-40156.herokuapp.com/"
      example_name: "Heroku Deployed"
      example_class: "fa fa-fw fa-xing"
  - title: "Technologies"
    text: "Sinatra, Capybara/RSpec, CSS"
  - title: "Background"
    text: "Week Three Makers Academy Challenge"
gallery:
  - url: rps_home.png
    image_path: rps_home.png
  - url: rps_single.png
    image_path: rps_single.png
  - url: rps_multi.png
    image_path: rps_multi.png
---
{% include gallery caption="RPSLS Gallery." %}

As part of the Makers Academy course we were tasked with creating a Rock Paper Scissors game. This solution makes use of the Sinatra framework for a streamlined deployment process, and relies on Capybara/RSpec for testing.

The application supports both single player against a randomised opponent, or two player multiplayer. While outwardly simple, I have used this as an opportunity to focus on best practices including SOLID principles and using TDD to ensure good test coverage. The Model classes in particular are a nice example of limiting public interfaces, duck typing and generally keeping code DRY.

Overall, I'm happy with how the project turned out. The visual elements could use improving, perhaps by using bootstrap or similar. However I'm happy that the Back-End is sufficiently well developed for a week three project.

Moving forward I hope to build this into a truly multiplayer experience to allow for users to play each other over the internet. This will require me to explore Sinatra's use of websockets.
