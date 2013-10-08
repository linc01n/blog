---
layout: post
title: "Using Parallels Desktop as Vagrant Provider"
date: 2013-10-05 21:40
comments: true
categories: [coding, ruby, vm, vagrant]
---

### Vagrant is great!

#### What is vagrant?
[Vagrant](http://www.vagrantup.com/) provides easy to configure, reproducible, and portable work environments. In other words, It creates a VM for development.

I use vagrant to do my development in the past year. Slowly learning all the pros and cons of vagrant when using it for development. Vagrant use VirtualBox as a VM host. There is a problem about the license of the guest addition of VirtualBox. If I want to use it commercially I need to purchase a license from Oracle(USD50 for 100 end users). Problem is 100 licenses min buy.

#### Vagrant can use VMWare as a host
Later version of vagrant abstract the VM provider as a provider plugin. This open the possibility to support other hypervisor.

###I own a Parallels Desktop 8 license
It would be great if I can use PD8 as my vagrant host. Now the dream came true. [https://github.com/yshahin/vagrant-parallels](https://github.com/yshahin/vagrant-parallels). Vagrant Parallels plugin!

I tested it for almost 2 days. Everything works fine. I can do `vagrant ssh` `vagrant up` `vagrant halt`.

The site provide one box for you to use. The box for vagrant-paralles is a bit special. I want Ubuntu 12.04.3 LTS for one of my project. So I decided to build the box myself.


### Veewee easy way to build vagrant box
[Veewee](https://github.com/jedi4ever/veewee) is a tool for easily (and repeatedly) building custom Vagrant base boxes, KVMs, and virtual machine images.

Veewee supports parallels export 15 days ago. It got almost all the common linux distribution.

### Building my own parallels box is not that easy

First, you need to install the parallels SDK for python. If you are using homebrew version of python you need to symlink the SDK separately.

`ln -s /Library/Frameworks/ParallelsVirtualizationSDK.framework/Libraries/Python/2.7/prlsdkapi /usr/local/lib/python2.7/site-packages/`

Then you need to create a definition for the box. I use

`veewee parallels define 'ubuntu-12.04-lts' 'ubuntu-12.04.3-server-amd64'`

In the definition directory edit the *definitions.rb* file. Remove virtualbox and vmware. We don't need those since we are creating for PD.

Let's build the box with

`veewee parallels build 'ubuntu-12.04-lts' --workdir=/path/to/wd`

Wait a while the os is installing. Here comes with a gotcha. If you get stuck in install grub-pc, you need to reboot you mac. I don't know why but it works. I also found we cannot do unattended install of Parallels tools. After the build is complete, do not shutdown the instance. Install the tools using ssh with vagrant/vagrant login.

The last step of creating the box is

`veewee parallels export 'ubuntu-1204-lts' --workdir=/path/to/wd`

### Vagrant up

So after importing the box, created the new Vagrantfile. We can start the vagrant box.

`vagrant up --provider=paralles`

Use the provider flag on the first time boot.


### Everything is just like VirtualBox

The networking config in Vagrantfile seems still not support. But this plugin is only 0.0.4. We will see.

