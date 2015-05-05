---
layout: project
title: Stateful Table View Pattern
repo: https://github.com/eoghain/RBStatefulTableViewController
image_url: https://raw.githubusercontent.com/eoghain/RBStatefulTableViewController/master/Screenshots/TableViewStoryboard.png
date: 2015-05-01
---

Explicit state handling for `UITableViewController`.

This was built because most of the table views that I've built in the past require the ability to show a loading screen as well as an empty data set screen.  Unfortunately, when starting to design a table view most everyone starts by building the populated view, and then attempts to shoe horn in the loading, empty, and error states.

`RBStatefulTableViewController` and `RBTableViewState` help you to separate out the multiple states keeping their code clean, understandable, maintainable, and easily expandable.

[Repository]({{ page.repo }})

![Storyboard]({{ page.image_url}})