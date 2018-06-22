---
layout: post
title: Display-agnostic Content
---

When I think about building **3 Screens** applications, one of the first things I try to get right is my content.  Back in the day when you created an application it was for a single platform and a single display technology.  Then along came the web and the holy grail of software development, *write once run anywhere*, started to look like it could be a reality (i know, java, but come on).  Then came smart mobile phones and the number and types of clients that could be consuming your application exploded, the ability to write a quality application and run it anywhere became harder and harder typically ending up with lowest common denominator solutions.  Usually, these lowest common denominator solutions were because the decision was made to stick with the Web as the medium for running the applications.  The problem is that many of these applications didn't separate their logic from their display, and those that did never thought someone other than a web viewer would be consuming that display.

By forcing all of your customers to display web pages you are relying on a 3rd party application, the web browser, to take charge of how your application is presented to your customers.  Anyone who's written a web page in the past knows the pain of trying to get your application to look right on a given browser, let alone all of them.  "This page best viewed in I.E.", anyone remember this (Government workers still have to deal with it)?  And once you conquered this along came phones with their small screens to just ruin everything you've done.

The way I like to avoid this nightmare is to pull my content out of it's display.  I like to write out my content into separate containers and then consume and layout that content at display/compile time.  To do this well we need to think about content container formats.  This is where your display-agnostic content lives and while there are a lot of formats you can use I'm just going to mention a couple here [Text](http://en.wikipedia.org/wiki/Text_file), [HTML](http://en.wikipedia.org/wiki/HTML), [MarkDown](http://en.wikipedia.org/wiki/Markdown), [TeX](http://en.wikipedia.org/wiki/TeX).  Yes I know I just finished telling you that HTML was bad and here I am including it in my list, it's not inherently evil but you need to really understand why you are using it and what you are using it for, be patient all will be made clear.

## Text

The end all, be all of display-agnostic content containers.  Just a bunch of ascii characters all lined up.  What's nice about using plain text is your front-end doesn't have to deal with any formatting embedded in your content.  The bad thing about plain text is that your content creators can't offer any suggestions as to how the text should be displayed.  What if you have an **important** word?  Text doesn't care.  What about a [link](http://en.wikipedia.org/wiki/Text_file) to a web page?  Text doesn't care.  If you want to call that out in your content you are out of luck, short of writing your own markup language, your content is just text.

## HTML

HTML is a very powerful text markup language (HTML - Hyper Text Markup Language), and it does a great job of taking your plain text and marking it up with various tags that allow you to do *various* **nifty** ***things*** to that text.  The problems with HTML are that, during the browser wars, it grew beyond a text markup language into a complete display solution and therein lies the rub.  If you can be very strict and limit your content creators to only the subset of HTML that defines text markup (bold, italic, underline, etc.) then HTML is a fine display-agnostic content container.  Just beware that on display you'll probably be using a general purpose HTML display engine that isn't as strict as you and could lead to many unintended consequences ([cross site scripting](http://en.wikipedia.org/wiki/Cross-site_scripting)).

## Markdown

My personal favorite.  Markdown is a throwback to what HTML was originally, before the browser wars.  Markdown simply adds tags to your text to offer up formatting hints to whoever is displaying it.  The nice thing about Markdown is that the content is still very human readable even if the displayer does nothing to it.

```
Heading
 =======
 
 Sub-heading
 -----------
 
 ### Another deeper heading
 
 Paragraphs are separated
 by a blank line.
 
 Text attributes *italic*, **bold**,
 `monospace`, ~~strikethrough~~ .
 
 A [link](http://example.com).

 List:
 
   * apples
   * oranges
   * pears
 
 Numbered list:
 
   1. apples
   2. oranges
   3. pears
 
 The rain---not the reign---in
 Spain.
```

## TeX

Pronounced Tek for those of you, like me, who always want to pronounce it like the beginning of Texas.  TeX is a content format designed originally for text books with the stated goal of "give exactly the same results on all computers, at any point in time.".  Which sounds very much like what I'm suggesting when it comes to display-agnostic content.  While I wouldn't recommend using TeX as your content container I had to include it here because it's a very intriguing choice.  TeX gives your content creators full control over the layout of the content (within whatever container you happen to place it in).  Since it's original goal was typesetting it very much thinks in that world of pages and figures and layouts I'm not convinced that it's a good choice for **3 screens** development, but I'm still intrigued by it.
