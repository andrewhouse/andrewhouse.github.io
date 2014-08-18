---
layout: post
title: "Homework App"
date: 2014-08-18 15:40:16 -0400
comments: true
categories: ['rails', 'ruby', 'heroku']
keywords: gem, figaro, ruby, rails, andrew, house, junior, rails, developer, engineer, dev
description: 'Building a Homework App'
---
I built an app which some of my fellow students can use as a medium to give
progress on their homework assignments.<br>
[My App](tiy-homework.herokuapp.com) uses Devise for registering users,
Heroku as the means for depoloyment, and a vast array of fun gems.
The whole point of the app is that there is a central place where our teacher
(James Dabbs) can check our homework progress.
James is the only authorized user (admin) capable of making new assignments.
Then each student can update the status of their homework by selecting a created
assignment, post the link of their website, and check to see if the assignment
has been completed.<br><br>
I think it works pretty well, all of the CSS was done with the help of
[Twitter Bootstrap](http://getbootstrap.com/). If you are interested in
checking out my code, I have it posted on [Github](https://github.com/andrewhouse/TIY-Homework-Checker).
<br>
But yea, making apps are fun. I had a lot of challenges in this app.
Something different I did in this that I had never done before was implementing
a has_many_through relationship.
It required me to do quite a bit of reading to see how each piece interacted,
but in the end it was totally worth it. 
