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

To kick of the day, I headed to a session on using the Z3 library in Ruby. The Z# library is a Microsoft library for solving problems whose solutions can be easily constrained. For example in sudoku we have a bunch of rules that constrain the possible solutions.\


The convener of the session, wrote the package bindings in Ruby and was able to really show us the power of this package. We worked through a couple of examples, from (Sudoku)[https://github.com/taw/z3/blob/master/examples/sudoku] to a (RegExp crossword)[https://github.com/taw/z3/blob/master/examples/regexp_solver], the latter really testing the power of the library.

The code itself is really interesting. The basic premise is to translate the problem into a mathematical problem for which we can describe a series of constraints. The library is then able to work its magic and return us a solution.

For example, to solve a Sudoku puzzle:

```ruby
require "pathname"
require_relative "../lib/z3"

class SudokuSolver
  def initialize(path)
    data = Pathname(path).read
    data = data.strip.split("\n").map do |line|
      line.split.map{|c| c == "_" ? nil : c.to_i}
    end
    @data = data
    @solver = Z3::Solver.new
  end

  def solve!
    @cells = (0..8).map do |j|
      (0..8).map do |i|
        cell_var(@data[j][i], i, j)
      end
    end

    @cells.each do |row|
      @solver.assert Z3.Distinct(*row)
    end
    @cells.transpose.each do |column|
      @solver.assert Z3.Distinct(*column)
    end
    @cells.each_slice(3) do |rows|
      rows.transpose.each_slice(3) do |square|
        @solver.assert Z3.Distinct(*square.flatten)
      end
    end

    if @solver.satisfiable?
      @model = @solver.model
      print_answer!
    else
      puts "failed to solve"
    end
  end

  private

  def cell_var(cell, i, j)
    v = Z3.Int("cell[#{i+1},#{j+1}]")
    @solver.assert v >= 1
    @solver.assert v <= 9
    @solver.assert v == cell if cell != nil
    v
  end

  def print_answer!
    @cells.each do |row|
      puts row.map{|v| @model[v]}.join(" ")
    end
  end
end

path = ARGV[0] || Pathname(__dir__) + "sudoku-1.txt"
SudokuSolver.new(path).solve!
```

This code has multiple assertions that make up the rules of sudoku:

- Each number must be > 1 and < 9.   
- If a cell has a number in at the start, that number must remain the same.   
- Each row and column must have the numbers 1-9 uniquely   
- Each box of nine numbers must have the numbers 1-9 uniquely   

Once we've set out these assertions we simply ask the package to find us a solution, which is dutifully does.

An hour really isn't enough time to explore any package in sufficient depth, but from what I've seen, this library seems like a really nice way to solve a specific subset of problems. It's surprisingly nippy, even with complex problem spaces. The difficulty lies in problems where the constraints are difficult to describe or indeed unknown. Still a really interesting start to the day!

## Session Two - Machine Learning

On to session two - Machine Learning, a topic whose name is thrown around so much at the moment it's almost lost meaning. I headed into this session with little more than a very basic understanding of what Machine Learning was.

Over the course of the next hour I was witness to an interesting discussion that meandered its way across the sea of Machine Learning like a ship with no captain. While the content was inherently interesting the lack of leadership, meant that there was no clear focus for the discussion and so we ended up touching on a lot of topics that for a beginner was more overwhelming than

Don't get me wrong, this was a really useful hour for finding some jumping off points to look into this deeper. But I think of all the talks, this one showed the pros and cons of the (Un)Conference format the clearest.

For those with a grounding in a topic, the open forum style gives a real opportunity to really jump into a topic with like minded people. But for those with limited experience, the wealth and breath of opinions on an issue can be as confusing as it is enlightening.

## Session Three - Opal

Ok, on to my favourite of the three sessions, an exploration through another slightly obscure ruby package, Opal. This one was slightly amazing. Opal is a set of gems that compiles your Ruby code into javascript. What this means for practical purposes is that it makes it possible to write a full stack application, both front and back end, in pure Ruby. Let that sink in for a minute.

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
