---
layout: post
title: Mobile First - what it really means
---

You hear the term **Mobile First** thrown around a lot lately when it comes to building a new application.  Unfortunately I don't think everyone is on the same page when it comes to what **Mobile First** means.  I hope to enlighten you as to it's meaning in this post and hopefully you'll walk away with a greater understanding of the issues facing you when building an application for our increasingly mobile world.

First off I don't believe that **Mobile First** means building out your application for a mobile platform before any other.  Which is how I expect most people take the meaning of that term.  You don't have to release a mobile product before your web or desktop product.  What you do have to do is consider the mobile platform and it's limitations when building an application.  Understanding that someone may be viewing your content on a 3.5" screen (iPhone 4S screen size) will inform your decisions when it comes to data density, or workflow, or API structure.

We will talk about the following elements of **Mobile First** design, Connectivity, Bandwidth, Data, Content, Authentication, Sessions, each of which can, and probably will, be an entire blog post on it's own.  But lets get a quick look into each of them here.

## Connectivity

## Bandwidth

## Data

## Content

I know, content sounds a lot like data but I've separated it out for a very specific reason.  When I talk about content I'm talking mainly about information in your application that doesn't really fall into the category of data.  It's easy to define data.  Data is a username, a rating (star or thumbs up) on a post, a phone number, what is a post or a comment on a post?  Are they just data or are they content?  I tend to think of these things as content because what they contain isn't as cut and dry as a data field.  So for this section lets think about a post and reply threaded discussion.

When your user submits a post will they just submit text and be done with it, or will they submit text with some formatting?  **Bold**, *Italic*, ~~strikethrough~~?  What about images or links to other content?  I'm sure many of you are already thinking about integrating [TinyMCE](http://www.tinymce.com/) or another WYSIWYG editor.  But how will that content be created or viewed on a mobile device?  Are we just going to embed webviews throughout our app so that we can display this content?  Are we going to strip out all of the HTML and just display plain text?  When desinging for **Mobile First** these are the questions we need to answer.  Personally I prefer to store this content in a format that is display agnostic while still containing formatting properties.  Would you be surprised to know that this Blog is written and stored in one of these formats?  Or that my [resume](/resume) also is?  Do you know why I've choosen to store this content in this way?  Perhaps viewing my resume like [this](/resume.html) will give you a hint.  Both of those views of my resume come from the same content, that content happens to be [Markdown](http://daringfireball.net/projects/markdown/).  By storing my content in this display agnostic format I'm able to reformat it on the display side, without changing the content, to best fit the medium where it will be consumed.

By using a display agnostic format for storing your content you give your frontend systems the freedom to display it in whatever way is best for them.  Which in turn will be the best way for your users to consume that data on the device they happen to be using.

## Authentication

## Sessions
