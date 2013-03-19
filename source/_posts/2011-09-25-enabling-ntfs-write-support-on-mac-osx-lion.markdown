---
layout: post
title: "Enabling NTFS Write Support on Mac OSX Lion"
date: 2011-09-25 22:25
comments: true
categories: [osx, mac, development setup, tutorial]
---

##Mac OSX Lion weaknesses

I have just bought my new Macbook Pro and setting it up as my dev box. Mac OSX has out-of-the-box support for NTFS read. If you want to transfer file between OSes you will choose FAT32. But FAT32 has one big problem which is no file can exceed 4GB size limit. 

##Setup procedure: fuse4x + ntfs-3g
Install Xcode 4 from App store [Link](http://itunes.apple.com/app/xcode/id448457090?mt=12)

After download, Click the "Install Xcode" from the Launchpad. (I really don't understand why Xcode behave like this when it comes to App Store. : ( )

Then open Terminal:
Press ⌘ + Space then type Terminal

Install Homebrew

```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.github.com/gist/323731)"
```

Install fuse4x + ntfs-3g

```
$ brew install ntfs-3g
```

Load fuse4x kext to kernel

```
$ sudo cp -rfX /usr/local/Cellar/fuse4x-kext/0.8.12/Library/Extensions/fuse4x.kext /System/Library/Extensions
$ sudo chmod +s /System/Library/Extensions/fuse4x.kext/Support/load_fuse4x
```


Setup Auto-mount

```
$ sudo mv /sbin/mount_ntfs /sbin/mount_ntfs.orig
$ sudo touch /sbin/mount_ntfs
$ sudo chmod 0775 /sbin/mount_ntfs
$ sudo chown 0:0 /sbin/mount_ntfs
$ sudo nano /sbin/mount_ntfs
```

After the last command, a text editor will open. Paste the following code to the editor.

```
#!/bin/bash

VOLUME_NAME="${@:$#}"
VOLUME_NAME=${VOLUME_NAME#/Volumes/}
USER_ID=501
GROUP_ID=20
TIMEOUT=20

if [ `/usr/bin/stat -f "%u" /dev/console` -eq 0 ]; then
        USERNAME=`/usr/bin/defaults read /library/preferences/com.apple.loginwindow | /usr/bin/grep autoLoginUser | /usr/bin/awk '{ print $3 }' | /usr/bin/sed 's/;//'`
        if [ "$USERNAME" = "" ]; then
                until [ `stat -f "%u" /dev/console` -ne 0 ] || [ $TIMEOUT -eq 0 ]; do
                        sleep 1
                        let TIMEOUT--
                done
                if [ $TIMEOUT -ne 0 ]; then
                        USER_ID=`/usr/bin/stat -f "%u" /dev/console`
                        GROUP_ID=`/usr/bin/stat -f "%g" /dev/console`
                fi
        else
                USER_ID=`/usr/bin/id -u $USERNAME`
                GROUP_ID=`/usr/bin/id -g $USERNAME`
        fi
else
        USER_ID=`/usr/bin/stat -f "%u" /dev/console`
        GROUP_ID=`/usr/bin/stat -f "%g" /dev/console`
fi

/usr/local/bin/ntfs-3g \
        -o volname="${VOLUME_NAME}" \
        -o local \
        -o noappledouble \
        -o negative_vncache \
        -o auto_xattr \
        -o auto_cache \
        -o noatime \
        -o windows_names \
        -o user_xattr \
        -o inherit \
        -o uid=$USER_ID \
        -o gid=$GROUP_ID \
        -o allow_other \
        "$@" &> /var/log/ntfsmnt.log

exit $?;
```

The USER_ID should be 501 if you are the only user of your machine. or you can update the USER_ID and GROUP_ID by typing this to check your user and group
```
$ id -u && id -g
```
which the first line is user id, second line is group id.

*source: [fernando's blog](http://fernandoff.posterous.com/ntfs-write-support-on-osx-lion-with-ntfs-3g-f) (I edit the path of ntfs-3g since we install in Homebrew)*