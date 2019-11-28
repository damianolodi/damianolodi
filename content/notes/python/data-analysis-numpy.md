---
title: Numpy
linktitle: Numpy
toc: false
type: docs
date: "2019-11-27T00:00:00+01:00"
draft: false
menu:
  python:
    parent: Data Analysis
    weight: 2

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

```py
import numpy as np

array[index1, index2]       # access an element. Use : to access the all line
array[start:stop:stride]    # array slicing
np.nditer(array)            # create an iterator object to be used in for loops

# Array generation
np.array(list)                  # create array from list
np.arange(first, size, spacing) # generate array
np.linspace(first, last, size)  # generate array, both end points are included
np.logspace(first, last, size, base=e) # generate log-spaced array
np.zeros((dim1,dim2))           # create array full of 0s
np.ones((dim1,dim2))            # create array full of 1s
```

_Numpy arrays are statically typed and homogeneous._ The type of the elements is determined when the array is created.

---
## Array Informations

- `np.shape(array)` &rarr; return array **dimensions** (tuple).
- `np.size(array)` &rarr; return **number of elements**.
- `np.ndim(array)` &rarr; return **number of dimensions**.
- `np.dtype(list)` &rarr; return the **data type** of the array.

---
## Probability

- `np.random.rand(d0,d1,...)` &rarr; generate a `d0, d1,…` dimension array made of **uniformly distributed random numbers** between 0 and 1.
- `np.random.randn(d0,d1,...)` &rarr; generate a `d0, d1,…` dimension array made of **gaussian distributed random numbers** between 0 and 1.
- `np.random.permutation(array)` &rarr; shuffle the array (or pandas Series).

---
## Operations on Arrays

- `np.dot(a,b)` &rarr; **dot product** of 2 arrays/matrices [doc](https://docs.scipy.org/doc/numpy/reference/generated/numpy.dot.html#numpy.dot).
- `np.sum(array)`, `np.cumsum(array)` &rarr; compute _sum_ and _cumulative sum._
- `np.prod(array)`, `np.cumprod(array)` &rarr; compute _product_ and _cumulative product._

---
## Data Analysis

- `np.mean(array)` &rarr; compute the _mean_ of the array.
- `np.std(array)` &rarr; compute _standard deviation._
- `np.var(array)` &rarr; compute _variance._
- `array.min()`, `array.max()` &rarr; find _min_ and _max_ values.

---
## File I/O from Arrays

- `data = np.genfromtxt('csv-or-tsv-file.dat')` &rarr; **import data** from file.
- `savetxt('file-name.csv', array-name, fmt='%.5f')` &rarr; **save the array** in a cvs file and specify the format (optional).
- `np.save(‘file-name.npy’,array-name)` &rarr; save array in a _proprietary file format._
- `np.load(‘file-name.npy’)` &rarr; load array from _proprietary file format._
