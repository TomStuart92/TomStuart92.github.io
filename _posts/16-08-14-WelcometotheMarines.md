---
title: Welcome to the Marines - Finishing the PreCourse.
excerpt: "A collection of thoughts, problems and reflections on the Makers Academy PreCourse."
header:
  overlay_image: bootcamp.jpg
  overlay_filter: 0.5
---

I once heard Makers Academy described as 'Like The Marines and Oxford combined, for Coding'. Well to this point there's been a little less shouting and perhaps a few less push-ups, but the rest stands true. It's been a mentally testing, and physically demanding but I've made it through the PreCourse.

For those that don't know, the PreCourse consists of four weeks of self learning before the start of the full twelve week Makers Academy course. Each of the four weeks has a specific focus:

- Week One - Introduction to the Command Line and Git  
- Week Two - Working through Chris Pine's Learn to Program   
- Week Three - More Ruby! Creating a student's directory  
- Week Four - Introduction to Test Driven Development

Each of the weeks builds on the previous weeks, which is something I think will continue into the main course. This sort of spiral learning is a great way to continually improve whilst reinforcing the concepts you've already learnt.

Overall, the content was challenging, but very doable. What was especially great was meeting up with my course mates at Google Campus several times each week. This was an excellent opportunity to compare approaches, practice pair programming, and look at any problems we may be having. It's also great to meet the people I'll be spending the next twelve weeks with.

I'm super excited to start the main course, and I hope I can keep this blog going through that. In the meantime though I'd thought I share a solution to a particular challenging problem that I really enjoyed solving.

## Criminal Programming - Guessing your Pin

So [codewars](www.codewars.com) is an incredible resource for anyone learning to code. Fundamentally its a database of hundreds of different problems for all sorts of different languages. You're given a brief description of the challenge and let loose to solve it as you wish.

Each challenge is graded for difficulty so it's a great place to start if you have very little experience. However the best part is that once you solve the problem, you get to see everyone else's solutions. This is a great way to see new ways to approach a problem.

One particular challenge consumed a fair amount of my time this week. The [kata](https://www.codewars.com/kata/5263c6999e0f40dee200059d) description is as follows:

```
Alright, detective, one of our colleagues successfully observed our target person, Robby the robber. We followed him to a secret warehouse, where we assume to find all the stolen stuff. The door to this warehouse is secured by an electronic combination lock. Unfortunately our spy isn't sure about the PIN he saw, when Robby entered it.

The keypad has the following layout:

┌───┬───┬───┐
│ 1 │ 2 │ 3 │
├───┼───┼───┤
│ 4 │ 5 │ 6 │
├───┼───┼───┤
│ 7 │ 8 │ 9 │
└───┼───┼───┘
    │ 0 │
    └───┘
He noted the PIN 1357, but he also said, it is possible that each of the digits he saw could actually be another adjacent digit (horizontally or vertically, but not diagonally). E.g. instead of the 1 it could also be the 2 or 4. And instead of the 5 it could also be the 2, 4, 6 or 8.

He also mentioned, he knows this kind of locks. You can enter an unlimited amount of wrong PINs, they never finally lock the system or sound the alarm. That's why we can try out all possible (*) variations.

* possible in sense of: the observed PIN itself and all variations considering the adjacent digits

Can you help us to find all those variations? It would be nice to have a function, that returns an array of all variations for an observed PIN with a length of 1 to 8 digits. We could name the function getPINs (get_pins in python). But please note that all PINs, the observed one and also the results, must be strings, because of potentially leading '0's. We already prepared some test cases for you.

Detective, we count on you!
```

Alright, seems quite straightforward right. We are given a pin number and just need to come up with all the possible combinations the number could be given a standard 0-9 keypad.

After playing around a little, I decided I'd need to store the possible replacement numbers for each number in a data structure. For example we need to know that 5 can be 2,4,5,6 or 8.

I settled on a hash with the keys as the input number and an array as the value for each key. It ended up looking a little like this:

```Ruby
combos = {
 "1" => ["1","2","4"],
 "2" => ["1","2","3","5"],
 "3" => ["2","3","6"],
 "4" => ["1","4","5","7"],
 "5" => ["2","4","5","6","8"],
 "6" => ["3","5","6","9"],
 "7" => ["4","7","8"],
 "8" => ["5","7","8","9","0"],
 "9" => ["6","8","9"],
 "0" => ["8","0"] }
```

We've had to use strings to represent our numbers, because a pin number needs to be a string as well to deal with leading zero problems. For example, the pin 0123 is acceptable, but if saved as a integer, ruby will see it as 123.

So whats the next step? Well we want to replace every digit in the pin code our detective thought he saw with all the other combinations.

The way I solved this was to break the pin we are given into each character using the  `String#split` method. We then have an array which holds each character as an element. Then we can use the `Array#map` method, to replace each element with its value in our combos hash:

```ruby
def get_pins(observed)
  combos = {
    "1" => ["1","2","4"],
    "2" => ["1","2","3","5"],
    "3" => ["2","3","6"],
    "4" => ["1","4","5","7"],
    "5" => ["2","4","5","6","8"],
    "6" => ["3","5","6","9"],
    "7" => ["4","7","8"],
    "8" => ["5","7","8","9","0"],
    "9" => ["6","8","9"],
    "0" => ["8","0"] }
  observed.split("").map{|x| combos[x]}
end
get_pins('1234') => [["1", "2", "4"], ["1", "2", "3", "5"], ["2", "3", "6"], ["1", "4", "5", "7"]]
```

As you can see, the first element of our new array has the values 1, 2 and 4 - the possible values of the key one. The same is true of the rest of the array.

So we've made headway. We now have an array which holds all the possibilities of each number in the pin. Now we just need to find a way to combine them. We need a method that will go through the array and give us every combination, i.e. 1121, 1124, 1125 and so on.

This is something I struggled with for a long time. However a memory of first year university maths ended up saving me. Suffice it to say, I didn't think that Matrix Multiplication would be useful in this problem but alas he we are. The `Array#product` method in ruby, allows us to combine two arrays and return the cartesian product. For those less mathematically inclined, all you need to know is that if we have two arrays it gives us the combination of the two:

```ruby
a = [1,2]
b = [3,4]
a.product(b) => [[1, 3], [1, 4], [2, 3], [2, 4]]
```

We don't just want to combine two arrays though, we have four; one for each number in our pin code. We can get around this by combining the product method to the `Array#reduce` method. This method allows us to reduce an array to a single item by applying an operation to each pair of elements in turn:

```ruby
 [5,6,7,8,9,10).reduce(:+) => 45
```
So if we use .reduce(&:product) we will reduce our arrays into an array that contains every possible combination of the pin code:

```ruby
def get_pins(observed)
  #continued from above
  observed.split("").map{|x| combos[x]}.reduce(&:product)
end

get_pins('1234') => [[[["1", "1"], "2"], "1"], [[["1", "1"], "2"], "4"], [[["1", "1"], "2"], "5"], [[["1", "1"], "2" ]....
```

 I've had to truncate the result, because it's rather long and complicated, but suffice it to say each block of four digits now represents a possible combination. However it's really hard to read, so the last thing we need to do it to join the numbers back together to give us our final pin codes. We can use `String#join` along with map again to do this:

```ruby
def get_pins(observed)
  #continued from above
  observed.split("").map{|x| combos[x]}.reduce(&:product).map(&:join)
end

get_pins('1234')
=> ["1121", "1124", "1125", "1127", "1131", "1134", "1135", "1137", "1161", "1164", "1165", "1167", "1221", "1224", "1225", "1227", "1231", "1234", "1235", "1237", "1261", "1264", "1265", "1267", "1321", "1324", "1325", "1327", "1331", "1334", "1335", "1337", "1361", "1364", "1365", "1367", "1521", "1524", "1525", "1527", "1531", "1534", "1535", "1537", "1561", "1564", "1565", "1567", "2121", "2124", "2125", "2127", "2131", "2134", "2135", "2137", "2161", "2164", "2165", "2167", "2221", "2224", "2225", "2227", "2231", "2234", "2235", "2237", "2261", "2264", "2265", "2267", "2321", "2324", "2325", "2327", "2331", "2334", "2335", "2337", "2361", "2364", "2365", "2367", "2521", "2524", "2525", "2527", "2531", "2534", "2535", "2537", "2561", "2564", "2565", "2567", "4121", "4124", "4125", "4127", "4131", "4134", "4135", "4137", "4161", "4164", "4165", "4167", "4221", "4224", "4225", "4227", "4231", "4234", "4235", "4237", "4261", "4264", "4265", "4267", "4321", "4324", "4325", "4327", "4331", "4334", "4335", "4337", "4361", "4364", "4365", "4367", "4521", "4524", "4525", "4527", "4531", "4534", "4535", "4537", "4561", "4564", "4565", "4567"]
```

We have our list of pins! Only 159 possible combinations to try. Shouldn't take too long at all.
