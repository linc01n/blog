---
layout: post
title: "Cloudflare messup with our geo targeting ad serving"
date: 2013-06-13 23:16
comments: true
categories: [work, ad network, geo, targeting]
---

### Self-doubt on my code

I have joined a local AD network company as a senior developer(who got out of college one year ago). I joined this company for more than half a year. Mainly working on cleaning the code and infrastructure. I redesign the AD selection algorithm and implement it. I have learned a lot from this practice. Geo targeting is one of the selection criteria.

I received a support ticket from our operation team last week. We have launched a new campaign targeting US internet user but we are recording HK IP address. The first thought that struck me was I messed up the implementation(some background information: my new algorithm is testing on production. There is no time for proxy shadowing or mock test. SHIP IT before test it!).

I rethink the whole logic in my head step-by-step. The truth is I don't know what happen. Large scale production system is a beast. I still find myself clueless on other parts of the system.

I didn't manage to figure out what happen so our team decided to checkout all the statistic we've got dig deeper to see what happen.

### Cloudflare is caching javascript

I visited the publisher site and found out our script tag type become 'text/rocketscript'. After some googling it is a thing called RocketLoader from Cloudflare.

This thing messes up with geo targeting for all of our ads. After some research, [AdvretServe](http://www.advertserve.com/blog/2013/04/cloudflare-and-rocketloader/) and [Google Adsense](http://udinra.com/blog/google-adsense-cloudflare-issue-and-solution) are also having this problem.

Our javascript content is different per request. Cloudflare execute our javascript and save it. I know static content should use CDN to speed up things. But some ad network is not using static js. I don't know much but at least all the 3-rd party agency we worked with are using dynamic js.

### Solution (which is not really a solution)

We need to ask publisher to disable Cloudflare. Cloudflare not only affect geotargeting campaign. We later find out that it also lower the publisher total ad serve. We are still investigating what's the issue. But we will list Cloudflare as not compatible with our network.
