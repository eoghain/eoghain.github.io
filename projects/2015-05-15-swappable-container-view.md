---
layout: project
title: Swappable Container View
repo: https://github.com/eoghain/RBSwappableContainerView
image_url: https://raw.githubusercontent.com/eoghain/RBSwappableContainerView/master/Screenshots/TransitionAnimation.png
date: 2015-05-15
---

An implementation of [Objc.IO's](www.objc.io) [Custom Container View Controller Transitions](http://www.objc.io/issue-12/custom-container-view-controller-transitions.html) with inspiration from Michael Luton's [custom container view controllers](http://sandmoose.com/post/35714028270/storyboards-with-custom-container-view-controllers) blog post.

My goal was to allow for the swapping out of a content view while leaving my navigation controls in place in my parent UIViewController.  With very little code in your main ViewController you can now have a container that changes it's content with animations while still having nice encapsulation.

[Repository]({{ page.repo }})

![Example]({{ page.image_url}})