---
layout: post
title:  "Using comma as decimal separator"
date:   2010-08-07
permalink: /posts/11-using-comma-as-decimal-separator
categories: old
---

tags: Ruby, Quick Snippets date: 2010-08-07 19:53:43.000000000Z

##Comma is good...

Sometimes when you are working with decimal numbers you just need to use comma as separator.
I was facing this trouble at work last week, and thank's to the awesome tip from [igorhrk](http://www.twitter.com/igorhrk).

To use comma as decimal separator you just need a few and quick steps:
Create a new initializer in your app, i like to call it comma_as_decimal_separator.rb:

    ActiveRecord::Base.class_eval do

      def convert_number_column_value_with_comma_separator(value)
        value = convert_number_column_value_without_comma_separator(value)
        if value.is_a?(String)
          value = value.gsub(',', '.')
        end
        value
      end

      alias_method_chain :convert_number_column_value, :comma_separator

    end

    ActiveRecord::ConnectionAdapters::Column.class_eval do

      def type_cast_with_comma_separator(value)
        if type == :decimal && value.is_a?(String)
          value = value.gsub(',', '.')
        end
        type_cast_without_comma_separator(value)
      end

      alias_method_chain :type_cast, :comma_separator

    end


This should get the job done, but still another problem, when you get some value from your database, the value
still coming with no comma, to fix that just create another initializer, i like to call it number_field.rb:

    ActionView::Helpers::FormBuilder.class_eval do

      def number_field(field, options = {})
        value = object.send(field)
        value = "%01.2f" % value if value.is_a?(Float)
        value = value.to_s.gsub('.', ',')

        options[:value] = value
        options[:class] = "#{options[:class]} number_field"
        text_field(field, options)
      end

    end

In your views now you can use the next helper:

    <%= f.number_field :value %>
