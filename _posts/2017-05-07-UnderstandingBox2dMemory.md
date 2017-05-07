---
title: Understanding Box2D's memory manager
layout: default
---

To perform creation of objects quickly, Box2D uses own memory manager, which is optimized to create small objects. 
Unfortunately, source code contains <a href="http://www.codeproject.com/useritems/Small_Block_Allocator.asp">broken link</a>, which should 
describe mechanics of memory manager.
Therefore I decided to dive into internals of Box2D's memory managment and here is results of my observation.