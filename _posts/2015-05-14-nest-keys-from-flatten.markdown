---
layout: post
title:  "Nest keys from flatten (dotted) hash"
date:   2015-05-14
categories: old
---

I was browsing through some old code today during the morning and found it.
The year was 2011 and this method was * such an awesome commit * at the time. I was working on a gem to help globalize apps. Still, tiny little nice method.

    def nest_translations(hash)
      hash.map do |main_key, main_value|
        main_key.to_s.split(".").reverse.inject(main_value) do |value, key|
          if value.is_a?(Hash)
            { key.to_s => nest_translations(value) }
          else
            { key.to_s => value }
          end
        end
      end.inject(&:deep_merge)
    end

    nest_translations('key1.key2.key3' => 'value')
     => {"key1"=>{"key2"=>{"key3"=>"value"}}}
