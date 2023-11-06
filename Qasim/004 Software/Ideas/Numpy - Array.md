---
tags:
  - "#Ideas"
  - "#Incomplete"
  - "#Software"
topics: "[[Numpy]]"
---
- NumPy arrays are stored in a contiguous block of memory, unlike Python lists. This means that all elements of the array are stored next to each other in memory, which allows for faster access and operations.
    - When you create a very large NumPy array, it doesn’t throw a memory error because it uses a technique called memory mapping
    - It does this by creating a ‘map’ between a disk file and the memory. This map acts as a cache for the data stored on the disk.
    - To create a memory-mapped array, you can use the numpy.memmap function.

# Syntax

# Examples

# Comments and Links
- 
# Reference