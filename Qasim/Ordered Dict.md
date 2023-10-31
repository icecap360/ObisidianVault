---
tags:
  - "#Ideas"
  - "#Incomplete"
  - "#Software"
topics:
---

- Ordered dictionaries are just like regular dictionaries but have some extra capabilities relating to ordering operations. 
- Implemented by [[Collections Library for Python Datatypes]], [[Datatypes]]
- 
- - They have become less important now that the built-in *dict* class gained the ability to remember insertion order (this new behavior became guaranteed in Python 3.7). But some differences remain
	- The *OrderedDict* was designed to be good at reordering operations. Space efficiency, iteration speed, and the performance of update operations were secondary.
	- The *OrderedDict* algorithm can handle frequent reordering operations better than dict. As shown in the recipes below, this makes it suitable for implementing various kinds of LRU caches.
	- The equality operation for *OrderedDict* checks for matching order.


# Syntax
- `popitem(last=true)`, LIFO if last is true, FIFO otherwise
- `move_to_end(key, last=True)`
	- Move an existing _key_ to either end of an ordered dictionary. The item is moved to the right end if _last_ is true (the default) or to the beginning if _last_ is false.
- In addition to the usual mapping methods, ordered dictionaries also support reverse iteration using `reversed()`.
- 


# Comments and Links
- 
# Reference