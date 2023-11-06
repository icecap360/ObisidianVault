---
tags:
  - "#Ideas"
  - "#Software"
topics: "[[Datatypes]]"
---

- A ChainMap class is provided for quickly linking a number of mappings so they can be treated as a single unit. It is often much faster than creating a new dictionary and running multiple update() calls.


# Syntax
- class collections.ChainMap(*maps*) - where maps is a user updateable list of mappings. The list is ordered from first-searched to last-searched. It is the only stored state and can be modified to change which mappings are searched. The list should always contain at least one mapping.
- new_child(m=None) - Returns a new Chainmap containing a new map followed by all of the maps in the current instance. If `m` is specified, it becomes the new map at the front of the list of mappings; if not specified, an empty dict is used, so that a call to `d.new_child()` is equivalent to: `ChainMap({}, *d.maps)`
- parents - Property returning a new ChainMap containing all of the maps in the current instance except the first one.. Used to skip the first map 
# Examples
```
baseline = {'music': 'bach', 'art': 'rembrandt'}
>>> adjustments = {'art': 'van gogh', 'opera': 'carmen'}
>>> list(ChainMap(adjustments, baseline))
['music', 'art', 'opera']
```
This gives the same ordering as a series of [`dict.update()`](https://docs.python.org/3/library/stdtypes.html#dict.update "dict.update") calls starting with the last mapping:

```
>>> combined = baseline.copy()
>>> combined.update(adjustments)
>>> list(combined)
['music', 'art', 'opera']
```
# Comments and Links
- - Implemented by [[Collections Library for Python Datatypes]], 
# Reference