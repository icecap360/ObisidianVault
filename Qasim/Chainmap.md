---
tags:
  - "#Ideas"
  - "#Incomplete"
  - "#Software"
topics: "[[Datatypes]]"
---

- A ChainMap class is provided for quickly linking a number of mappings so they can be treated as a single unit. It is often much faster than creating a new dictionary and running multiple update() calls.


# Syntax
- class collections.ChainMap(*maps*) - A user updateable list of mappings. The list is ordered from first-searched to last-searched. It is the only stored state and can be modified to change which mappings are searched. The list should always contain at least one mapping.
- 

# Examples

# Comments and Links
- - Implemented by [[Collections Library for Python Datatypes]], 
# Reference