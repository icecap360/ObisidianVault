---
tags:
  - "#Ideas"
  - "#Incomplete"
  - "#Software"
topics: "[[Datatypes]]"
---
- Returns a new deque object initialized left-to-right (using [`append()`](https://docs.python.org/3/library/collections.html#collections.deque.append "collections.deque.append")) with data from _iterable_. If _iterable_ is not specified, the new deque is empty.
- generalization of stacks and queues
- thread-safe, memory efficient appends and pops from either side of the deque with approximately the same O(1) performance in either direction
	- Though *list* objects support similar operations, they are optimized for fast fixed-length operations and incur O(n) memory movement costs for `pop(0)` and `insert(0, v)` operations which change both the size and position of the underlying data representation.

# Syntax
- `class collections.deque([iterable[, maxlen]])`
	- If _maxlen_ is not specified or is `None`, deques may grow to an arbitrary length. Otherwise, the deque is bounded to the specified maximum length. Once a bounded length deque is full, when new items are added, a corresponding number of items are discarded from the opposite end.
# Examples

# Comments and Links
- - Implemented by [[Collections Library for Python Datatypes]], 

# Reference