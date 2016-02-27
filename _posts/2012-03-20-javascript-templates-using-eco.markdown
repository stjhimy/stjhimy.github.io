---
layout: post
title:  "Javascript Templates using eco"
date:   2012-03-20
permalink: /posts/28-javascript-templates-using-eco
categories: old
---

tags: Ruby, Ruby on Rails date: 2012-03-20 21:00:00.000000000Z


##Writting inline HTML output with Javascript, fuuuuu

Have you ever used Javascript templates? No? Let's write an example.
Imagine if you have a function that receives some data in json format and you should add this data in a human format to the html body of the application.

    function renderData(data) {
      output = "<li class='"+ data.class+"'>"+ data.name +"</li>";
      $("body").append(output);
    }

Mounting the output string can be a pain in the ass some times, usually you will need to concatenate a lot of data to format the html output. Javascript templates are here to make it easy.

##Eco: Embedded CoffeeScript templates

[https://github.com/sstephenson/eco](https://github.com/sstephenson/eco)

Eco lets you embed CoffeeScript logic in your markup. It's like EJS and ERB, but with CoffeeScript inside the <% ... %>. Use it from Node.js to render your application's views on the server side, or compile your templates to JavaScript with the Eco command-line utility and use them to dynamically render views in the browser.

******************************************************************

An example:

    <% if @projects.length: %>
      <% for project in @projects: %>
        <a href="<%= project.url %>"><%= project.name %></a>
        <p><%= project.description %></p>
      <% end %>
    <% else: %>
      No projects
    <% end %>

##Rails, Coffee, Eco, JQuery

Let's try Eco with a new Rails app, using Coffee and JQuery.

    $ rails new eco
    $ rails g scaffold post title:string content:text
    $ rake db:migrate

Add Eco to the Gemfile on the assets group:

    group :assets do
      gem 'sass-rails',   '~> 3.2.3'
      gem 'coffee-rails', '~> 3.2.1'
      gem 'uglifier', '>= 1.0.3'
      gem 'eco'
    end

And bundle again:

    $ bundle install

Now we are ready to start creating our templates, by convention I will add our templates to the folder app/assets/javascripts/templates/ with the extension jst.eco.

    ├── app
    │   ├── assets
    │   │   ├── images
    │   │   │   └── rails.png
    │   │   ├── javascripts
    │   │   │   ├── application.js
    │   │   │   ├── posts.js.coffee
    │   │   │   └── templates
    │   │   │       └── posts
    │   │   │           └── posts.jst.eco

Now it's time to configure the asset pipeline to get the template files. If you already have a "//= require_tree ." in your application.css.scss there's nothing else to do. The file will be already indexed by the asset pipeline, if not, you can add manually the file using the next line:

    //= require /templates/posts/posts.ejs.eco

If you check out on your browse http://localhost:3000/post the source code should show the post’s template already compiled to js:

![](https://img.skitch.com/20120314-dns46k58xsdxbp2ni6yicgb6f5.jpg)

Now we write a simple template to render our posts:

app/assets/javascripts/templates/posts/posts.ejs.eco

    <% if @posts.length: %>
      <% for post in @posts: %>
        <a href=""><%= post.title %></a>
        <p><%= post.content %></p>
      <% end %>
    <% else: %>
      No posts
    <% end %>

Can you imagine how painful would be write something like that without using templates? Meh, nasty.
If you take a look at the compiled js you will see how awesome it looks like, it also escape js and sanitize all the stuff for us:


    (function() {
      this.JST || (this.JST = {});
      this.JST["templates/posts/posts"] = function(__obj) {
        if (!__obj) __obj = {};
        var __out = [], __capture = function(callback) {
          var out = __out, result;
          __out = [];
          callback.call(this);
          result = __out.join('');
          __out = out;
          return __safe(result);
        }, __sanitize = function(value) {
          if (value && value.ecoSafe) {
            return value;
          } else if (typeof value !== 'undefined' && value != null) {
            return __escape(value);
          } else {
            return '';
          }
        }, __safe, __objSafe = __obj.safe, __escape = __obj.escape;
        __safe = __obj.safe = function(value) {
          if (value && value.ecoSafe) {
            return value;
          } else {
            if (!(typeof value !== 'undefined' && value != null)) value = '';
            var result = new String(value);
            result.ecoSafe = true;
            return result;
          }
        };

        .................AND MORE MORE MORE.

Take a look at the 3th line: JST["templates/posts/posts"], this is how we render the template.

Let's now write some Coffee to make an Ajax request/posts return a JSON containing a list of posts and then render our template.

First let's create some posts using the Rails console:

    $ rails c
    1.9.3-p125 :001 > Post.create(:title => "Eco rulez!", :content => "How to master js templates using eco.")
    1.9.3-p125 :002 > Post.create(:title => "Js for dummies.", :content => "How to master js in a couple of minutes, ohaaay!")

Let's write a simple script to get the posts and print on the console:

    $.ajax
      url: "/posts/",
      type: "GET",
      dataType: "JSON",
      success: (data) ->
        console.log JST["templates/posts/posts"](posts : data)

Bring tears to my eyes seing how beaultiful coffee is.
If everything goes right you should now see something like that in your console:

![](https://img.skitch.com/20120315-dip5yx5f7fp1n8ahdqmgrdghje.jpg)

All you need to do now is render this generated HTML to the page. I will use the append function of JQuery:

    $.ajax
      url: "/posts/",
      type: "GET",
      dataType: "JSON",
      success: (data) ->
        $("body").append JST["templates/posts/posts"](posts : data)

![](https://img.skitch.com/20120315-tt62kenehqarurrhtcbffc6n6q.jpg)

Beautiful!

##Syntax

There's a lot of stuff that you can do on Eco, here's the basic ones:

- <% expression %>: Evaluate a CoffeeScript expression without printing its return value.
- <%= expression %>: Evaluate a CoffeeScript expression, escape its return value, and print it.
- <%- expression %>: Evaluate a CoffeeScript expression and print its return value without escaping it.
- <%= @property %>: Print the escaped value of the property from the context object passed to render.
- <%= @helper() %>: Call the helper method from the context object passed to render, then print its escaped return value.
- <% @helper -> %>...<% end %>: Call the helper method helper with a function as its first argument. When invoked, the function will capture and return the content... inside the tag.
- <%% and %%> will result in a literal <% and %> in the rendered template, respectively.

You can check the full list [here](https://github.com/sstephenson/eco).

##Finishing up

Coffee is beautiful (in my opinion) and Eco brings all that beauty to the Javascript templates. There's a lot of alternatives to Javascript templates, jst, haml and others, but I will stick to Eco for a while, I think.

That's it, it's a pretty simple example in the blog post, if you like you can check the source code [here](https://github.com/stjhimy/blog-posts/tree/master/28-javascript-templates-using-eco).
