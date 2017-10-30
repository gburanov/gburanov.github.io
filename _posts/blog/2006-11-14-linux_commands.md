---
layout: post
title: "Linux commands that help me to create a correct environment for debugging"
modified:
categories: blog
excerpt:
tags: [linux]
image:
  feature:
date: 2006-11-14T00:00:00
---

# Linux commands that help me to create a correct environment for debugging

To begin with, I am rather inexperienced Linux developer. All 4 years of commercial development I have used Windows + Visual Studio only. Building software for many platforms is not common for me. So, I created a list of commands (as well as software) and other hints that help me to debug my software on Linux. When I say Linux, it 75% means Mac Os too.

And yes, I am using Ubuntu 8.10.

## Gdb in Ctrl+X+A GUI mode.

Usually I am debugging code using Netbeans, but there are situations where I need to debug code on remote computer, but installing Netbeans or even set up gdbserver will take too much time. So, I just use Ctrl+X+A keys and gdb switches to simple GUI mode. Very useful

![]({{ site.url }}/images/blog/2006-11-14-linux_commands/gdb_gui.png)

## Ldd

This programs shows library dependencies of some executable file. If the file is not executed for some reason, the first thing we need to check is ldd. In windows the LIB (included with VS) do something like the same.

```
gburanov@gburanov-ubuntu:~$ ldd /mnt/G:/Source/project/exe/lx/debug/english/schedmgr.exe
 linux-gate.so.1 =>  (0xb80b1000)
 libpthread.so.0 => /lib/tls/i686/cmov/libpthread.so.0 (0xb806a000)
 librt.so.1 => /lib/tls/i686/cmov/librt.so.1 (0xb8061000)
 libstdc++.so.6 => /usr/lib/libstdc++.so.6 (0xb7f71000)
 libdl.so.2 => /lib/tls/i686/cmov/libdl.so.2 (0xb7f6d000)
 libm.so.6 => /lib/tls/i686/cmov/libm.so.6 (0xb7f47000)
 libgcc_s.so.1 => /lib/libgcc_s.so.1 (0xb7f38000)
 libc.so.6 => /lib/tls/i686/cmov/libc.so.6 (0xb7dda000)
 /lib/ld-linux.so.2 (0xb8097000)
gburanov@gburanov-ubuntu:~
```

## fuser

Shows who owns the file. In windows I always used Unlocker application for it (installed separately).

```
gburanov@gburanov-ubuntu:~$ fuser /mnt/G:/Source/project/exe/lx/debug/english/schedmgr.exe
/mnt/G:/Source/project/exe/lx/debug/english/schedmgr.exe: 17846e
gburanov@gburanov-ubuntu:~$
```

## dmesg

Shows you the kernel bootup messages. Used together with grep. Example:

```
gburanov@gburanov-ubuntu:~$ dmesg | grep eth0
[    4.552705] eth0: registered as PCnet/PCI II 79C970A
[   79.763326] eth0: link up
[  300.200990] Inbound IN=eth0 OUT= MAC=00:0c:29:d1:4e:30:00:00:aa:9e:33:95:08:00 SRC=10.2.144.8 DST=10.2.148.57 LEN=83 TOS=0x00 PREC=0x00 TTL=30 ID=6445 PROTO=UDP SPT=161 DPT=42372 LEN=63
[  300.210026] Inbound IN=eth0 OUT= MAC=00:0c:29:d1:4e:30:00:14:38:da:c4:85:08:00 SRC=10.2.148.62 DST=10.2.148.57 LEN=83 TOS=0x00 PREC=0x00 TTL=64 ID=61780 PROTO=UDP SPT=161 DPT=42372 LEN=63
[  300.222510] Inbound IN=eth0 OUT= MAC=00:0c:29:d1:4e:30:00:00:aa:9c:e1:3b:08:00 SRC=10.2.144.125 DST=10.2.148.57 LEN=83 TOS=0x00 PREC=0x00 TTL=64 ID=4541 PROTO=UDP SPT=161 DPT=42372 LEN=63
[  311.040399] eth0: no IPv6 routers present
```

## which, whereis, type

Which shows you the full path to the specific executable file, whereis do the same + shows you manuals and sources. Type shows you alias for the command

```
gburanov@gburanov-ubuntu:~$ whereis ls
ls: /bin/ls /usr/share/man/man1/ls.1.gz
gburanov@gburanov-ubuntu:~$ which ls
/bin/ls
gburanov@gburanov-ubuntu:~$ type ls
ls is aliased to `ls --color=auto'
gburanov@gburanov-ubuntu:~$
```

## slocate

Slocate is more generalised version of which and whereis. It performs a quick search over database with the list of all files. The database is updated using cron every day. Example:

```
gburanov@gburanov-ubuntu:~$ slocate trueimage
/var/log/trueimage-setup.log
/usr/sbin/trueimagecmd
/usr/sbin/trueimageremote
/usr/sbin/trueimagemnt
/usr/lib/Acronis/Agent/libag_trueimage.so
/usr/share/man/man8/trueimagecmd.8.gz
/usr/share/man/man8/trueimagemnt.8.gz
/home/gburanov/Acronis/ATI Echo 2/.trueimageconsole
gburanov@gburanov-ubuntu:~$
```
