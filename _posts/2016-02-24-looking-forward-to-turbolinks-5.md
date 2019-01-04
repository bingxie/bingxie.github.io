---
layout: post
title: Looking forward to Turbolinks 5
categories: [Tech]
tags: [rails5, turbolinks]
---

![](/images/Bing_704.JPG)

As a practical rails developer, we all want to keep up to date with new Rails direction and status. With coming Rails5 we will have Turbolinks 5 and Rails API.

Turbolinks 5 focuses on integration with native iOS and Android Wrappers. Rails API is designed for the client-side MVC and 100% native mobile apps.

Turbolinks acturally is very good technology. It gives rails developers the performance benefits of SPA approaches without writing a lot of javascript.

What we need to do is just follow Rails guide and keep all of you JS and CSS in the HTML <head>. Turbolinks will only load the body party via AJAX when user triggers a link.

But sometimes if I want to update part of the page with Rails, I can response a piece of javascript rendered with a html partial, this is also very cool.
And PJAX is another good solution，especially when your page has a side menu bar or tabs, PJAX can update the content part of the page.

Some of the famous rails websites are using turbolinks, like Shopify, Github(pjax) and Basecamp.
So if you haven't used it, you can start with the Turbolinks 5.
