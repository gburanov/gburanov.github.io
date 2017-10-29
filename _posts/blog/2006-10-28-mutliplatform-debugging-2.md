---
layout: post
title: "Multiplatform debugging environment, Part 2 (Linux)"
modified:
categories: blog
excerpt:
tags: [cpp, debugging]
image:
  feature:
date: 2008-10-28T00:00:00
---
# Multiplatform debugging environment, Part 2 (Linux)

## Remote debugging for Linux

There is no port of Visual Studio Remote Debugger for Linux, so we cannot use the same technology as for Windows. Let’s think what we have on Linux. Yes, GDB. But gdb needs source code and symbol table to do all the debug stuff.

One solution is to use gdbserver on debug machine and connect to this server using gdb from computer with source codes (Windows XP). As far as I know VS does not work with gdb, so if we want to use GDB GUI, we need to install Netbeans or Eclipse on Windows. I don’t like this solution.

Another, better solution is to mount windows shared folder with source codes, symbol table, executable file and so on.

First of all, you need to create shared folder.
