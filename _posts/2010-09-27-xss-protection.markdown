---
layout: post
title:  "XSS Protection"
date:   2010-09-27
permalink: /posts/16-xss-protection
categories: old
---

tags: Security date: 2010-09-27 21:40:57.000000000Z

##Protection

This week a lot of famous websites like twitter, youtube and even the dead orkut received some XSS attacks.

##But first, what is XSS ? Wikipedia says:

*Cross-site scripting&nbsp;(XSS) is a type of&nbsp;<a title="Computer insecurity" href="http://en.wikipedia.org/wiki/Computer_insecurity">computer security</a>&nbsp;<a class="mw-redirect" title="Vulnerability (computer science)" href="http://en.wikipedia.org/wiki/Vulnerability_(computer_science)">vulnerability</a>&nbsp;typically found in&nbsp;<a title="Web application" href="http://en.wikipedia.org/wiki/Web_application">web applications</a>&nbsp;that enables malicious attackers to&nbsp;<a title="Code injection" href="http://en.wikipedia.org/wiki/Code_injection">inject</a>&nbsp;<a class="mw-redirect" title="Client-side script" href="http://en.wikipedia.org/wiki/Client-side_script">client-side script</a>&nbsp;into&nbsp;<a title="Web page" href="http://en.wikipedia.org/wiki/Web_page">web pages</a>&nbsp;viewed by other users. An exploited cross-site scripting vulnerability can be used by attackers to bypass&nbsp;<a title="Access control" href="http://en.wikipedia.org/wiki/Access_control">access controls</a>&nbsp;such as the&nbsp;<a title="Same origin policy" href="http://en.wikipedia.org/wiki/Same_origin_policy">same origin policy</a>. Cross-site scripting carried out on websites were roughly 80% of all security vulnerabilities documented by Symantec as of 2007*

##How can this little thing bring the caos to your website?

Imagine you have a field, a simple text field, so the user can update his status:

    <%= f.text_field :status %>

if you fill with something like that:

    <script>alert("Your website sucks!")</script>

In your view you show the status like that:

    <%= @user.status %>

If Theres no XSS protection, every time you hit the page, you gonna see the alert:

**/lost image/**

##How XSS protection works in Rails 3?

By default all your outputs will be escaped, so in the example before you never saw the popup, just the plain text:

    <script>alert("Your website sucks!")</script>

And if you want the output to not be escaped you can use:

    <%= raw @user.status %>

And then the popup will raise.

**This is a short and simple post, but remember to ALWAYS ESCAPE dangerous user text input, this probably will save you a lot of time in the future.**
