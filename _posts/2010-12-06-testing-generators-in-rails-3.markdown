---
layout: post
title:  "Testing generators in rails 3"
date:   2010-12-06
permalink: /posts/18-testing-generators-in-rails-3
categories: old
---

tags: Ruby on Rails date: 2010-12-06 10:43:57.000000000Z


##Generators in rails 3 rocks

Rails 3 comes with an entire new way to create generators and it's quite awesome. To make it more beautiful rails comes with a new Generators::TestCase making easy to test a generator.

##A real world generator

Let's take a look at some simple generator, everyone by know should be familiar with "devise" and everyone know about:

    rails g devise:install

This generator is quite simple, here's the code:

    module Devise
      module Generators
        class InstallGenerator < Rails::Generators::Base
          source_root File.expand_path("../../templates", __FILE__)

          desc "Creates a Devise initializer and copy locale files to your application."
          class_option :orm

          def copy_initializer
            template "devise.rb", "config/initializers/devise.rb"
          end

          def copy_locale
            copy_file "../../../config/locales/en.yml", "config/locales/devise.en.yml"
          end

          def show_readme
            readme "README" if behavior == :invoke
          end
        end
      end
    end

The idea is pretty simple, you need to run the generator and make sure all files are created properly. There are a few methods to help you:

##The API

**tests(klass)**: Sets which generator should be tested:

    tests AppGenerator

**destination(path)**: Sets the destination of generator files:

    destination File.expand_path("../tmp", File.dirname(__FILE__))

**prepare_destination()**: Erases the destinations directory, preparing it for the test

**run_generator(args=self.default_arguments, config={})**: Runs the generator configured for this class. The first argument is an array like command line arguments:

**assert_file(relative, *contents)**: Asserts a given file exists. You need to supply an absolute path or a path relative to the configured destination.

The complete API can be found [here](http://api.rubyonrails.org/classes/Rails/Generators/TestCase.html).


##Let's see the code

    class InstallGeneratorTest < Rails::Generators::TestCase
      tests Devise::Generators::InstallGenerator
      destination File.expand_path("../tmp", File.dirname(__FILE__))
      setup :prepare_destination

      test "Assert all files are properly created" do
        run_generator
        assert_file "config/initializers/devise.rb"
        assert_file "config/locales/devise.en.yml"
      end

    end

- Set the generator to test
- Set the destination folder
- Prepare the directory
- Run the generator
- Assert all files are properly created

Simple hum?

I just loved this new generators test API, usually the generators are the darkside of the application and no one cares about testing it.
Again, the complete API is [here](http://api.rubyonrails.org/classes/Rails/Generators/TestCase.html), take a deep look and start testing your new generators.

  Cya!
