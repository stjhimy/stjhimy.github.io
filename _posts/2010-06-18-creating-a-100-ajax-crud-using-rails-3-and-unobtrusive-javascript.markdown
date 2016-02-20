---
layout: post
title:  "Ajax CRUD using rails 3 and unobtrusive javascript"
date:   2010-06-18
permalink: /posts/07-creating-a-100-ajax-crud-using-rails-3-and-unobtrusive-javascript
categories: old
---

tags: JQuery, Ajax, Javascript, Ruby on Rails date: 2010-06-18 01:02:42.000000000Z

##Creating the project and setting the Gemfile

*Edited: Just corrected the misspelling on the title "unobtrusive", sorry for that**
*Ps: I'm using the latest rails 3 beta version while coding**

    $ rails -v
    Rails 3.0.0.beta4

Creating the project:

    $ rails new ajax_on_rails -d mysql

##Setting up the Gemfile

I'm setting the nifty-generators here, just to generate our main CRUD.

    source 'http://rubygems.org'
    gem 'rails', '3.0.0.beta4'
    gem 'mysql'
    gem 'nifty-generators'

##Generating the layout scaffold

    $ rails g nifty:layout
    $ rails g nifty:scaffold Post title:string content:text
    $ rake db:create
    $ rake db:migrate

If you run your server and go to the "/posts" you will see a common scaffold.

##Changing the CRUD for an unique form

So... we need to change some little things. We gonna create a unique form in "/posts".
Inside that form we gonna make everything with ajax: list, create, edit and delete.
So first we need to change our Post controller to make everything works:

    def index
      @posts = Post.all
      @post = Post.new
    end

    def create
      @posts = Post.all
      @post = Post.new(params[:post])
      if @post.save
        flash[:notice] = "Successfully created post."
        redirect_to posts_url
      else
        render :action => 'index'
      end
    end

    def edit
      @posts = Post.all
      @post = Post.find(params[:id])
      render :action => "index"
    end

    def update
      @posts = Post.all
      @post = Post.find(params[:id])
      if @post.update_attributes(params[:post])
        flash[:notice] = "Successfully updated post."
        redirect_to posts_url
      else
        render :action => 'index'
      end
    end

    def destroy
      @post = Post.find(params[:id])
      @post.destroy
      flash[:notice] = "Successfully destroyed post."
      redirect_to posts_url
    end

Making a basic refactoring creating a method to load some objects before every action:

    before_filter :load

    def load
      @posts = Post.all
      @post = Post.new
    end

    def index
    end

    def create
      @post = Post.new(params[:post])
      if @post.save
        flash[:notice] = "Successfully created post."
        redirect_to posts_url
      else
        render :action => 'index'
      end
    end

    def edit
      @post = Post.find(params[:id])
      render :action => "index"
    end

    def update
      @post = Post.find(params[:id])
      if @post.update_attributes(params[:post])
        flash[:notice] = "Successfully updated post."
        redirect_to posts_url
      else
        render :action => 'index'
      end
    end

    def destroy
      @post = Post.find(params[:id])
      @post.destroy
      flash[:notice] = "Successfully destroyed post."
      redirect_to posts_url
    end
    end

As you can see we are not using the "new" and "show" methods anymore
Now we need to change our index template to render the post form, we can do that adding a partial:

app/views/posts/_form.erb

    <%= form_for @post do |f| %>
      <%= f.error_messages %>
      <p>
        <%= f.label :title %><br/>
        <%= f.text_field :title %>
      </p>
      <p>
        <%= f.label :content %><br/>
        <%= f.text_area :content, :rows => 5 %>
      </p>
      <p><%= f.submit %></p>
    <% end %>

And we render this one in our index file:

spp/views/posts/index.html.erb

    <% title "Posts" %>

    <%= render :partial => 'form' %>

    <table>
      <tr>
        <th>Title</th>
        <th>Content</th>
      </tr>
      <% for post in @posts %>
        <tr>
          <td><%= post.title %></td>
          <td><%= post.content %></td>
          <td><%= link_to "Show", post %></td>
          <td><%= link_to "Edit", edit_post_path(post) %></td>
          <td><%= link_to "Destroy", post, :confirm => 'Are you sure?', :method => :delete %></td>
        </tr>
      <% end %>
    </table>

    <p><%= link_to "New Post", new_post_path %></p>


It should be working now with a unique view. We are not using the "show.html.erb", "new.html.erb" and "edit.html.erb" files anymore, feel free to delete it.
Before we reach the next part, let's add some validation in our model, we gonna need that later:

    class Post < ActiveRecord::Base
      attr_accessible :title, :content

      validates_presence_of :title,:content
    end


##Changing from Prototype to Jquery

For default Rails implements ajax requests with prototype, we need to change it to work with jquery.
First, download the rails.js equivalent with jquery from [github](http://github.com/rails/jquery-ujs/blob/master/src/rails.js).
Copy the rails.js under the src folder to your public/javascripts/.
In our layout "application.html.erb" we'll change the next lines:

    <%= javascript_include_tag :defaults %>
    <%= csrf_meta_tag %>

to

    <%= javascript_include_tag 'http://ajax.googleapis.com/ajax/libs/jquery/1.4.2/jquery.min.js' %>
    <%= javascript_include_tag 'rails' %>

##It's time to the ajax magic

Keep it simple, let's think about what we want:

- Our controller and links should make remote requests
- Our html should contain divs so we can change the content with unobstructive javascripts.
- Our actions should return js (javascript) content to change the html page.

So let's start changing everything again.
The template(layout) need to contain a div to show the flash_notice:

    <body>
      <div id="container">
        <div id="flash_notice" style="display:none"></div>
        <%= content_tag :h1, yield(:title) if show_title? %>
        <%= yield %>
      </div>
    </body>

Put some partials in our index and add the remote => true in our links and forms:

    <% title "Posts" %>

    <div id="post_form"><%= render :partial => 'form' %></div>
    <div id="posts_list"><%= render :partial => "posts" %></div>

app/views/posts/_form.html.erb

I also put a div to show the errors here, so we can change it in our js responses:

    <%= form_for(@post, :remote => true) do |f| %>
      <div id= "post_errors" style="display:none"></div>
      <p>
        <%= f.label :title %><br />
        <%= f.text_field :title %>
      </p>
      <p>
        <%= f.label :content %><br />
        <%= f.text_area :content, :rows => 5 %>
      </p>
      <p><%= f.submit %></p>
    <% end %>

app/views/posts/_posts.html.erb

    <table>
      <tbody><tr>
        <th>Title</th>
        <th>Content</th>
      </tr>

      <% @posts.each do |post| %>
        <tr>
          <td><%= post.title %></td>
          <td><%= post.content %></td>
          <td><%= link_to "Edit", edit_post_path(post), :remote => true %></td>
          <td><%= link_to "Destroy", post, :remote => true, :confirm => 'Are you sure?', :method => :delete %></td>
        </tr>
      <% end %>

      </tbody>
    </table>

I created this partial so i can update the list of posts with javascript too:

    <table>
      <tr>
        <th>Title</th>
        <th>Content</th>
      </tr>
      <% for post in @posts %>
        <tr>
          <td><%= post.title %></td>
          <td><%= post.content %></td>
          <td><%= link_to "Edit", edit_post_path(post), :remote => true %></td>
          <td><%= link_to "Destroy", post, :remote => true, :confirm => 'Are you sure?', :method => :delete %></td>
        </tr>
      <% end %>
    </table>

We need to change our controller too.
Since we gonna make a 100% ajax CRUD we don't need that redirects anymore:

    class PostsController < ApplicationController

      before_filter :load

      def load
        @posts = Post.all
        @post = Post.new
      end

      def index
      end

      def create
        @post = Post.new(params[:post])
        if @post.save
          flash[:notice] = "Successfully created post."
          @posts = Post.all
        end
      end

      def edit
        @post = Post.find(params[:id])
      end

      def update
        @post = Post.find(params[:id])
        if @post.update_attributes(params[:post])
          flash[:notice] = "Successfully updated post."
          @posts = Post.all
        end
      end

      def destroy
        @post = Post.find(params[:id])
        @post.destroy
        flash[:notice] = "Successfully destroyed post."
        @posts = Post.all
      end
    end

Almost there. If you try to use the form now, nothing happens right?
If you take a look at the console(server) you will find some errors telling you Rails can't find the view js to render.
So let's create this files. Basically we gonna create a view to render javascript, with this javascript we gonna inject or modify our html.
We are using jquery to do the modification/injection part, so if you never work with that take a look at the official site to learn a lot of cool stuffs: http://www.jquery.com
We need files to create, edit, update and destroy actions, so let's create them:

app/views/posts/create.js.erb

Here we verify if the @post object contains errors and changes the behavior according to that:

    <% if @post.errors.any? -%>
      /*Hide the flash notice div*/
      $("#flash_notice").hide(300);

      /*Update the html of the div post_errors with the new one*/
      $("#post_errors").html("<%= escape_javascript(error_messages_for(@post))%>");

      /*Show the div post_errors*/
      $("#post_errors").show(300);
    <% else -%>
      /*Hide the div post_errors*/
      $("#post_errors").hide(300);

      /*Update the html of the div flash_notice with the new one*/
      $("#flash_notice").html("<%= escape_javascript(flash[:notice])%>");

      /*Show the flash_notice div*/
      $("#flash_notice").show(300);

      /*Clear the entire form*/
      $(":input:not(input[type=submit])").val("");

      /*Replace the html of the div post_lists with the updated new one*/
      $("#posts_list").html("<%= escape_javascript( render(:partial => "posts") ) %>");
    <% end -%>

app/views/posts/edit.js.erb

In this action we just need to update the form with the select post.

    $("#post_form").html("<%= escape_javascript(render(:partial => "form"))%>");

app/views/posts/update.js.erb

    <% if @post.errors.any? -%>
      $("#flash_notice").hide(300);
      $("#post_errors").html("<%= escape_javascript(error_messages_for(@post))%>");
      $("#post_errors").show(300);
    <% else -%>
      $("#post_errors").hide(300);
      $("#flash_notice").html("<%= escape_javascript(flash[:notice])%>");
      $("#flash_notice").show(300);
      $(":input:not(input[type=submit])").val("");
      $("#posts_list").html("<%= escape_javascript( render(:partial => "posts") ) %>");
    <% end -%>

app/views/posts/destroy.js.erb

    $("#post_errors").hide(300);
    $("#flash_notice").html("<%= escape_javascript(flash[:notice])%>");
    $("#flash_notice").show(300);
    $("#posts_list").html("<%= escape_javascript( render(:partial => "posts") ) %>");

And that's it, go to your "/posts" page and you should see our 100% ajax CRUD.

*Ps1: That uses html5 to make our forms and links remotes (:remote => true), so it's not gonna work in any version of INTERNET EXPLORER yet.*

*Ps2: You can also use the redirect/render in your actions pointing to the index and then create only one javascript view (index.js.erb) treating that as you want. I prefer to keep things separated in case I decide to change the visual behavior of some view.*

*Ps3: Someone should make a generator for this ajax forms.*
