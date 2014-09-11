---
layout: post
title: "Security with Rails"
date: 2014-09-11 07:53:39 -0400
comments: true
categories: ['rails', 'security']
keywords: security, Iron Yard, ruby, rails, andrew, house, junior, rails, developer, engineer, dev
description: "A workshop I attended regarding security in Rails"
---
There is so much I don't know about security.<br>
Yet it is so fascinating.
Yesterday I attended a workshop hosted by [Nvisium](https://nvisium.com/), with the premise
of discussing vulnerabilities in Rails and how to avoid them.
It amazes me how much security professionals know.
In order to understand security, you need to know everything about
the framework you're using, as well as exactly how the web works.<br><br>
The workshop was led by [Ken Johnson](https://twitter.com/cktricky), the
CTO of Nvisium.
I won't go into the details of exactly what we did and what we covered.
Instead I'll focus on mainly what I gained from the workshop.
For me, Security has three faces.<br><br>
The first face is owned by the designers and those who develop the front
end.
For them, the only care for security is that it is there, but doesn't
change how they want to order and design the page. <br><br>
The second face is owned by the developers who want to get code to work.
I am guilty of this.
I generally attack code trying to get a feature to work, not really thinking
about how the parameter I have listed could be sql injected or could
give away a person's id to allow to try to narrow down who is admin
and try to attack that specific person.<br><br>
The last side lies on the ones who want to protect security.
For them, trying to convince the first two to use best security practices
must be an absolute headache.
To take a feature that looks the way the design team wants, and functions
the way the developers want, and tell them that they are vulnerable
to attacks.
It seems to me that they would always be the "bad" guy in that situation.
People care about security, as long as it doesn't effect their daily lives.<br><br>
I took a lot of notes and got a ton of resources from the workshop.
My plan is to try to do the best I can do make sure that I plug the big
holes of security in my apps.
I know that at my current state of being a junior developer that it would
be impossible to try and do everything, but I will take all the
necessary steps to prevent my app from being completely vulnerable.<br>
