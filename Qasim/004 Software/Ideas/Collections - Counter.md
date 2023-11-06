---
tags:
  - "#Ideas"
  - "#Software"
topics: "[[Datatypes]]"
---

- A counter tool is provided to support convenient and rapid tallies. 
- Internally,  It is a collection where elements are stored as dictionary keys and their counts are stored as dictionary values.
- 
# Syntax
```
c = Counter()                           # a new, empty counter
>>> c = Counter('gallahad')                 # a new counter from an iterable
>>> c = Counter({'red': 4, 'blue': 2})      # a new counter from a mapping
>>> c = Counter(cats=4, dogs=8)             # a new counter from keyword args
```

- elements - interator over elements repeating each as many times as its count
```
c = Counter(a=4, b=2, c=0, d=-2)
>>> sorted(c.elements())
['a', 'a', 'a', 'a', 'b', 'b']
```
- most_common(n), returns n most common elements and their counts from the most common to the least
```
Counter('abracadabra').most_common(3)
[('a', 5), ('b', 2), ('r', 2)]
```
- total() - returns total sum of the counts
# Examples
```
# Tally occurrences of words in a list
>>> cnt = Counter()
>>> for word in ['red', 'blue', 'red', 'green', 'blue', 'blue']:
...     cnt[word] += 1
...
>>> cnt
Counter({'blue': 3, 'red': 2, 'green': 1})

>>> # Find the ten most common words in Hamlet
>>> import re
>>> words = re.findall(r'\w+', open('hamlet.txt').read().lower())
>>> Counter(words).most_common(10)
[('the', 1143), ('and', 966), ('to', 762), ('of', 669), ('i', 631),
 ('you', 554),  ('a', 546), ('my', 514), ('hamlet', 471), ('in', 451)]
```

# Comments and Links
- - Implemented by [[Collections Library for Python Datatypes]], 
# Reference