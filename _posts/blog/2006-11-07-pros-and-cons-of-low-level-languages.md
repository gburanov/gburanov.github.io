---
layout: post
title: "Pros and cons of low level languages"
modified:
categories: blog
excerpt:
tags: [cpp, design]
image:
  feature:
date: 2006-11-07T00:00:00
---
# Pros and cons of low level languages

Some time ago I’ve talked about DST problem with one of the Java developers and the first question he asked to me was: “Hey, bullshit, I guess you don’t even have a single class for the string”. I think the difference between C++ and Java can be put down to topic “high and low level languages”

The C++ was initially designed this way – the language is just an OO wrapper over plain C, nothing more. The initial design was to leave the language as simple as it is, and move all concepts implementations to libraries. That’s we don’t have string data type, like in .NET Framework or Java. That’s why we don’t have an operator to double the value or to get square root from value. That’s why the C++ without libraries is just nothing.

Of course we have STL – standard template library that is a part of C++ and it has std::string class (for ANSI strings) and std::wstring (for Unicode strings) and 99.9% that you need to use this class to work with strings. But, even talking about time concept, let’s look what we have in C++.

|Structure|Description|What is it|
|:------------: | :-----------: | :-----------: |
|time_t|ISO time standard|seconds since 1970-Jan-01|
|FILETIME|Windows time standard|Ticks (1 tick = 100 nanoseconds) since 1601-Jan-01|   
|SYSTEMTIME|Windows time standard|structure with date, time year and so on|
|Tm|ISO time structure|structure with date, time year and so on|
|UDate|ICU time|milliseconds since 1970-Jan-01|

Moreover, we have Mac Os time format, Java time format and so on... It is common that a third library invents its own standard for time concert, and if you use the library in the development, you need to write conversion routines. At this point you need to cry out: “Oh, C++ is horrible, every guy is inventing its own bicycle”

## Procs

But this is not always bad as you can think and this is one of the reasons why C++ is so popular for more than 20 years. Let’s imagine string was added as a data type to the C++ in the beginning of 80’s. That was the time when nobody was thinking about localisation and globalization, and I guess that would be ANSI string. Then, with the invention of Unicode this string (that is basic data type) becomes obsolete and need to be banned. That’s shit. Most things are subject to change, and they implementation must be in a library, not in language.

Another reason why do we have so many time concepts is looking for trade-off between memory and usability. Java is a new modern language, and, when one is writing Java application, he will not use the same memory optimisations as C++ developer does, even now. I just want to notice, that Java date is 64-bit data type, and C ISO time is 32-bit data type. Sometimes, even if the initial design of the library is perfect, it can become obsolete. Java is young language and I think (I am not sure, haven’t tried it actually) it does not have many obsolete classes, but is it just the matter of time.

Also, you need to node that even a string can’t be a common concept if we are talking about low-level programming. Everybody needs his own tradeoffs between speed, memory and easy of usage. For example, the Windows core team does not use std::vector but it’s own [DynArray class](http://sim0nsays.livejournal.com/31460.html).

## Cons

The cons of this concept are, yes, writing too many bicycles. Almost every huge project has it’s own implementation of even basic concepts. One of the popular mistakes in our company (at least for the newbie) is not using the primitives, developed mostly by our developers.

For example, one can write

```
std::string guid1 = “0FEC22BC-64B1-4f98-AD93-5B870095C911”;
std::string guid2 = “0FEC22BC-64B1-4f98-AD93-5B870095C912”;
assert(guid1 != guid2);
```

But this code will not complete the process of review, because we need to use special Guid class.

```
Common::Guid guid1 = “0FEC22BC-64B1-4f98-AD93-5B870095C911”;
Common::Guid guid2 = “0FEC22BC-64B1-4f98-AD93-5B870095C912”;
assert (guid1 != guid2);
```

For example, the .Net developer will no doubt use Guid class from the .Net Framework class library.

```
Guid g = Guid.NewGuid();
```

He knows it because it is a well documented part of .Net Framework. Because it is a part of standard, the time to understand the code, written by another developer, decrease.

I need to note that C++ has boost library that is somehow not part of C++, but what is going to be part of C++ (at least it is discussed now). The boost sometimes fixes the problem with inventing bicycles.
