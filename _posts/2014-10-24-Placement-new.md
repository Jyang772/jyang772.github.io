---
layout: post
title: Placement new
date: 2014-10-24 22:42:00
tags: c++
---

I am still intrigued by the use of malloc() and “placement new” to create objects on the heap in C++. This allows you to construct an object at a particular location in memory. Basically memory that has already been allocated.

Malloc() only allocates the requested amount of memory and returns a pointer to it. Whereas, new combines mallac() with a call to the object’s constructor which initializes the object.  In order to use malloc() with a class, you would have to explicitly call the objects constructor. As well as explicitly calling the object’s destructor!!!

Please do not use placement new unless you really need to place an object in a specific part of memory.


<script src="https://gist.github.com/anonymous/43c530e6f721f8263cea.js"></script>
