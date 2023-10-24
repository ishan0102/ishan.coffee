---
title: MLIB Practice Problems
date: 2023-10-22
tags:
  - sapling
enableToc: true
---
I'll be solving all of the problems from Chip Huyen's [ML Interviews Book](https://huyenchip.com/ml-interviews-book/) (MLIB). Many ML companies ask questions similar to the ones found in this book and I've found it to be an invaluable resource in my preparation.

# Math

## Algebra
### Vectors
1. Dot product
	1. The geometric interpretation of the dot product is that for $u \cdot v$, it projects $u$ onto $v$ and multiplies the magnitude of the projected vector by $|v|$. This result tells you how much one vector goes in the direction of the other. The projection can be done the other way around and will yield the same result. If the vector you project onto doesn't have unit length, the result of the dot product is scaled by the length of the vector being projected onto. [1](https://math.stackexchange.com/questions/805954/what-does-the-dot-product-of-two-vectors-represent), [2](https://gregorygundersen.com/blog/2018/06/26/dot-product/)
	2. We want to maximize $u \cdot v$ with the constraint that $v$ is of unit length. Multiplying a vector by itself yields the maximum product, which is what makes the dot product useful as a similarity measure between vectors. A positive dot product implies two vectors that point in the same direction while a negative dot product implies the opposite. Thus, $v$ should be $u$ scaled down to be of unit length, so $v = \frac{u}{|u|}$.
2. Outer product
	1. Given $a = [3, 2, 1]$ and $b = [-1, 0, 1]$, $a^Tb = \begin{bmatrix} -3 & 0 & 3\\ -2 & 0 & 2\\ -1 & 0 & 1 \end{bmatrix}$.
	2. The outer product is useful in cases where we care about pairwise interactions between features. For example, it is used when computing the covariance matrix in PCA. The outer product is also much faster than using two for loops. [1](https://towardsdatascience.com/outer-products-a-love-letter-b29a2c2c818e)
3. Two vectors are linearly independent if one vector cannot be constructed from the other vector using nothing but linear operations (vector addition and scalar multiplication). If a vector in a set of vectors can be represented as a linear combination of the other vectors, it is said to be a linearly dependent vector.
4. Two sets of vectors $A = a_1, a_2, a_3, \ldots, a_n$ and $B = b_1, b_2, b_3, \ldots, b_m$ share the same basis if both $span(A) = span(B)$. A span of a vector set is all of the points reachable through some linear combination of the vectors in the set. This implies that both sets also share the same basis vectors, i.e. every vector in $A$ and $B$ can be formed given some linearly independent set $C$ that spans the entire space.
5. Given $n$ vectors, each of $d$ dimensions, the dimension of their span is 