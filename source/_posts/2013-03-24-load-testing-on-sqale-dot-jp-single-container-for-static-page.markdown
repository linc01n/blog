---
layout: post
title: "Load testing on sqale.jp single container for static page"
date: 2013-03-24 12:49
comments: true
categories: [PaaS, sqale.jp, loading test, rails, loader.io]
---

I met with some amazing developers from [Sqale.jp](https://sqale.jp) when I attent [Ruby Conf TW 2012](http://rubyconf.tw/2012/) last year.

Sqale is a japanese company compete with [Heroku](http://www.heroku.com/) to provide developer friendly platform to host rails app.

Sqale is addressing some of the problems Heroku.

1. Server location - Heroku servers are in US which is quite far away if the target audience are in APAC area.
2. SSH access
   > Since your app is spread across many dynos in the dyno manifold, there is no single place to SSH into. You deploy and manage apps, not servers.[ref][1]
3. Slug size - Heroku limit it to a maximum 200MB.

Sqale provides server in Tokyo and gives developer a LXC container to work with. You can run cron job on Sqale without any changes if you migrate from dedicate server. So the platform dependent is lesser than Heroku.

For slug size, Sqale do not have a concept of slug. They give you 2GB of disk space for one container.


###Pricing model
Sqale is charge by number of containers. Each one is ¥940 per month which is around 10USD.

##How powerful is one container?
I am still testing out Sqale. Before I use Sqale in production I need to know what's my operation cost.

###Deploy a sample app
I always want to try out [Swordfish](https://github.com/github/swordfish). So I deployed it to sqale. The process is not too hard. But therer are lack of english documentation. I can only just guess what the console mean and try my best. Support from sqale is really helpful.

###Static file test
I don't want to test dynamic content yet. So I created a blank index.html in public/ directory and see how their nginx perform.

####10 concurrent request warmup for 60 second
<div style="width: 600px;">
<iframe  style="overflow: hidden;" scrolling="no" width='600' height='300' frameborder='0' src='//share.loader.io/results/9fc6e5f2ac4f708b91a86095c13d8762/widget'></iframe>
<div style="width: 100%; text-align: right;">
<a href="http://loader.io/results/9fc6e5f2ac4f708b91a86095c13d8762" target="_blank"  style="padding: 0 10px 10px 0; font-family: Arial, 'Helvetica Neue', Helvetica, sans-serif; font-size: 14px;">View on loader.io</a>
</div></div> 

No error and response is steady.

####1000 concurrent request for 300 second
<div style="width: 600px;">
<iframe style="overflow: hidden;" scrolling="no" width='600' height='300' frameborder='0' src='//share.loader.io/results/36216016f1884328b0b0e71451adcc37/widget'></iframe>
<div style="width: 100%; text-align: right;">
<a href="http://loader.io/results/36216016f1884328b0b0e71451adcc37" target="_blank"  style="padding: 0 10px 10px 0; font-family: Arial, 'Helvetica Neue', Helvetica, sans-serif; font-size: 14px;">View on loader.io</a>
</div></div>

####Conclusion
First error start to occur when concurrent connection is about 815. The total error rate is 3% is not bad.
You can scale out by adding more container to your application. But from what I understand from their website seems you can only add 4 more containers to your app. So maximum is 5 containers per app. I am not sure it is a technical reason or you can add more by contacting Sqale.

It also seems that they will help you do some ddos mitigation behind the scene. After the c1k rush I cannot run a smaller test on it. All requests are timeout.



   
[1]: https://devcenter.heroku.com/articles/one-off-dynos#ssh-access