---
layout: post
title:  "How to create a dynamic sitemap using Ruby on Rails"
date:   2010-06-01
permalink: /posts/02-how-to-create-a-dynamic-sitemap-using-ruby-on-rails
categories: old
---

##What are sitemaps and why make a good use of that

Sitemaps are a very simple way to tell all search engines about pages in your website. Keeping it updated is always recommended if you want to people find your page in search engines like Google, Bing, Yahoo and others.Sometimes it helps the search engines to index pages which is not indexed by default.

It's basically a XML file describing all URLs in your page:

    <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
      <url>
        <loc>http://www.stjhimy.com</loc>
        <priority>1.0</priority>
      </url>
      <url>
        <loc>http://www.stjhimy.com/category/1</loc>
        <priority>1.0</priority>
      </url>
    </urlset>

##Creating your dynamic sitemap with Ruby on Rails

First thing we need to do is specify the URL of our sitemap in the routes.rb file:

    map.sitemap "/sitemap.:format",
         :controller => "home",
         :action => "sitemap",
         :conditions => { :method => :get }

As you can see, my sitemap URL is gonna be http://www.mysite.com/sitemap.xml

Now go to your desired controller and create the sitemap method and put all you want to show in your sitemap there.
I'm using a blog as example, so all I need are my posts and my categories:

    @posts = Post.all
    @categories = Category.all

Now that we have all the logic done, we need to create our view to show that sitemap when some request riches http://www.mysite.com/sitemap.xml.We need to build a XML containing the 3 main nodes to make it a sitemap:

 - loc - The URL of your page
 - priority - The priority of the indexing page process between 0 and 1
 - lasmod - Date of the last modification


The first node is mandatory, the others you can skip if you want.
Using the XML builder, create the file sitemap.xml.builder to our sitemap and put the next lines:

    xml.instruct!
    xml.urlset "xmlns" => "http://www.sitemaps.org/schemas/sitemap/0.9" do

      xml.url do
        xml.loc "http://www.mysite.com"
        xml.priority 1.0
      end

      @categories.each do |category|
        xml.url do
          xml.loc category_url(category)
          xml.priority 0.9
        end
      end

      @posts.each do |post|
        xml.url do
          xml.loc post_url(post)
          xml.lastmod post.updated_at.to_date
          xml.priority 0.9
        end
      end

    end

I guess the code is self explanatory.

## Submitting your sitemap to Google and others search engines

There are 3 ways to submit our sitemap:

##1 - Google Webmaster Tools:

Click [here][1] , create your account, add your website and submit your sitemap without any trouble.

##2 - Sending a ping request to Google with your sitemap URL:

www.google.com/webmasters/tools/ping?sitemap=http://www.mysite.com/sitemap.xml

##3 - Editing your robots.txt file:

robots.txt is the default file that specify the content you want to be find by the spiders and crawlers of the search engines
If you put your sitemap URL there, search engines can easily find that and index your pages.
You can find the file in /public/robots.txt, put the next line:

    Sitemap: http://www.stjhimy.com/sitemap.xml

And, that's all, your page should be indexed in a few days.

  [1]: https://www.google.com/accounts/ServiceLogin?service=sitemaps&amp;passive=true&amp;nui=1&amp;continue=https://www.google.com/webmasters/tools/&amp;followup=https://www.google.com/webmasters/tools/&amp;hl=en
