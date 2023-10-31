---
tags:
  - "#Ideas"
  - "#Incomplete"
  - "#Software"
topics:
---
- They have become less important now that the built-in *dict* class gained the ability to remember insertion order (this new behavior became guaranteed in Python 3.7). But some differences remain
	- The *OrderedDict* was designed to be good at reordering operations. Space efficiency, iteration speed, and the performance of update operations were secondary.
	- The *OrderedDict* algorithm can handle frequent reordering operations better than dict. As shown in the recipes below, this makes it suitable for implementing various kinds of LRU caches.
	- The equality operation for [`OrderedDict`](https://docs.python.org/3/library/collections.html#collections.OrderedDict "collections.OrderedDict") checks for matching order.




- Ordered dictionaries are just like regular dictionaries but have some extra capabilities relating to ordering operations. 
# Purpose

# Syntax/Examples

# Comments and Links
- 
# Reference