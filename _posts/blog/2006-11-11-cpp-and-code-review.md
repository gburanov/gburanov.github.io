---
layout: post
title: "C++ and the process of code review"
modified:
categories: blog
excerpt:
tags: [cpp, review]
image:
  feature:
date: 2006-11-11T00:00:00
---

# C++ and the process of code review

One of the big differences between C++ and Java or .Net Framework for example is the way you write your code. .Net Framework or Java have it's own code style, that is a part of a language and everybody who writes code must obey this code style. For example, exceptions is a must, OOP programming is a must.

C++ is more flexible and allows programmer to write code anyway he wants. If it is a low-level API, is can use function return values instead of exceptions and if you prefer to use functional programming over OOP - it's your choice. If you want you can still use macroprogramming, there is #define.

The con of this design is that still one project limit this possibilities to write code. For example, in C++ you can return errors using return codes and using exceptions, but in our low-level project we must not use exceptions. But, before committing the code, the "eye" review by the other developer must be done. You see, what is prohibited on the level of language in Java and .Net Framework is allowed in C++ but this leads to more reviewing time.

Here is example of C++ code review in our project

```
void Task::execute(const TaskParames& params)
{
  if (!Validate(params))
  {
   return bad_param_exception(L"Params validation not passed");
  }
  executeInternal(params);
}
```

This code will not pass code review. The correct code is:

```
bool Task::execute(const TaskParames& params)
{
  bool ret = false;
  if (Validate(params))
  {
    ret = executeInternal(params);
  }
  return ret;
}
```

This is just one example of code style in one separate project can differs from code style in another project. So, every developer should spend some time understanding project coding rules. In .Net Framework it is also true, of course, but returning error by return value is prohibited somehow on the level of language, not on the level of project, so every developer (who knows .Net Framework) knows about the rule.

The problem is that the example I showed you is easy and simple. But there are examples of the code where it is not so easy to say if we should allow something or deny it. For example, macroses are generally prohibited in our code (as well as in all c++), but if you are familiar with C++, there are places where it is easier (and more beautiful) to use macros instead of another solution (function, template or whatever)
