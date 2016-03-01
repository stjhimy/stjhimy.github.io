---
layout: post
title:  "Riding the hash-bang(#!) with PathJs and ajax"
date:   2011-05-26
permalink: /posts/22-riding-the-hash-bang-with-pathjs-and-ajax-easy-twitter-like-urls
categories: old
---

tags: JQuery, Javascript, Ajax, Ruby on Rails date: 2011-05-26 23:37:05.000000000Z

## Have you ever asked yourself...

How twitter urls work with thoses hash-bangs (#!)?

**/lost image/**

Actually it's pretty simple; you need to listen the url changes and try to
match the route. If it matches, you just need to call an ajax function to do
the html replace stuffs.

And how I listen to a url changes?

************************************************************

## PathJs

PathJS is a lightweight, client-side routing library that allows you to create
"single page" applications.

You can create stuffs like that:

    // Use an anonymous function
    Path.map("#/my/first/route").to(function(){
        alert("Hello, World!");
    });

    // Or define one and use it
    function hello_world(){
        alert("Hello, World!");
    }
    Path.map("#/kittens").to(hello_world);

You can check the full documentation here: [path.js repository][11]

   [11]: https://github.com/mtrpcic/pathjs

## Creating a twitter like app

## Setting up

I will create a table called 'users' and put some information there just to
see the PathJs and ajax in action.

    rails g model user username:string email:string info:text
      invoke  active_record
      create    db/migrate/20110526012026_create_users.rb
      create    app/models/user.rb
      invoke    test_unit
      create      test/unit/user_test.rb
      create      test/fixtures/users.yml

I know it's not a good practice to insert data inside a migration, but I will
do this here just for testing purpose. After running the migration our app
will already have some basic data. My migration looks just like that:

    class CreateUsers < ActiveRecord::Migration
      def self.up
        create_table :users do |t|
          t.string :username
          t.string :email
          t.text :info

          t.timestamps
        end

        User.create(
          :username => "stjhimyblog",
          :email => "stjhimy@gmail.com",
          :info => "Ruby developer"
        )

        User.create(
          :username => "jhonm",
          :email => "jhonm123@gmail.com",
          :info => "Java developer"
        )

        User.create(
          :username => "Link",
          :email => "link123past@gmail.com",
          :info => "Game character"
        )

      end

      def self.down
        drop_table :users
      end
    end

Now I need a view and a route for that, I also gonna use the nifty-generators
gem to create a basic layout.

Gemfile:

    source 'http://rubygems.org'

    gem 'rails', '3.0.7'
    gem 'sqlite3'
    gem 'nifty-generators'
    #bundle install after that

    rails g nifty:layout
       force  app/views/layouts/application.html.erb
      create  public/stylesheets/application.css
      create  app/helpers/layout_helper.rb
      create  app/helpers/error_messages_helper.rb

Controller:

    rails g controller home index
      create  app/controllers/home_controller.rb
       route  get "home/index"
      invoke  erb
      create    app/views/home
      create    app/views/home/index.html.erb
      invoke  test_unit
      create    test/functional/home_controller_test.rb
      invoke  helper
      create    app/helpers/home_helper.rb
      invoke    test_unit
      create      test/unit/helpers/home_helper_test.rb

Routes:

    PathjsAjax::Application.routes.draw do
      root :to => "home#index"
    end

At last we need to add JQuery and Pathjs inside our layout, in my case the
application.html.erb:

    <head>
    <%= javascript_include_tag "jquery.min.js" %>
    <%= javascript_include_tag "path.js"%>

And delete the file public/index.html:

    rm public/index.html

You can now start the app server.

## Time to code, ohay!

I will get all users in my home controller and add links using the hash-bang
(#!) in my view:

HomeController#Index:

    def index
      @users = User.all
    end

application.html.erb layout:

    <% @users.each do |user| %>
      <%= link_to user.username, "#!/#{user.username}" %>
    <% end %>
    <%= yield %>

If you click on one of the links in the page now, you'll be able to see the
url changing:

**/lost image/**

## The pathjs magic

All we need to do now is to create PathJs route and do the ajax stuff.

I like to write a separated file for my PathJs routes, routes.js:

    Path.map("#!/:username").to(function(){
      var username = this.params['username'];
      $.ajax({
        type:"GET",
        dataType:"script",
        url: "/home/user_info/" + username,
        error: function(){
          alert("fail :(");
        }
      });
    });

Add the route.js to your layout:

    <%= javascript_include_tag "routes" %>

And finally add the Path.listen() to the end of the html body:

    <script type="text/javascript" language="javascript" charset="utf-8">
      $(function(){
          Path.listen();
      });
    </script>

If you click any link now, you're going to see a fail alert.

** What just happened?
PathJs are listening to our url. Any change that matches a route written
inside our routes.js will call the correspondent function. In our case it will
try to make an ajax call to /home/user_info/:username and, since we do not
have this route, it will throw an error. **

## The ajax magic

First we need a place to render the html returned from the ajax call, in my
application.html.erb layout I'll create a div:

    <% @users.each do |user| %>
      <%= link_to user.username, "#!/#{user.username}" %>
    <% end %>
    <div id="ajax-content">
    </div>
    <%= yield %>

A bit of style to our div:

    #ajax-content {
      width: 90%;
      height: 250px;
      margin: 0 auto;
      margin-top: 20px;
      padding:15px 20px;
      background-color: #DDD;
      border: solid 2px #efefef;
    }

Now we need a route that matches /home/user_info/:username, so in my
routes.rb:

    match "/home/user_info/:username", :to => "home#user_info"

And an action in the home controller:

    def user_info
      @user = User.find_by_username(params[:username])
    end

Almost there, now we just need a view.

I like to create two files here: one for the js response and a partial for the
real html. The partial file will be called _info.html.erb and it will render
the user information:

    <h1><%= @user.username %></h1>
    <h2><%= @user.email %></h2>
    <h2><%= @user.info %></h2>

The js file which responds the ajax call will be named user_info.js.erb and it
will replace the div #ajax-content for the partial file created before:

    $("#ajax-content").html("<%= escape_javascript(render :partial => 'info') %>")

If you try to click any of the user's links, you should see this info bellow:

**/lost image/**

Ohay! Creating hash-bang urls using PathJs is pretty fun and clearly very easy.
If you prefer there is an alternative called [sammyjs][16].

   [16]: http://sammyjs.org/

The source code of this post can be found [here][17]

   [17]: https://github.com/stjhimy/blog-posts/tree/master/22-riding-the-hash-bang-with-pathjs-and-ajax-easy-twitter-like-urls
