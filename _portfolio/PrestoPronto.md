---
title: "PrestoPronto"
excerpt: "A Twitter clone built in the Sinatra framework."
layout: single
author_profile: false
share: true
header:
  teaser: pp_home.png
sidebar:
  - title: "Technologies"
    image: pp_home.png
    image_alt: "logo"
    text: "Sinatra, Capybara/RSpec, Bootstrap-CSS, CSS"
  - title: "Background"
    text: "Week Four Makers Academy Challenge"
gallery:
  - url: pp_home.png
    image_path: pp_home.png
  - url: pp_sign_up.png
    image_path: pp_sign_up.png
  - url: pp_peeps.png
    image_path: pp_peeps.png
  - url: pp_make_peep.png
    image_path: pp_make_peep.png
  - url: pp_comment.png
    image_path: pp_comment.png
---

Project is hosted on Heroku - [Link](https://PrestoPronto.herokuapp.com/)        
Github Repository - [Link](https://github.com/TomStuart92/chitter-challenge)

As part of the Makers Academy course we were tasked with creating a twitter clone. This solution makes use of the Sinatra framework for a streamlined deployment process, and relies on Capybara/RSpec for testing. Visual styling was improved using Sinatra-Bootstrap CSS and user authentication was achieved using the BCrypt Gem.

Stylistically, I was happy with the look achieved, given this was my first use of Bootstrap-CSS. The implementation of the home page was especially good.

On the backend, I spent time trying to improve the code to fall in line with best practices. The controllers and models are particularly skinny. I'm also pleased with the test coverage with helped me with to refactor quickly.

{% include gallery caption="This is a sample gallery to go along with this case study." %}

Moving forward I'd like to integrate picture uploads, most likely through use of AWS. If I find time, I would also implement a follow user system to more closely mimic twitters interface, allowing selective display of tweets based on who you are following.
