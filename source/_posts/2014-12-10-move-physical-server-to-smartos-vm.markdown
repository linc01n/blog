---
layout: post
title: "Move Physical server to SmartOS VM"
date: 2014-12-10 23:23:17 +0800
comments: true
categories: [sysadmin, ops, development-setup, smartos, kvm]
---

# Background
In our office we got a dev server running centos. It is server grade machine with ECC ram and SAS hard drive. The problem with that it used to use by one dev. As the team is expanding, we need more dev server to test our code. Because I am familiar with SmartOS so we will use SmartOS as the hypervisor.


# Preparation

### Clone Disk
We need to do a clone on our hard disk. Since we only have one SAS hard drive with 1 partition, I need to plug in a external hard drive to clone the drive.

First we boot into [GParted](http://gparted.org/) with live CD.
Then we issue this command


```
# dd if=/dev/sda of=(external harddrive) bs=1M
```


### Erase Disk

After the disk clone we can plug in the SmartOS live usb and format the hard drive.

# Create KVM

After SmartOS installation, we now create a new KVM

```json /opt/new_vm.json
{
  "brand": "kvm",
  "vcpus": 1,
  "autoboot": true,
  "ram": 2048,
  "alias": "old_machine",
  "host": "old_machine",
  "resolvers": ["8.8.8.8", "8.8.4.4"],
  "disks": [
    {
      "boot": true,
      "model": "ide",
      "size": 153600
    }
  ],
  "nics": [
    {
      "nic_tag": "admin",
      "model": "e1000",
      "ip": "192.168.xx.xx",
      "gateway": "192.168.xx.1",
      "allow_ip_spoofing": true,
      "primary": true,
      "netmask": "255.255.255.0"
    }
  ]
}
```

Replace `ip` and `gateway` with your network settting.

Why I use `allow_ip_spoofing` is because my old machine got multiple static ip setup.

Create kvm by

```
# vmadm create < /opt/new_vm.json
```

You will get a uuid of the machine after this command.

```
Successfully created VM 226d5a1d-973e-4b4e-8158-ee33cc81e3f5
```

# Restore old machine

Now we copy back the old machine disk image to the newly created KVM.

```
# dd if=(external harddrive) of=/dev/zvol/226d5a1d-973e-4b4e-8158-ee33cc81e3f5-disk0
```

# Boot and test

After the disk copy finish, now you can boot up your vm and check if everything is working.

```
# vmadm boot 226d5a1d-973e-4b4e-8158-ee33cc81e3f5
```


