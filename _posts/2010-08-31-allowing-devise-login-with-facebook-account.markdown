---
layout: post
title:  "Allowing devise login with facebook account"
date:   2010-08-31
permalink: /posts/14-allowing-devise-login-with-facebook-account
categories: old
---

tags: Ruby on Rails, Authentication date: 2010-08-31 22:58:40.000000000Z

##Creating the application with devise authentication

The first thing we need to do is create a new application with devise authentication:

    rails new devise_facebook -d mysql

Devise already comes with oauth2 support, so we just need to put this gems in the Gemfile:

    gem 'devise', :git => "http://github.com/plataformatec/devise.git"
    gem 'oauth2' gem 'nifty-generators' #Just to create a basic layout

Now we just create the basic devise authentication.
Installing device inside the application:

    rails g devise:install

Creating the user authentication:

    rails g devise user

Making some routes:

    devise_for :users

Migrate the database:

    rake db:migrate

Your application should be working by now, if you go to localhost:3000/users/sign_up you can see it.

##Creating the Facebook application

Next step is create a facebook application so we can use to communicate with the rails application.
Go to <http://www.facebook.com/developers/createapp.php> and follow the steps:

**Application name:**

![](https://img.skitch.com/20110906-f6m9dd22jystqfkirf587ftjsc.jpg)

**Security check:**
![](https://img.skitch.com/20110906-r15i611112g4ywr93cs32rbrti.jpg)

**Your application:**
![](https://img.skitch.com/20110906-p219n8cjbfupypn8xgnhesic4h.jpg)

Your facebook application was created, in the next step we gonna use this Apllication ID, Secret and key to connect our rails application.

##Allowing devise to login with facebook account

Devise comes with OAuth2 support, but to use this you need the github devise version, that's why we put gem 'devise', :git => "http://github.com/plataformatec/devise.git" in our Gemfile.
If you never heard about oauth, here's a basic definition:

*OAuth 2.0 is the next evolution of the OAuth protocol which was originally created in late 2006. OAuth 2.0 focuses on client developer simplicity while providing specific authorization flows for web applications, desktop applications, mobile phones, and living room devices. This specification is being developed within the IETF OAuth WG and is based on the OAuth WRAP proposal.*

You can find more about oauth [here](http://oauth.net/).

The first thing to make devise works with facebook account is to add :oauthable in the devise model:

    devise :database_authenticatable, :registerable,
            :recoverable, :rememberable, :trackable, :validatable,:oauthable

Next we need to specify the facebook application details in our config/initializers/devise.rb:

    config.oauth :facebook, 'APP_ID', 'APP_SECRET',
      :site => 'https://graph.facebook.com/',
      :authorize_path => '/oauth/authorize',
      :access_token_path => '/oauth/access_token',
      :scope => %w(email)

##A little explanation about the configuration:

**APP_ID** is the id of the application created in the second step. Facebook uses this ID to find your application.

**APP_SECRET** is the secret of the application created in the second step.

**site** is the graph.facebook.com, responsible for the authentication process of the facebook account.

**authorize_path and acces_token_path** is the urls for the the authentication process.

**scope** is basically the information that we want from facebook, by default facebook returns just some basic information about the user, but we need the email to create the devise account.

When using devise and oauth, devise creates a "/users/oauth/facebook/callback" in your application to get the response from facebook, we just need to create a find_for_facebook_oauth in the User model.

    def self.find_for_facebook_oauth(access_token, signed_in_resource=nil)
      data = ActiveSupport::JSON.decode(access_token.get('https://graph.facebook.com/me?'))
      if user = User.find_by_email(data["email"])
        user
      else # Create an user with a stub password.
        User.create!(:email => data["email"], :password => Devise.friendly_token[0,20])
      end
    end

This method will decode the JSON response from graph.facebook.com containing the authenticated user information and try to find a user in the database with the
Almost there, we just need to create a link to the facebook authentication and devise provide a nice helper for that:

    <%= link_to "Sign in as User with Facebook", user_oauth_authorize_url(:facebook) %>

Devise uses the root_path and flash[:notice] to display the authentication messages, so don't forget to set the root in your routes.rb and a flash in your layout.
Just click in the "Sign in as User with Facebook" and everything should be working by now.

![](https://img.skitch.com/20110906-dr48jki3bkfd8jngu2bs5eu43y.jpg)
![](https://img.skitch.com/20110906-ffugpgd5wd62jegtdbqkcxidq8.jpg)
