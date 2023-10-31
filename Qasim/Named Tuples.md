---
tags:
  - "#Ideas"
  - "#Incomplete"
  - "#Software"
topics:
---
- Named tuples assign meaning to each position in a tuple and allow for more readable, self-documenting code. 
- They can be used wherever regular tuples are used, and they add the ability to access fields by name instead of position index.
- Located in the [[Collections Library for Python Datatypes]], [[Datatypes]]


# Syntax
- `collections.namedtuple(_typename_, _field_names_, _*_, _rename=False_, _defaults=None_, _module=None_)`
# Examples

```
# Basic example
>>> Point = namedtuple('Point', ['x', 'y'])
>>> p = Point(11, y=22)     # instantiate with positional or keyword arguments
>>> p[0] + p[1]             # indexable like the plain tuple (11, 22)
33
>>> x, y = p                # unpack like a regular tuple
>>> x, y
(11, 22)
>>> p.x + p.y               # fields also accessible by name
33
>>> p                       # readable __repr__ with a name=value style
Point(x=11, y=22)
```

# Comments and Links
- 
# Reference
https://docs.python.org/3/library/collections.html#namedtuple-factory-function-for-tuples-with-named-fields