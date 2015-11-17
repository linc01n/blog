---
layout: post
title: "Faraday::SSLError"
date: 2015-11-17 23:57:52 +0800
comments: true
categories: [ruby, osx, rvm, rails]
---

#SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed

When you are using rvm and upgrade osx to Yosemite, this error may happen when you are developing for a rails app.

#solutions

To solve this, the simplest solution is

```
rvm reinstall 2.2.3 --disable-binary
```

this will recompile ruby from scratch and it should solve the problem.
