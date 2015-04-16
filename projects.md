---
layout: page
title: Projects
permalink: /projects/
---
# Info Folder Collection View Layout

An `UICollectionViewLayout` that uses supplemental views to display a folder below a cell that looks like the screen was split. Mimics the pre iOS7 springboard groups visualization. Wrote this because all of the other "folder" controls like this use a screen shot to do the splitting, but I wanted my collection view to still be functional even with the folder open.

[Repository](https://github.com/eoghain/RBCollectionViewInfoFolderLayout)

![Example](https://raw.githubusercontent.com/eoghain/RBCollectionViewInfoFolderLayout/master/screenshots/portrait.png)

-----
# Balances Column Layout

A` UICollectionViewLayout` that displays your cells in a variable number of columns that fit to the bounds of the CollectionView. Why? Cause every other layout that attempts to mimic the Pintrest waterfall layout (as this does) wants you to set the number of columns from the outside and I wanted my layout to figure that out for me so I didn't have to deal with it in the rotation logic. Also I wanted a single layout setup for iPhone and iPad resolutions, and whatever comes next.

[Repository](https://github.com/eoghain/RBCollectionViewBalancedColumnLayout)

![Example](https://camo.githubusercontent.com/3e8ea957e2f0b64ed335e1b818f78d2789a61f23/68747470733a2f2f7261772e6769746875622e636f6d2f656f676861696e2f5242436f6c6c656374696f6e5669657742616c616e636564436f6c756d6e4c61796f75742f6d61737465722f496d616765732f726f746174696f6e2e706e67)

-----

# Stacks Controller

A navigation controller that works by stacking views on top of each other and allows for reveals to lower views.

[Repository](https://github.com/eoghain/RBStacksController)

----

# Easing Segues

A collection of segues that utilize the AHEasing library to add smoother transitions between views.

[Repository](https://github.com/eoghain/RBEasingSegues)

----

# Tree Manager

RBTreeManager is a set of classes built to easily manage hierarchical data in iOS TableViews. These classes were originally built because of a need to display threaded discussion posts (lots of them) in an iOS application. Unfortunately TableViews want their data in a flat format (i.e. array) but we all like to have our data reference it's parents/children so we can easily navigate it. So this was my solution.

[Repository](https://github.com/eoghain/TreeManager)