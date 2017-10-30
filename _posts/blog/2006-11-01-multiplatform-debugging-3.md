---
layout: post
title: "Remote debugging for Mac Os X (part 3)"
modified:
categories: blog
excerpt:
tags: [cpp, debugging]
image:
  feature:
date: 2006-11-01T00:00:00
---
# Remote debugging for Mac Os X (part 3)

As you probably know, Mac Os X is based on FreeBSD Unix, so the methods of debugging are pretty the same as for Linux. We need to mount the sources and symbol table and then set correct way to them.

However the process of mounts differs slightly from Linux. The Mac Os X has GUI interface to do all the mounting. Switch to Finder and press Command+K to open connection dialog.

![]({{ site.url }}/images/blog/2006-11-01-multiplatform-debugging-3/debug_1.png)

Then choose the folder to mount (folder with source codes)

It is also possible to mount folder automatically on logon. To do this, open System Preferences, click Accounts icon, choose Login Items tab and choose folder you have mounted.

![]({{ site.url }}/images/blog/2006-11-01-multiplatform-debugging-3/debug_2.png)

Now, about Mac Os X gdb. Mac Os X gdb is hacked version of Unix gdb. Because of the hacks, it does not understand some gdb commands, including PATH command. It means, that creating .gdbinit file will not help. The only workaround for this case is to create the folder structure with the “/” root. To do this, type the following

```
sudo mkdir /cygdrive
sudo mkdir /cygdrive/g
ln -s /Volumes/Source /cygdrive/g/Source
```

Now you are ready to do all the debugging staff using gdb.

![]({{ site.url }}/images/blog/2006-11-01-multiplatform-debugging-3/debug_3.png)

Speaking about GUI, you can use XCode, that is installed with Mac Os X (I will not go into details, the interface is somehow intuitive) or, alternatively, you can use Netbeans (if you are used to Linux)

![]({{ site.url }}/images/blog/2006-11-01-multiplatform-debugging-3/debug_4.png)
