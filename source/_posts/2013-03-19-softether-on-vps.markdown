---
layout: post
title: "SoftEther on VPS"
date: 2013-03-19 19:06
comments: true
categories: [vps, vpn, ops, sysadmin]
---

I saw a post on [lowendtalk](http://www.lowendtalk.com/discussion/8783/softether-very-powerful-easy-to-use-multi-protocol-vpn-software) last week introducing a new piece of vpn software call SoftEther.

### What is [SoftEther][1]
A Free ​Cross-platform Multi-protocol VPN program,
as an academic project from [University of Tsukuba](http://www.tsukuba.ac.jp/english/).

It is a single server which support SSL-VPN (HTTPS) and 6 major VPN protocols (OpenVPN, IPsec, L2TP, MS-SSTP, L2TPv3 and EtherIP).

I would like to also highlight the [VPN over ICMP and VPN over DNS][2] feature if you are inside a very strict network.

### VPS Setup Guide

Requirement: VPS

I am using vps from buyvm. I got from a year deal which is USD12/year.

* Spec:
	* RAM: 128MB
	* Burst: 256MB
	* Disk Space: 15GB
	* Bandwidth: 500GB
	* Location: Buffalo, USA
* OS:
	* Debian 6.0 64bit (minimal)


Make sure you login root as follow:

![login as root](/images/softether_vps/login.png)

Before installing SoftEther Server let us install some prerequisite.
{% include_code build_tools.sh %}

Go to [SoftEther][3] to download the server binary.


![download selection](/images/softether_vps/download_selection.png)

I am using Ver 1.00, Build 9029, rc2 in this tutorial.

After download, run
{% include_code make.sh %}

Read the Agreement and press 1 three times.

Then we move the dir to /usr/local/
{% include_code install.sh %}

paste the following content to nano
{% include_code vpnserver.sh %}

ctrl-o ret ctrl-x

After that we enable the service
{% include_code service.sh %}

Then we need to do a checking.
```
./vpncmd
3
check
```

You should get 4 "Pass".

We need to set the server admin password
```
./vpncmd
1
ret
ret
VPN Server>ServerPasswordSet
```
Enter the admin password.

I do the setting on a windows client.
It is running


![server manager](/images/softether_vps/server_manager.png)

Install it, run it, click New setting. Input ip and password.


![input setting](/images/softether_vps/setting.png)

Then connect to it.
On successful login, it will pop up Easy Setup


![easy setup](/images/softether_vps/easy_setup.png)
Check Remote Access Server and Next


![warning](/images/softether_vps/warning.png)

Yes

![Virtual Hub name](/images/softether_vps/virtual_hub.png)

OK

![Setup L2TP](/images/softether_vps/l2tp.png)
Check Enable L2TP Server Function and input your own pre-share key

OK

![Azure](/images/softether_vps/azure.png)
We disable Azure VPN

![Create User](/images/softether_vps/create_user.png)


Then we need to create a new user. This is trivial.


![User](/images/softether_vps/user.png)

![Manager](/images/softether_vps/manager.png)
Click on "Manage Virtual Hub"

Click on "Secure NAT"


![Secure NAT](/images/softether_vps/secure_nat.png)


Enable the Virtual NAT

### Done
You can try to connect to the vpn server.


[1]: http://www.softether.org/
[2]: http://www.softether.org/1-features/1._Ultimate_Powerful_VPN_Connectivity#1.6._VPN_over_ICMP.2C_and_VPN_over_DNS_(Awesome!)
[3]: http://www.softether-download.com/en.aspx?product=softether
