---
layout: post
title: "Code review and gained experience "
modified:
categories: blog
excerpt:
tags: [cpp, review]
image:
  feature:
date: 2006-11-13T00:00:00
---

# Code review and gained experience

During code reviews I am trying to write down (to remember) all my defects (bugs in my code). Some of them are trivial (like “Functions names must be listed in alphanumeric order” or “Use Pascal casing for member variables”) but some are not so, and I want to list them here. Of course, one must understand that during code review it is possible to find only simple defects. If you have error in code algorithm, it is not likely to be found during code review.

After 3 or four bugs in code review, you will probably gain an experience to found this defects in your code “on fly”. The things noted here are well known. If you are an experienced developer, you probably know already about the stuff. In that case, let it be just a note.

## Using C++ cast style

Initially the C++ was just a wrapper over C and all casts in C (no matter if they are legal or not). C++ has not banned old style of cast, but invented four new casts const_cast, static_cast, reinterperet_cast and dynamic_cast.

Example (incorrect):

```
void*p = get_Widget_Poiner();
int32 i32 = 123;
int64 i64 = (int64)i32;
Widget* w = (Widget*)p;
```

Example (correct):

```
void*p = get_Widget_Poiner();
int32 i32 = 123;
int64 i64 = static_cast<int64>(i32);
Widget* w = reinterpret_cast<Widget*>(p);
```

It is a good style never to use old C cast in new code.

## Using smart pointers

Creation of objects using new/delete operators is possible but should be avoided. Different types of smart pointers must be used everywhere it is possible. In our code we use std::auto_ptr and boost::smart_ptr. auto_ptr is light weighted but it is not possible to use it in STL containers and as a member of classes, because of the problem with the ownership.

Example:

```
std::auto_ptr ptr1(new Widget);
std::auto_ptr ptr2 = ptr1;
//as this point of code the ONLY one who owns Widget object is ptr2. ptr1 "gave" the ownership to ptr2.

boost::smart_ptr ptr1(new Widget);
boost::smart_ptr ptr2 = ptr1;
//as this point both pt1 and ptr2 owns its own Widget object.
```

## Use const everywhere it is possible

C++ has a const keyword, although many developers just forget about it. After designing every new function interface, one must always think about cast.

Example (incorrect):

```
bool Processes::GetParentProcessId(PID& processId, PID& parentProcessId);
```

Example (correct):

```
bool Processes::GetParentProcessId(const PID& processId, PID& parentProcessId) const;
```

## Invalidate the function return in case of error

This is the rule I disagree with, but still it is our project rule. If the functions notifies the upper layer about error using error code it must invalidate it’s output.

Example (incorrect):

```
bool Processes::GetParentProcessId(const PID& processId, PID& parentProcessId) const
{
  parentProcessId = -1;
  //bla bla bla somewhere here we are crashed
  //...
  //...
  return false;
}
```

When this function returns false, the parentProcessId output is invalid. This is done for the following reason: if the guy who uses this function forgets to check the error code, he will not be able to continue working with parentProcessId (there can be a case when it is valid before GetParentProcessId execution).

Example (correct):

```
PID processId = GetProcessId(processId);
PID parentProcessId = processId; //this line of code is stupid but possible
GetParentProcessId(processId, parentProcessId);
//we forget to check the return code but parentProcessId is valid. It will be difficult to
//find this bug
```

## Single return point concept

There is no single rule about single return error concept, but different developers have different points of view on the problem. Using single return point, it is easier to debug code (you can be sure that it is the only exit point from the function) and easier to do free-resources operations on exit. Second point can be fixed inventing smart pointers (see 2), but point 1 is still important.

Example (correct):

```
bool fun1(int a, int b, int c)
{
  bool ret = false;
  if (a)
  {
    Do(b, c);
    ret = true;
  }
  return ret;
}
```

On the other point of view, when there are many validations in the code, the code that uses single return point concept becomes huge.It is better not to use the concept here

Example (incorrect):

```
bool fun1(int a, int b, int c)
{
  bool ret = false;
  if (a)
  {
    DoSmth();
    if (b)
    {
      DoSmthAgain();
      if (c)
      {
        Do(b, c);
        ret = true;
      }
    }
  }
  return ret;
}
```

Example (correct):

```
bool fun1(int a, int b, int c)
{
  if (!a)
  {
    return false;
  }
  DoSmth();
  if (!b)
  {
    return false;
  }
  DoSmthAgain();
  if (!c)
  {
    return false;
  }
  Do(b, c);
  return true;
}
```
