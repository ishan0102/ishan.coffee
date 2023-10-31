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
4. Two sets of vectors $A = a_1, a_2, a_3, \ldots, a_n$ and $B = b_1, b_2, b_3, \ldots, b_m$ share the same basis if both $span(A) = span(B)$. A span of a vector set is all of the points reachable through some linear combination of the vectors in the set. This implies that both sets also share the same basis vectors, i.e. every vector in $A$ and $B$ can be formed given some linearly independent set $C$ that spans the entire space. [1](https://math.wvu.edu/~hdiamond/Math251S14/basis.pdf)
5. Given $n$ vectors, each of $d$ dimensions, the dimension of their span is at most $d$ if all of the vectors are linearly independent, and at least 1 if none of the vectors are linearly independent. If every vector can be written as a linear combination of one vector, the basis vector is just that line, hence the minimum dimension of the span is 1.
6. Norms and metrics
	1. A norm is a measure of a vector's length. A norm must satisfy the properties of positive definiteness, homogeneity, and triangle inequality. [1](https://montjoile.medium.com/l0-norm-l1-norm-l2-norm-l-infinity-norm-7a7d18a4f40c)
		- $L_0$ norm: The number of nonzero elements in a vector. Technically not a norm.
		- $L_1$ norm: Known as the Taxicab norm, it is the sum of the absolute values of elements in a vector. $\sum_{i=1}^n |x_i|$.
		- $L_2$ norm: Known as the Euclidean norm, it is the Euclidean distance of a vector. $\sqrt{\sum_{i=1}^n x_i^2}$.
		- $L_\infty$ norm: The magnitude of the maximum element in a vector. The norm values from $L_2, L_3, \ldots$ trend toward this value.
	2. A metric is a function that can measure the distance between two points in space. Norms are defined for vectors spaces while metrics can be defined on any set. Essentially, norms measure the size of a single item while metrics measure distances between pairs of items. Thus, given a norm $|| \cdot ||$ a metric $d$ can be defined as $d(x, y) = ||x - y||$. Given a metric, we cannot necessarily create a norm as norms are only defined for vector spaces. [1](https://www.youtube.com/watch?v=fl5fOAYY-1s)

### Matrices
1. Matrices are linear transformations because matrix operations boil down to linear operations