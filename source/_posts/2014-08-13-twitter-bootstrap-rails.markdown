---
layout: post
title: "Twitter Bootstrap Rails"
date: 2014-08-13 17:38:02 -0400
comments: true
categories: ['rails', 'bootstrap']
keywords: ruby, rails, andrew, house, junior, rails, developer, engineer, dev
description: "How to use the Twitter-Bootstrap-Rails Gem"
---
Being entirely void of creativity, the [Twitter Bootstrap Rails Gem](https://github.com/seyhunak/twitter-bootstrap-rails)
is amazing.<br>
I use Twitter Bootstrap(bootstrap for short) to generate basic layouts so I
can get a handle on how things can possibly look.
Not to mention, it gives an amazing starting point for building good looking css.
If you visit the [Twitter Bootstrap Home Page](http://getbootstrap.com/components/),
you can see all of the styles you can create.
If you are a beginniner, ignore anything and everything Javascript related.
Stick primarily to the CSS and Components page and customize your page with
their features.
A little pizzazz goes a long way ya' know. <br>

<h3>Getting Started</h3>
I am assuming that you have a new rails project and are looking to add bootstrap
to that project.<br><br>
Let's start by adding the 'twitter-bootstrap-rails' gem to the Gemfile.
{% codeblock Gemfile lang:ruby https://github.com/seyhunak/twitter-bootstrap-rails %}
gem 'twitter-bootstrap-rails'
{% endcodeblock %}
<!-- more -->
<strong>ALWAYS</strong> After you update your Gemfile in any way, run bundler.
Now open up your terminal/iTerm/command prompt.
The rest of generating default bootstrap logic will be done from the terminal.
{% codeblock Terminal lang:bash https://github.com/seyhunak/twitter-bootstrap-rails %}
bundle install
{% endcodeblock %}
*Note: bundle is shorthand for bundle install.*<br><br>
Next, we need to generate the bootstrap into our Rails app.
{% codeblock Terminal lang:bash https://github.com/seyhunak/twitter-bootstrap-rails %}
rails generate bootstrap:install static
{% endcodeblock %}
*Note: g is shorthand for generate when Rails is the context.*
<h3>Generating the Default Layout Page</h3>
Now bootstrap has been installed into your Rails app and you can access bootstrap
logic if you want to completely customize your page.
However, bootstrap can help generate default layouts for your application.html page and
the generated views.<br>
In order to generate a default layout page, you need to run the following.
{% codeblock Terminal lang:bash https://github.com/seyhunak/twitter-bootstrap-rails %}
rails g bootstrap:layout application fluid -f
{% endcodeblock %}
*Note: You can run the code without the -f flag. All the flag will do is force the changes
so you don't have to repeatedly say yes to every little thing.*<br><br>
*Note: If you have installed haml or slim and your application file uses those tags,
then bootstrap will install the layout in that format. Cool right?*<br><br>
Now if you look at your application.html.erb(if you're using erb) you will notice
the drastic change to the html. Don't be afraid of all of the head logic.
Most of it is for mobile and meta data.
If you wanted to go ahead and customize your layout page, you can use
the css or components on the [Twitter Bootstrap Page](http://getbootstrap.com/components/).
If there are other types of bootstrap that you want to change.
For example, you hate the color of when you hover over your title on the navbar.
To make changes directly to the css go to *'./app/assets/stylesheets/bootstrap_and_overrides.css'*
and make your customizations there.<br>
<h3>Simple Form</h3>
If you are using [Simple Form](https://github.com/plataformatec/simple_form),
there is a bootstrap generator for simple form. If you are thinking about
using simple form, do so **before** installing the default views. If not,
the view forms will be installed in the default form format.
{% codeblock Gemfile lang:ruby https://github.com/plataformatec/simple_form %}
gem 'simple_form'
{% endcodeblock %}
*Note: Remember to bundle after changing the Gemfile.*<br>
{% codeblock Terminal lang:bash https://github.com/seyhunak/twitter-bootstrap-rails %}
bundle
rails g simple_form:install --bootstrap
{% endcodeblock %}
<h3>Generating Default Views</h3>
The synax for changing the layout of a set of views are.
{% codeblock Terminal lang:bash https://github.com/seyhunak/twitter-bootstrap-rails %}
rails g bootstrap:themed [RESOURCE_NAME]
{% endcodeblock %}
Resource name is the name of the *controller* of which you would like the views
changed. Let's say that you want to make a Posts controller.
{% codeblock Terminal lang:bash https://github.com/seyhunak/twitter-bootstrap-rails %}
rails g scaffold post title:string content:text
rake db:migrate
rails g bootstrap:themed posts -f
{% endcodeblock %}
*Note: The pluralization is critical.*<br><br>
*Note: Once again, the -f flag is just for saving precious time, it's not necessary.*<br><br>
Now if you look at the views under *'./app/views/posts'* the index and show will now be
in table format. The new and edit views will render the form page.
The form will have a great starting layout. <br>

<h3>Now the fun begins!</h3>
This is the true starting point for customizing your page however you want.
The basics have been done, now you don't have an ugly beginning point.
Once again, reference the [Twitter Bootstrap Page](http://getbootstrap.com/components/)
and make every little detail your own.
