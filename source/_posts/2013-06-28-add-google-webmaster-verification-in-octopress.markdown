---
layout: post
title: "Add google webmaster verification in octopress"
date: 2013-06-28 19:34
comments: true
categories: [blog, octopress, google webmaster, verification]
---

### Google Webmaster verification

I deployed my blog on github. So I can't use DNS verification. I used HTML verification. It require you to put a specific html file on your root directory.

### How to add the file in Octopress

I tried to place the file in /public folder every time before I run `rake deploy` to deploy my blog. This method is too much trouble and sometime I will forget to do it.

To solve this problem, go to _config.yml file in Octopress.

Add this line:

`include: ["googlexxxxxx.html"]`

And place the googlexxxxxx.html file under source directory.

You can try to remove the whole public directory and run `rake generate`. You will see the google html file is also inside.
