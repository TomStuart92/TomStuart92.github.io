---
title: Cryptic Madness
excerpt: "In this blog post we explore some basic cryptology."
---
## Introduction

The following blog post probably contains a whole load of mistakes, mistruths and general misinformation.

So through an acquaintance, I have stumbled upon the Cryptopals Crypto Challenges: [Link](http://cryptopals.com/).

This is a series of 48 exercises that demonstrate attacks on real-world crypto. These sorts of exercises have always fascinated me since I took a cryptology unit at university. That however, was a theoretical exploration of the mathematical principals behind common crypto-systems.

These challenges are an entirely different kettle of fish. They are entirely practical, asking you to exploit very real vulnerabilities that can be found in modern systems.

I've found them particularly challenging to date, because of my inexperience with both programming and cryptology. So I'm writing this blog post, as in my view, there's no better way to improve your understanding of a topic then to talk to someone about it. Even if its just the vacuum of the internet.

So far I've solved the first five problems of the first set, but lets look at the first two in a bit of detail.

## Problem One - Convert hex to base64

**Problem:** Given a Hexadecimal string, convert it to Base64.

**Background:** Numbers. Numbers. Numbers. The numbers we're all familiar with, the good old 0-9, are whats called base 10. When you get to 9, you repeat again 10-19, then 20-29 and so on. This is so familiar to humans that we mostly can't imagine counting any other way. This probably has something to do with ten fingers, but thats for another time.

However, if we think about it there's no reason that we need to stop at ten. We could reset at any arbitrary number. There are a whole range of these different systems using different bases:
- Base 2 [Binary] - 0..1 then reset.
- Base 8 [Octal] - 0..7 then reset.
- Base 16 [Hexadecimal] - 0..F then reset.
- Base 64 [Base 64] - 0..63 then reset

The observant will notice that Hexadecimal uses letters to 'count' the numbers greater than ten. So to solve our problem we need to convert a number from Base 16 to Base 64. Luckily ruby gives us a number of built in operators to deal with such problems.  

**Solution:**
We need the base64 module for this, so we include it first. Our first task is to convert out hex string to a binary string, which we do using ruby's pack method. The 'H*' argument tells it to use Hexadecimal encoding. We then take this binary string and pass it to the Base64 module, with the instruction to encode it.

```ruby
require "base64"

hex = '49276d206b696c6c696e6720796f757220627261696e206c696b65206120706f69736f6e6f7573206d757368726f6f6d'

string_to_ascii = [string].pack('H*')

output = Base64.strict_encode64(string_to_ascii)

output = 'SSdtIGtpbGxpbmcgeW91ciBicmFpbiBsaWtlIGEgcG9pc29ub3VzIG11c2hyb29t'
```

## Problem Two - Fixed XOR

**Problem:** Write a function that takes two equal-length buffers and produces their XOR combination.

**Background:** So for this challenge we're given two hex encoded strings. We need to decode them, but luckily we know how to do this from exercise one. So then we need to produce their XOR combination.

So what is XOR, I hear you ask...

Well lets take a step back, and look at two other operators: and/or.

We use these operators to combine two values based on their truthfulness.
When we use the and operator, we return 'true' if both are 'true', or 'false' otherwise.
When we use the or operator, we return 'false' if both are 'false' or 'true' otherwise.

AND - `&&`:

|Combination|Result|
| --- | --- |
| TRUE && TRUE | TRUE |
| TRUE && FALSE | FALSE |
| FALSE && TRUE | FALSE |
| FALSE && FALSE | FALSE |  


OR - `||`:

|Combination|Result|
| --- | --- |
| TRUE || TRUE | TRUE |
| TRUE || FALSE | TRUE |
| FALSE || TRUE | TRUE |
| FALSE || FALSE | FALSE |   

We call these tables truth tables. Now what about XOR. Well XOR stands for exclusive or.
When we use the exclusive or operator, we return true if the inputs are different:

OR - `^`:

|Combination|Result|
| --- | --- |
| TRUE ^ TRUE | FALSE |
| TRUE ^ FALSE | TRUE |
| FALSE ^ TRUE | TRUE |
| FALSE ^ FALSE | FALSE |   

Now we can also use these operators to combine two binary strings. Looking at each bit in turn we can treat 1 as TRUE, and 0 as FALSE. Lets encode two letters using our three operators:

A = 01000001
C = 01000011

AND - `&&`:

01000001   
01000011   
========     
01000001   

OR - `||`:

01000001   
01000011   
========      
01000011   

XOR - `^`:

01000001   
01000011   
========      
00000010

Decoding to ASCII gives us:

AND - 'A'  
OR - 'C'  
XOR - '\x02'  

So encoding with AND, and OR just gave us back the original plaintext. But using the XOR operator gave us a new character, which happens to be a hexadecimal character. More generally we use the XOR operator because it gives us an equal mixture when combining two random strings.
AND returns 75% False, 25% True.
OR returns 25% False, 75% True.
XOR returns 50% False, 50% True.
This makes it better for obscuring the original distributions.

**Solution:**

So to our solution. We decode the strings, as before. We then break each into their binary representations using unpack('B*'). We then zip each bit of both into a single array and then combine each bit using the XOR operator. Finally we re-encode to ascii (to see a nice english plaintext string), then to hexadecimal.

```ruby
input = '1c0111001f010100061a024b53535009181c'
fixed_string = '686974207468652062756c6c277320657965'

output = '746865206b696420646f6e277420706c6179'

input_decimal = [input].pack('H*')
fixed_decimal = [fixed_string].pack('H*')

xor = input_decimal.unpack('B*')[0].chars.zip(fixed_decimal.unpack('B*')[0].chars)

xor = xor.map do |c1, c2|
  (c1.to_i ^ c2.to_i)
end

p xor_string = xor.join   #01110100011010000110010100100000011010110...
p ascii_string = [xor_string].pack('B*') #"the kid don't play"
p hex_string = ascii_string.unpack('H*')[0] #746865206b696420646f6e277420706c6179
p output #746865206b696420646f6e277420706c6179

```
