---
layout: post
title:  "Gem review: Octopus, database sharding for ActiveRecord"
date:   2011-02-28
permalink: /posts/19-gem-review-octopus-database-sharding-for-activerecord
categories: old
---

tags: Databases, Ruby on Rails, Scaling date: 2011-02-28 20:58:51.000000000Z

##Information

 - The gem: Octopus
 - Repo: [https://github.com/tchandy/octopus][11]
 - Creator and maintainer: [tchandy][12]
 - Contributors: [https://github.com/tchandy/octopus/contributors][13]

   [11]: https://github.com/tchandy/octopus
   [12]: https://github.com/tchandy
   [13]: https://github.com/tchandy/octopus/contributors

## Seriously wtf is database sharding?

Database sharding is a method of horizontal partitioning in a database or
search engine. Each individual partition is referred to as a shard or database
shard.

Basically => Multiple databases in your rails application.

*************************************************************

##  Why/when should i care about sharding my database?

**/lost image/**

  * If you have tables with massive reads/writes you should shard this table to separete database, octopus can help you
  * If you need to replicate your database, octopus can help you
  * If you need to move data along databases, octopus can help you

## Setting up

Can't be easier:


      gem 'ar-octopus', :require => "octopus"


and call the bundler:


      bundle install


Now you need to create a config file to tell your application which shards you
want, a simple yaml config/shards.yaml:


      octopus:
        shards:
          shard_sqlite:
          adapter: sqlite3
          database: db/db_one.sqlite3
          pool: 5
          timeout: 5000

          shard_pgsql:
            adapter: postgresql
            username: postgres
            password:
            database: db_two
            encoding: unicode


Easy as pie, this is pretty simple, if you need to group or something like
that, please thake a look [here][16]

   [16]: https://github.com/tchandy/octopus/wiki/config-file

##Time to code, ohay!

##Dynamic change your shard looking for something in your database


      Project.where(:name => "stjhimyblog").using(:shard_sqlite)
      Project.where(:name => "stjhimyblog").using(:shard_pgsql)


You can also use a block, just nice:


      Octopus.using(:shard_sqlite) do
        Project.find(1)
      end


This feature is nice when you want to use a specific shard in only part of the
project, maybe some report, usually big queries.

##Specific model, specific database

You can also tell your model a default db to use:


      class Project < ActiveRecord::Base
        octopus_establish_connection(:adapter => "sqlite3", :database => "db_one")
      end


##Migrations

using method is also available in your migrations:


      class CreateSomeRandomProjects < ActiveRecord::Migration
        using(:shard_sqlite)

        def self.up
          Project.create!(:name => "stjhimyblog")
        end

        def self.down
          Project.delete_all
        end
      end


## Last words

Octopus is a pretty solid gem, the wiki/documentation is very detailed and the features just great. If someday you need to do some database replication/sharding you should give octopus a shot.

[Again, the official repository here](https://github.com/tchandy/octopus)
