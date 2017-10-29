---
layout: post
title: "The true downsides of C++"
modified:
categories: blog
excerpt:
tags: [Zsh, bash]
image:
  feature:
date: 2008-07-14T15:39:55
---
# The true downsides of C++

I am currently working as Ruby software developer, and this is quite uncommon for the developer that used to be C++ developer in the past.

Usually, when I tell somebody about my previous experience, they reply: “Oh, that damn C++. We heard there are so many problems…”. And when I reply: “Oh, this is interesting, please tell me more”, they usually start with: “Well, I heard that memory management is very complicated…”. And I was saying that this is not true for so long time, so that I decided to create a post about it.

So, what are illusion (fake) problems of C++

## Memory leaks

Well, that used to be true some time ago. In modern C++ this is definitely not. Smart pointers rule the world of modern C++ development, and they allow you to write nice and almost memory-leak free code.

```
shared_ptr<Type> type(new Type(...));
type->Call(...)
```

The general idea is that use pass objects instead of pointers, and this objects manage the lifetime of the instance. One of the possible implementations is reference counting.

## Complexity of the language itself

This is only partly true. Some subsets of the language (for example templates) are complicated, especially if you read Alexandresku but mostly you can just stay out of these features.

Multiple inheritance is also hard, but you usually can inherit one real class and abstract classes (interfaces). Macroses are hard, but just try not to use them.

Without all this features C++ is still powerful and simple languages, that is quite easy to use.
And what are the the real main problems of C++?

## No package manager

I think this is the biggest problem in modern C++. Language means not only syntax, but also infrastructure around it. And C++ lacks it.

All modern languages understand that package manager is critical, that’s why Rust has “cargo” from the very beginning. Python has pip, Ruby has gems and bundler. C++ got some prototypes, for example conan, but it is far from perfect.

That’s why all C++ developers are usually not reusing the existing code, but just write it from scratch.The only common library that is used everywhere in C++ is STL. And it lacks a lot of functionality.

I was once working on Chromium project (very big, and definitely has a lot of dependencies, and wanted to understand how do they deal with dependencies) and found out that they created their own solution – DEPS file.

## Does not apply for full stack development

C++ nowadays means you are working on small part of application, and this small part is usually quite low level. Even in modern desktop world, there be much better ways then Qt (for example, JavaScript) to build UI. If I need to name most common applications in the modern world than it will be

* Writing other languages
* Writing core functionality

I have worked in several companies where they choose to use C++ for all layers (including UI), and that was mostly waste of time and resources – there are much better ways to write it nowadays – for example .Net or even JavaScript.

Why is that bad? Sometime you code the black-box and don’t see the global results of your work.

## Why to learn C++ than?

But still I understand the great benefits from being the C++ developer. These guys usually understand better different algorithms and  data structures. Moreover, they usually understand the complexity of the new language easier then the other people.

So, to summarize I think that C++ is not very good for the industry nowadays. In web, it is better to take a look at Go, and in embedded world – at Rust. But hire ex-C++ developer is usually a good choice.
