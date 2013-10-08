---
layout: post
title: "Using gruff graphing library"
date: 2013-10-08 22:09
comments: true
categories: [ruby, development, graph]
---

### Requirement
I played around with [gruff](https://github.com/topfunky/gruff) today. The final result is really impressive. Especially it can visualize data from your database in the way you want.

Before you install this gem. You need to install few extra things. They are [imagemagick](http://www.imagemagick.org/script/index.php), [ghostscript](http://www.ghostscript.com/). On Mac OSX you need to do

`brew install imagemagick ghostscript`

After installing both packages, you can run `bundle install`

###Usage

To generate a graph you need to

```ruby
require "gruff"
require "date"

# Create the graph object
g = Gruff::Line.new

# Set the title
g.title = "My First Graph"

# Let say we have a data hash
# The element is like {Date => Integer}

today     = Date.today
yesterday = Date.today.prev_day


data = { yesterday => 100, today => 200}

# We need to set the x-axis labels
# It need to be a Hash { 0 => "name 1", 1 => "name 2", … }
# We need to do a little bit of data transform here.
g.labels = Hash[(0...data.keys.size).zip data.keys.map{|day| day.strftime("%d %b")}] # => {0=>"07 Oct", 1=>"08 Oct"}

# Then we need to set the y-axis, we need to give it a name
g.data :Price, data.values

# Write the generated graph to a file
g.write "gruff.png"
```

This is the generated graph:


![Generated Graph](/images/gruff.png)
