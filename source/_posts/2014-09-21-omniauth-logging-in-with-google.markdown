---
layout: post
title: "Omniauth - Logging in with Google"
date: 2014-09-21 08:42:44 -0400
comments: true
categories: ['gems', 'rails']
keywords: gems, Iron Yard, ruby, rails, andrew, house, junior, rails, developer, engineer, dev
description: "Logging into Google with Omniauth"
---
In this article I'll detail the process of logging into google via the
[Omniauth Gem](https://github.com/intridea/omniauth).<br>
I set up my code almost the same as how my teacher, [James Dabbs](https://github.com/jamesdabbs)
at The Iron yard. Also keep in mind that [devise](https://github.com/plataformatec/devise)
is needed to be setup before omniauth will work.<br>
<h3>Getting Set Up</h3>
The first thing we will do is add of the gems and gem dependencies
we're going to need for logging into google.
There are two gems that are going to be needed.
The first is the actual Omniauth Gem and the second can be found
[on the list of provider gems](gem "omniauth-google-oauth2")
that are supported with Omniauth.
I decided to use the [google-oath-2][https://github.com/zquestz/omniauth-google-oauth2]
gem.
{% codeblock Gemfile lang:ruby https://github.com/zquestz/omniauth-google-oauth2 %}
gem 'omniauth'
gem "omniauth-google-oauth2"
gem 'hashie'
{% endcodeblock %}
*Note: Always run bundle after changing your gemfile*<br>
*Note: I'll go into the purpose of Hashie in the identifier section*<br>
<!-- more -->
Next we will bundle our gems for use.
{% codeblock Terminal lang:console https://github.com/zquestz/omniauth-google-oauth2 %}
bundle
{% endcodeblock %}
*Note: bundle is shorthand for bundle install.*<br>
<h3>Configuring Devise</h3>
Next we need to configure devise to let it know that we are using
omniauth, and let it know where to put our API key and secret token.
{% codeblock ~/config/initializers/devise.rb lang:ruby https://github.com/zquestz/omniauth-google-oauth2 %}
config.omniauth :google_oauth2, ENV['GOOGLE_ID'], ENV['GOOGLE_SECRET']
{% endcodeblock %}
Note that in this case we are using environment variables.
It is not wise or safe to put any type of api keys, secrets, or passwords
live on the internet.
I suggest using the [Figaro Gem](http://iamandrewhouse.com/blog/2014/08/14/figaro/)
to store environment variables for easy deployment to heroku.
Now we actually need te data to store in the (in this example)
'GOOGLE_ID' and 'GOOGLE_SECRET'.<br>
<h3>Getting Key and Secret from Google</h3>
The [Google Console](https://console.developers.google.com) is the starting
point. Through here you go through the process of creating a new project.
I'm not going to go into the specifics of what to click because it is pretty
straight forward, but I will cover the crucial information that is
needed.<br>
Once the project is created, under the tab API's & auth select API's and
turn on Contacts API and Google+ API.<br>
Next, under the consent screen select an email-address and enter
a product name. <strong>THIS IS IMPORTANT</strong>.
The product name cannot be the same name as the project name (which
  can be seen on the top left corner of the console).<br>
Lastly we'll configure the credentials.
Since all I'm going to use is for development I'm going to put in
the url's as http://localhost:3000. If you're going to deploy to an online
website, add the same url's as I'm going to add, but replace
localhost:3000 with (for example) 'http://yourwebsitehere.com'.
Under Javascript Origins enter in http://localhost:3000.
Under Redirect URI's enter http://localhost:3000/users/auth/google_oauth2/callback.<br>
Keep in mind, the callback gave me a lot of trouble while setting up.
Various blogs never had the same callback.
This one worked for me because google told me to use it!
I got this information when I got to the point where I was trying to
login with google and on the 404 page it said it couldn't find
that url as a redirect uri.
If yours is different, add it onto the redirect uri list and prosper.<br>
Finally we can copy the Client ID as the GOOGLE_KEY and Client Secret
as GOOGLE_SECRET.
Again, see my [Figaro](http://iamandrewhouse.com/blog/2014/08/14/figaro/)
blog post onto how to add in the two.
<h3>Continuing Devise Setup</h3>
Now we need to setup the routes so devise knows how to handle the
omniauth callback requests.
{% codeblock ~/config/routes.rb lang:ruby https://github.com/zquestz/omniauth-google-oauth2 %}
devise_for :users, :controllers => { omniauth_callbacks: 'omniauth_callbacks' }
{% endcodeblock %}
*Note: My devise model is named Users, yours may be different.*<br>
As you can guess sometime soon we're going to make a controller called
omniauth_callbacks. We'll get to that soon, first we need to
let the devise model know that our app is "omniauthable".
{% codeblock ~/app/models/user.rb lang:ruby https://github.com/zquestz/omniauth-google-oauth2 %}
class User < ActiveRecord::Base
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable, :omniauthable
  ...
end
{% endcodeblock %}
*Note: Again, my model is user.rb, yours may be different.
Also, note the addition of omniauthable.*<br>
<h3>Configuring the Callback Controller</h3>
Now we'll go ahead and setup the omniauth callbacks controller.
This controller will inherit from the devise:omniauth callbacks controller.
Essentially, we're adding methods for each provider.
Each method is named after the provider.
In my implementation, I'm setting it up where it is easy to add
multiple providers, the only line that will need to be edited
after adding an additional provider is the array list of providers.
{% codeblock ~/app/controllers/omniauth_callbacks_controller.rb lang:ruby https://github.com/zquestz/omniauth-google-oauth2 %}
class OmniauthCallbacksController < Devise::OmniauthCallbacksController
  def self.provides_callback_for(provider)
    define_method provider do
      auth = env["omniauth.auth"]
      @user = Identifier.new(auth, current_user).resolve

      if @user.persisted?
        sign_in_and_redirect @user, event: :authentication
        set_flash_message(:notice, :success, kind: "#{provider}".capitalize) if is_navigational_format?
      else
        session["devise.#{provider}_data"] = auth
        redirect_to new_user_registration_url
      end
    end
  end


  [:google_oauth2].each { |provider| provides_callback_for provider }
  # ^ Here is where you add additional providers.
end
{% endcodeblock %}
One thing may stick out as being odd.
The Identifier.new hasn't been defined yet.
Next we'll create a service called just that with the purpose of
managing the creation of identities (a model which we will also set up).
The general idea is a User has many identities.
An identity being the providers themselves.
With this setup, a person can sign into google, amazon, or whatever
and maintain the user.
<h3>Creating the Identifier Service</h3>
{% codeblock ~/app/services/identifier.rb lang:ruby https://github.com/zquestz/omniauth-google-oauth2 %}
class Identifier
  def initialize auth, user=nil
    @auth = Hashie::Mash.new auth
    @user = user
  end

  def resolve
    identity = find_or_create_identity
    user     = ensure_user identity
    link identity, user
    user
  end

private

  def find_or_create_identity
    Identity.where(provider: @auth.provider, uid: @auth.uid).
      create_with(auth_data: @auth).
      first_or_create!
  end

  def ensure_user identity
    @user ||= identity.user
    return @user if @user

    @user = User.where(email: identity.email).
      create_with(
        name:      identity.name,
        password:  Devise.friendly_token[0,20]
      ).first_or_create!
  end

  def link identity, user
    identity.update_attribute :user, user unless identity.user == user
  end
end

{% endcodeblock %}
It would take a while to type out what each individual method is doing.
Hashie::Mash is from the hashie gem.
Essentially, it allows the use of the dot syntax from returned JSON
and not having to use the bracket syntax all the time.
You'll see a lot of talk of identity. It may feel awkward because
we haven't defined the Identity Model yet.
Take note of the resolve method which was called in the omniauth callbacks
controller.
On the creation of an Identifier we pass in the current_user and
the data generated from the provider to check and see if the
user exists with another provider and if that provider has already been
created. If it hasn't been created, it is and if not then it returns
the provider.
<h3>Creating the Identity Model with Relationships </h3>
Next we need to create the Identity Model and relate it to the User.
{% codeblock Terminal lang:console https://github.com/zquestz/omniauth-google-oauth2 %}
rails g model Identity user_id:integer provider uid auth_data:text
rake db:migrate
{% endcodeblock %}
Now that the model has been created and migrated, lets go ahead and
set up the relationships.
{% codeblock ~/app/models/user.rb lang:ruby https://github.com/zquestz/omniauth-google-oauth2 %}
class User < ActiveRecord::Base  
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable, :omniauthable
  has_many :identities
  ...
end

{% endcodeblock %}
*Note: This is for the User Model.*<br><br>
Now the Identity Model will have a bit more complexity.

{% codeblock ~/app/models/identity.rb lang:ruby https://github.com/zquestz/omniauth-google-oauth2 %}
class Identity < ActiveRecord::Base
  belongs_to :user
  validates_presence_of :uid, :provider
  validates_uniqueness_of :uid, :scope => :provider

  serialize :auth_data, JSON

  def auth
    Hashie::Mash.new auth_data
  end

  %w(name email image).each do |attr|
    define_method attr do
      auth.info.send(attr) || raise("Could not find #{attr} in #{auth}")
    end
  end
end
{% endcodeblock %}
There are a few things going on here.
The relationship connection, validations, serialization to json, and
creating methods.
Now we can call things like current_user.identities.first.provider
Which will return "google_oath2" if it was your first sign in (since
  identities returns an array).
Likewise, current_user.identities.first.auth_data will return all
of the data from google.
I can see keys such as first name, last name, email, images and more!
<h3>Calling the Sign in with Google Link</h3>
If you look in ~/apps/views/devise/shared/links.html.haml (I'm using haml).
There is this line of code at the bottom.
{% codeblock  ~/apps/views/devise/shared/links.html.haml lang:ruby https://github.com/zquestz/omniauth-google-oauth2 %}
- if devise_mapping.omniauthable?
  - resource_class.omniauth_providers.each do |provider|
    = link_to "Sign in with #{provider.to_s.titleize}", omniauth_authorize_path(resource_name, provider)
{% endcodeblock %}
If you click on the normal devise sign in route, you'll see that
sign in with google was automatically generated for you.
If you want to add your own custom link on a given page, use this link.
{% codeblock  ~/apps/views/yourview/index.html.haml lang:ruby https://github.com/zquestz/omniauth-google-oauth2 %}
= link_to "Sign in with Google", user_omniauth_authorize_path(:google_oauth2)
{% endcodeblock %}
<h3>Conclusion</h3>
With this lies the completion of connecting to google using the omniauth
gem.
The purpose of this tutorial was to allow the addition of other providers
with very few lines of code.
Lets say for example you were wanted to add facebook.
You would need to add the omniauth-facebook gem, add the config under
devise.rb, create the api key/secret from the facebook developers page,
setup the redirect uri callback from facebook,
log that information into application.yml (if you're using figaro), and
finally in the omniauth_callbacks controller add :facebook to the
array of providers.<br><br>
Something like facebook is easy to integrate with the current setup.
There are a couple of providers that aren't so easy.
Twitter and Linkedin don't return email addresses in their auth_data.
Since devise requires email I'm sure you can see the hassel.
Some strategies are to after clicking the sign in with twitter button,
have them fill out a quick form for email, or to randomly generate an
email for them.<br><br>
Either way, good luck and happy coding!
