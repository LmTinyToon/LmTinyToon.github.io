---
title: Understanding Box2D's memory manager
layout: default
---

To perform creation of objects quickly, Box2D uses own memory manager, which is optimized to create small objects. 
Unfortunately, source code contains <a href="http://www.codeproject.com/useritems/Small_Block_Allocator.asp">broken link</a>, 
which should describe mechanics of memory manager.
Therefore I decided to dive into internals of Box2D's memory managment and here is results of my observation 
(keep in mind, that I used Box2D version specified by commit 71a6079 - roughly speaking it is slightly modified v2.3.1 release).

Memory managment is encapsulated into b2BlockAllocator class. Every time you create Box2D object, which should be stored on heap, it will be created
via that allocator. It uses simple idea - each object of specified size will be placed into reserved slice of memory, 
designated to store objects with the same size (to be more precisely, sizes can be different, but differences 
should not be big values).