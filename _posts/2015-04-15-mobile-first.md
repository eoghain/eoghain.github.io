---
layout: post
title: Mobile First - what it really means
---

You hear the term **Mobile First** thrown around a lot lately when it comes to building a new application.  Unfortunately I don't think everyone is on the same page when it comes to what **Mobile First** means.  I hope to enlighten you as to it's meaning in this post and hopefully you'll walk away with a greater understanding of the issues facing you when building an application for our increasingly mobile world.

First off I don't believe that **Mobile First** means building out your application for a mobile platform before any other.  Which is how I expect most people take the meaning of that term.  You don't have to release a mobile product before your web or desktop product.  What you do have to do is consider the mobile platform and it's limitations when building an application.  Understanding that someone may be viewing your content on a 3.5" screen (iPhone 4S screen size) will inform your decisions when it comes to data density, or workflow, or API structure.

We will talk about the following elements of **Mobile First** design, Connectivity, Bandwidth, Data, Content, and Authentication each of which can, and probably will, be an entire blog post on it's own.  But lets get a quick look into each of them here.

## Connectivity

When developing for **Mobile First** we really need to think about the ability of our applications to reach our servers.  When we talk about the web most people experience it through a desktop computer that has an almost permanent connection to the internet.  This isn't the case when we start talking about mobile devices.  Some devices only have WiFi connectivity, so they are portable but they aren't always able to reach the internet.  Other devices utilize cellular networks to reach the internet and could easly loose connectivity during a session or even request.  So it's very important to think about how connectivity will affect your application.  What happens if the user is in the middle of a process and looses connectivity?  Have we left them in some inconsistent state that they can't recover from?  Does this inconsistent data hurt the rest of our system?  Knowing that your user could loose connectivity right in the middle of some process is very important information to have, and designing your systems to be able to deal with this situation is only going to make them stronger for everyone.


## Bandwidth

Bandwidth is probably one of the least thought about elements when it comes to developing a new application.  We as developers are all very spoiled when it comes to internet connectivity, we have always on high speed broadband internet connections at home and work and just assume the rest of the world does also.  Unfortunately that isn't the case, many people only experience the internet through their mobile devices, and many carriers (most last time I checked) have bandwidth limitations and even charge for overages.  How is our application going to treat our users limited bandwidth?  Are we going to be good citizens and use as little bandwidth as possible, get in, get out, get our work done?  Or are we going to expect our applications to call home for every little thing the user might do (e.g. autocomplete a location by looking up results and narrowing them down for every character entered into a form)?  You users won't be thanking you, or coming back if using your application means they go over their data limit every month.

One quick example, I worked on an application where the minimum number of network calls to the server would have consumed a 250Gb data plan within 10 days of a typical users usage.  How could we justify asking our users to use an application that would cost them more money every month?  Needless to say that system never took off.


## Data

Data isn't just the storage of information it's also the way in which you expose that information to your users.  Many API's start out as simple [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) end points.  While this is a great way to start it's an extremely chatty interface, meaning it takes lots and lots of calls into the system to get all of the needed data for an application to display.  So while it's great to start here, don't think you are done because you can Create, Read, Update, and Delete your data.  You need to think about higher level abstractions of your data above CRUD I always see an aggregation layer that allows for interacting with your data in batches.  Along side, and maybe above, this aggregation layer I envision a composition layer where you can join multiple data points together.  Finally, if it's needed I see domain specific APIs that allow interaction with your data in a way that matches your domain.


## Content

I know, content sounds a lot like data but I've separated it out for a very specific reason.  When I talk about content I'm talking mainly about information in your application that doesn't really fall into the category of data.  It's easy to define data.  Data is a username, a rating (star or thumbs up) on a post, a phone number, what is a post or a comment on a post?  Are they just data or are they content?  I tend to think of these things as content because what they contain isn't as cut and dry as a data field.  So for this section lets think about a post and reply threaded discussion.

When your user submits a post will they just submit text and be done with it, or will they submit text with some formatting?  **Bold**, *Italic*, ~~strikethrough~~?  What about images or links to other content?  I'm sure many of you are already thinking about integrating [TinyMCE](http://www.tinymce.com/) or another WYSIWYG editor.  But how will that content be created or viewed on a mobile device?  Are we just going to embed web-views throughout our app so that we can display this content?  Are we going to strip out all of the HTML and just display plain text?  When designing for **Mobile First** these are the questions we need to answer.  Personally I prefer to store this content in a format that is display agnostic while still containing formatting properties.  Would you be surprised to know that this Blog is written and stored in one of these formats?  Or that my [resume](/resume) also is?  Do you know why I've chosen to store this content in this way?  Perhaps viewing my resume like [this](/resume.html) will give you a hint.  Both of those views of my resume come from the same content, that content happens to be [Markdown](http://daringfireball.net/projects/markdown/).  By storing my content in this display agnostic format I'm able to reformat it on the display side, without changing the content, to best fit the medium where it will be consumed.

By using a display agnostic format for storing your content you give your front-end systems the freedom to display it in whatever way is best for them.  Which in turn will be the best way for your users to consume that data on the device they happen to be using.


## Authentication

Authentication is another one of those places where very little thought is given to how a mobile user will interact with it.  Most people feel that asking the user to login before using the system every time is just fine as we don't want to leave a session open for hijacking on a public terminal.  However, most mobile users don't let other people use their devices and so once they've logged into their device they expect to already be logged into their applications.  This drives security people nuts, but if you don't think about how your user is going to use your application and conform somewhat to their expectations you aren't going to have happy returning users.

Another example, I worked with a team that wanted to allow for [OAUTH 2](http://oauth.net/2/) authentication and use [SAML Tokens](http://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) for authorization in what would be a [white label](http://en.wikipedia.org/wiki/White-label_product) application that was branded for each customer.  While jumping through re-direction hoops on the web to allow a 3rd party to authenticate your user is normal, and tends to give the user a good feeling (I know I like when google asks me if I want to let a 3rd party site authenticate, mainly so I don't have to remember yet another password), doing this on a mobile app that is already branded for the organization I'm logging into is strange and leads to confusion for the user.  As a side note in this solution the SAML Tokens added 4k to every network call killing the users Bandwidth.

So when developing and Authentication system take the time to think about the mobile users who may be expecting to stay "logged in" for weeks or months at a time, how often do you enter your gmail username/password into your phone?

## Conclusion

**Mobile First** isn't about building the next great mobile application, it's about building the next great application that performs as well on mobile as it does on the desktop.  More people have mobile devices capable of running your mobile application than have desktops capable of viewing your web page.  If that isn't reason enough to build your application to serve them as well as you serve your web users then I don't know what is.