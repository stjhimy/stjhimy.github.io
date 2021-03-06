---
layout: post
title:  "Using ActiveRecord::Base methods"
date:   2010-06-03
permalink: /posts/03-using-activerecord-base-methods-to-create-multiple-database-connections-in-ruby-on-rails
categories: old
---

tags: Databases, Ruby on Rails date: 2010-06-03 21:37:16.000000000Z

##Understanding the ActiveRecord::Base

As all we know, ActiveRecord provents a bunch of customizations, as the title says we gonna change our database connection
in the execution time.

Digging a little deeper in ActiveRecord::Base we find a method called "establish_connection", let's take a look at the source code:

    # File activerecord/lib/active_record/connection_adapters/abstract/connection_specification.rb, line 51
    def self.establish_connection(spec = nil)
     case spec
       when nil
         raise AdapterNotSpecified unless defined? RAILS_ENV
         establish_connection(RAILS_ENV)
       when ConnectionSpecification
         self.connection_handler.establish_connection(name, spec)
       when Symbol, String
         if configuration = configurations[spec.to_s]
           establish_connection(configuration)
         else
          raise AdapterNotSpecified, "#{spec} database is not configured"
         end
       else
         spec = spec.symbolize_keys
         unless spec.key?(:adapter) then raise AdapterNotSpecified, "database configuration does not specify adapter" end

         begin
           require 'rubygems'
           gem "activerecord-#{spec[:adapter]}-adapter"
           require "active_record/connection_adapters/#{spec[:adapter]}_adapter"
         rescue LoadError
           begin
             require "active_record/connection_adapters/#{spec[:adapter]}_adapter"
           rescue LoadError
             raise "Please install the #{spec[:adapter]} adapter: `gem install activerecord-#{spec[:adapter]}-adapter` (#{$!})"
           end
         end

         adapter_method = "#{spec[:adapter]}_connection"
         if !respond_to?(adapter_method)
           raise AdapterNotFound, "database configuration specifies nonexistent #{spec[:adapter]} adapter"
         end

         remove_connection
         establish_connection(ConnectionSpecification.new(spec, adapter_method))
     end
    end

Now that we find this method, we just need to actually call it and change our database connection

##Calling the method and overwriting the environment connection

Since ActiveRecord provides all we need to change this connection, all we need to do is call this method passing diferentes parameters.

    ActiveRecord::Base.establish_connection(
      :adapter  => "adapter",
      :host     => "host",
      :username => "username",
      :password => "password",
      :database => "database"
    )

You can put this lines in your controller or you can write a method calling the establish_connection from ActiveRecord::Base

    def change_connection(parameters)
      ActiveRecord::Base.establish_connection(
        :adapter  => parameters[:adapter],
        :host     => parameters[:host],
        :username => parameters[:username],
        :password => parameters[:password],
        :database => parameters[:database]
      )
    end

And then you can call this method passing a hash of parameters:

    change_connection(
      :adapter => "mysql",
      :host => "localhost",
      :username => "root",
      :password => "yourpassword",
      :database => "yourdatabase"
    )

##Changing the connection of a specific model

Since our models inherit from ActiveRedord::Base, we can use the establish_connection method to change the connection of a specif model

     Model.establish_connection(
      :adapter  => "adapter",
      :host     => "host",
      :username => "username",
      :password => "password",
      :database => "database"
     )

And that's all, you should be abble to customize your connections now.

ps: I'm using rails 2.3.8 and had not tested with rails 3.
