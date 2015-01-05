---
layout: post
title: "Installing an Octopress theme"
date: 2015-01-04 19:28:47 -0600
comments: true
categories: coding, blogging
---
Installing a theme in Octopress is trivial. I chose the "slash" theme because I prefer its sans-serif fonts and clean layout. Here's how I installed it:

```
$ cd git/blog # where my octopress repo is
$ git clone git://github.com/tommy351/Octopress-Theme-Slash.git .themes/slash
$ rake install['slash']
$ rake generate
```

Commit your changes if you are happy and you are done!
