---
layout: post
title: "Pry-ing Apart Code"
date: 2014-08-05 20:36:37 -0400
comments: true
categories:
---

I like to think of writing code similar to designing puzzles.
The more simple the code, the easier to dissect each individual element and
know the code with exact precision.
However, I have a fondness for diving deep into code I don't understand.
One of my favorite things to do is to find a method on ruby-docs that I know
nothing about, and pick it apart piece by piece.
Originally, I used mass amounts of puts and tabbing back and forth between
my editor and irb. During my first day at the IronYard my Instructor James
introduced me to Pry. <br><br>
Let me say, pry is a dream. <br><br>
Pry is pretty sweet because I'm able to use pry in place of irb.
It also uses a colorized
text to help me instantly recognize what the type the presented code is.
For me, the most powerful feature of pry is <code>binding.pry</code>.
When I'm wanting to find out what a particular piece of code is doing
at a specific point in time, I drop <code>binding.pry</code> at that specific
line. Then when I run the code, pry automatically opens and I'm given the ability
to check specific states of code, even during complicated loops. 
In addition, I like to use the pry-byebug gem to be able to go line by line,
continuously being able to check code and debug/figure out whats going on.<br><br>
As a Junior Dev I find these practices to be crucial.
There are plenty of times I have no idea what I'm doing, and by calmly
dissecting the code, I can pull myself through even the hardest bits of code.
