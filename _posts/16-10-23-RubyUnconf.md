---
title: Ruby (Un)Conference 2016
excerpt: "Highlights of a day spent locked in a building full of Ruby coders"
teaser: unconf.png
header:
  overlay_image: unconf.png
  overlay_filter: 0.5
---
## So it's the Saturday morning after a big night...
The night before holds blurred memories of the average curry and one too many beers. You roll over and look at your clock. 8am. You roll over and try to sleep it off.

Well no, not if you're a Makers Student; Instead I and four of my fellow students spent our Saturday at the 2016 Ruby (Un)Conference. One of the things we're continually being told is that it's important in the programming world to be active in the community. It will make a huge difference when we come to finding jobs due to the connections we'll make; but also that its an excellent way to keep up to date in the ever moving world of programming.

So yes, no lie in for us. The day was spent exploring some of the more bizarre aspects of the ruby environment. It was a really worthwhile experience in the end, and I learned a tonne about things I never even new existed. So lets dive in and talk a little about what each of the sessions covered.

## What's an (Un)Conference?

But wait, I hear you shout. What is an (Un)Conference?

Well let me tell you dear reader! An (Un)Conference is like a Conference without speakers, a schedule or even a formal plan. Basically, everyone turns up in the morning and there's a group session where those who want to propose a session put forward an idea and schedule it. Anyone can put forward an idea on anything they want.

Once the sessions are decided on, your free to attend whatever sessions pique your interest, and indeed jump between sessions in the same block. At the end of the day then, its really a group of enthusiasts talking about whatever part of the topic they find most interesting.

At this Ruby (Un)Conference we had sessions on everything from the philosophical - 'Truth and Programming' to the bizarrely named - "Shaving the Architecture Yak" to the devils advocate - "Why bother speeding up your Rails app?".

## Session One - Solving Problems using the Z3 library



## Session Two - Machine Learning

## Session Three - Opal

Ok, on to my favourite of the three sessions. Again Tomas led us through another slightly obscure ruby package, Opal. This one was slightly amazing. Opal is a set of gems that compiles your Ruby code into javascript. What this means for practical purposes is that it makes it possible to write a full stack application, both front and back end, in pure Ruby. Let that sink in for a minute.

Now the is a very immature project. Don't start throwing away your javascript just yet. That being said though we saw examples of pure JS, jQuery and React front-end code written purely in Ruby.

[There's a really good walkthrough of an example of some opal based code here](https://www.sitepoint.com/opal-ruby-browser-basics/). The code is basically Ruby, albeit with some subtle differences. For example to create a grid object:

```ruby
require 'opal'
require 'opal-jquery'

class Grid
  attr_reader :height, :width, :canvas, :context, :max_x, :max_y

  CELL_HEIGHT = 15;
  CELL_WIDTH  = 15;

  def initialize
    @height  = `$(window).height()`                  
    @width   = `$(window).width()`                   
    @canvas  = `document.getElementById(#{canvas_id})`
    @context = `#{canvas}.getContext('2d')`
    @max_x   = (height / CELL_HEIGHT).floor           
    @max_y   = (width / CELL_WIDTH).floor             
  end

  def draw_canvas
    `#{canvas}.width  = #{width}`
    `#{canvas}.height = #{height}`

    x = 0.5
    until x >= width do
      `#{context}.moveTo(#{x}, 0)`
      `#{context}.lineTo(#{x}, #{height})`
      x += CELL_WIDTH
    end

    y = 0.5
    until y >= height do
      `#{context}.moveTo(0, #{y})`
      `#{context}.lineTo(#{width}, #{y})`
      y += CELL_HEIGHT
    end

    `#{context}.strokeStyle = "#eee"`
    `#{context}.stroke()`
  end

  def canvas_id
    'conwayCanvas'
  end

end

grid = Grid.new
grid.draw_canvas
```

The other aspect of this session which was really interesting was the discussion of the future of browser based code. While opinions were mixed, there was a general dislike for Javascript (though perhaps unsurprisingly given this was a Ruby Conference). However, the discussion of a future browser based assembly language or indeed that JS would just become a language that is rarely written in but compile into were quite thought provoking. It's definitely a space to watch!

## Thoughts

Overall, I had a great time at Ruby (Un)Conference 2016. The format of the day was both it's biggest strength and weakness. The openly convened sessions and freedom to pick and choose what to attend made for really interesting listening and a great dynamic in sessions. However, It did mean that for sessions without a clear purpose, the discussion tended to jump around never really getting to the core of the issue.

That said, it's something I'd recommend to add into your yearly schedule alongside the bigger formal conference circuit. I'll definitely be going next year!
