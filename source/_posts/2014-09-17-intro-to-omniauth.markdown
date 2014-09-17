---
layout: post
title: "Intro to Omniauth"
date: 2014-09-17 09:01:26 -0400
comments: true
categories: [gems, rails]
keywords: gems, Iron Yard, ruby, rails, andrew, house, junior, rails, developer, engineer, dev
description: "Introduction to Omniauth"
---
It is becoming a trend for websites to enable logging into that website
via another.<br>
We've all seen it.
The buttons that say "Sign in with Facebook", or various other
providers.
For some reason unknown to me (really unknown to me because I don't
use social media), seeing these logins provide some kind of weird
validity to a website.
It's like "Oh, they are connected with facebook, they must be credible".
When I know perfectly well that its just a little bit of code hooking into
the facebook api.
Regardless, implementing these login features for a user is a
great strategy from a user experience (UX) standpoint.<br><br>
Over the next few blog posts I'm going to go over step by step
using provider authentication, and being able to use multiple
providers for the same user.
I feel logging into various providers is a valuable skill for every Rails developer, and
a great tool to be able to use.<br>
In the upcoming posts, I'll use Amazon and Google as my examples.
I'll also detail what is needed to add another provider, and some quirks
about certain ones (Twitter).
