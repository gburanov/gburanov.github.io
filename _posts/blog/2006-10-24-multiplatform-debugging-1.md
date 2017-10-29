---
layout: post
title: "Multiplatform debugging environment, Part 1 (Windows)"
modified:
categories: blog
excerpt:
tags: [cpp, debugging]
image:
  feature:
date: 2008-10-20T00:00:00
---

# Multiplatform debugging environment, Part 1 (Windows)

## Introduction

Our product is compiled on many OSes, including Windows, Linux and Mac OsX. I have two work computers to do all the work. Our source control is CVS, and I have WinCVS installed (you can also use TortoiseCVS as well). These are just my preferences; any guy in the team (if he is rather experienced developer) can use any tool he wants to do the job. I said experienced, because there is recommended list of the developer tools, and there is something like official support for the problems with this tools. Anyway, there are guys on the project who use notepad to write code, command line to build project and gdb to debug it.

As I said, I have two work computers. Main computer is running Windows XP with Visual Studio 2005. It also has Virtual machine (I prefer VMware, there are guys who prefer Parallels, at least the Parallels developers work in the same building =)). The VMware is needed to run test environment. Generally, we have a rule: develop machine is for code writing and building only. If you want to run the code, use virtual machine (even if you want to run it on the same environment as development). Most of Acronis products do use low-level disk operations, so every bug in program can cause corrupting of developing environment.

Second computer is Mac OS. Actually, is it not mine only, we have one for the whole team. It is iMac (computer + monitor in the same brick), so anyone can easily move it from one seat to another. We use real computer, because Mac OS on VMware runs completely slowly.

All code (both for Linux, Mac Os and Windows) is compiled on the main computer using make files. For Linux and Mac Os we use cygwin to compile. But, as I said before, after the compiling you test the code on virtual machine or Mac Os machine.

The way of debugging differs for different operation systems.

## Remote debugging for Windows

For windows I use remote debugging tools (included with the VS 2005 or VS 2003). You need to install debugging monitor (just copy one folder) to the environment, where you program will be tested. First of all, disable authentication.

![]({{ site.url }}/images/blog/2006-10-24-multiplatform-debugging-1/debug_1.gif)

Now the monitor starts listening to connections

![]({{ site.url }}/images/blog/2006-10-24-multiplatform-debugging-1/debug_2.gif)

Switch back to main computer and load VS. Load your solution, and then open project properties.

![]({{ site.url }}/images/blog/2006-10-24-multiplatform-debugging-1/debug_3.gif)

Chose “Remote Windows Debugger”, type in remote server name and choose correct connection type (Remove with no authentication). In “Remote command” field type in the patch to the executed file on remote computer. That’s all! Your solution is ready for remote debugging.
