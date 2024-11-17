---
title: Skew Algorithm 
date: 2024-08-30
math: true

---

DRAFT

The suffix array is a powerful data structure for processing strings. It stores all the suffixes of a string sorted in lexicographical order. Since storing all $n$ suffixes in their entirity would take $O(n^2)$ space, we instead store just the indices of the corresponding suffix for an array that takes $O(n)$ space.

Most suffix array constructions assume the ability to sort in $O(n)$ using some form of radix sort (putting things into bins). Under this assumption, we can first describe the standard suffix array construction:

For convenience, we assume our string is of length $n = 2^k$. We make $O(k)$ iterations, with iteration $0 \leq i \leq k$ sorting all suffixes when considering only the first $2^i$ characters of the suffix. Note that this means that the first iteration is the same thing as just sorting the characters of the string on their own, finding the indices of the relevant characters.

Most divide and conquer algorithms that I'm familiar with make 2 recursive calls of equal size. The recurrence $T(n) = 2T(n / 2) + O(n)$ is very common and fairly intuitive to bound by $O(n \log n)$. It is rare however, to encounter times where the intention is to make unbalanced recursive calls. The skew algorithm does this.


