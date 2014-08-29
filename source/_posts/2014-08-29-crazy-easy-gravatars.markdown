---
layout: post
title: "Crazy Easy Gravatars"
date: 2014-08-29 14:24:22 -0400
comments: true
categories:['gem', 'gravtastic', 'ruby', 'rails']
keywords: gem, Iron Yard, ruby, rails, andrew, house, junior, rails, developer, engineer, dev
description: "The Gem Gravtastic"
---
[Gravtastic](https://github.com/chrislloyd/gravtastic) saves a ton of headaches when wanting a gravatar.<br>
In 4 lines of code (seriously 4) I had a gravatar up and running on my app.
Here's how. <br>

<h3>Getting Started</h3>
Keep in mind, gravtastic relies on Devise to work. So all of the instructions are
to be used after Devise is setup.<br>
Go ahead and add the gem gravtastic to your Gemfile.
{% codeblock Gemfile lang:ruby https://github.com/chrislloyd/gravtastic %}
gem 'gravtastic'
{% endcodeblock %}
<br>
As always, make sure to bundle after installing a gem.
{% codeblock Terminal lang:console https://github.com/chrislloyd/gravtastic %}
bundle
{% endcodeblock %}
<br>
<h3>Implementation</h3>
Now we need to make sure our model uses gravtastic. Your file may be different
depending on what you named your devise model.
I'm going to name mine User and generate the code on that premise.
{% codeblock app/models/user.rb lang:ruby https://github.com/chrislloyd/gravtastic %}
class User < ActiveRecord::Base
  include Gravtastic
  gravtastic
  ...
end
{% endcodeblock %}
<br>
With that gravtastic has been able to hook into your devise model. Hooray!
The last job is rendering the gravatar to the view.<br>
I'm using Haml, so the html syntax may be weird. If you're using Erb, just
replace the = with <%= and end the line with %>

{% codeblock app/views/whicheverview.html.haml lang:haml https://github.com/chrislloyd/gravtastic %}
=image_tag current_user.gravatar_url
{% endcodeblock %}
<br>
That's it!<br>
Now a gravatar should be rendering on your page.
If you want to adjust the size you can pass a size option on gravtastic in the model.
The specifics of different options to use can be found on
the [Gravtastic Github Page](https://github.com/chrislloyd/gravtastic)
