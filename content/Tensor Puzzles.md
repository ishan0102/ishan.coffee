---
title: Tensor Puzzles
date: 2024-01-01
tags:
  - seed
enableToc: true
---
My solutions to Sasha Rush's [tensor puzzles](https://github.com/srush/Tensor-Puzzles). This is useful as a quick refresher for things you can do with vanilla tensor operations. Here's the official [solution video](https://www.youtube.com/watch?v=Hafo7hIl8MU).

## arange and where
These are provided and are really useful. The two paradigms I see for tensor operations are masking (apply `where` to input matrix) and matmuls (need different dimensions, use `arange` to generate correct size matrix and `where` to broadcast).
```python
def arange(i: int):
	"Use this function to replace a for-loop."
	return torch.tensor(range(i))

def where(q, a, b):
	"Use this function to replace an if-statement."
	return (q * a) + (~q) * b
```

## ones
Use `arange` to loop over all the indices and set all values to 1.
```python
def ones(i: int) -> TT["i"]:
	return where(arange(i) >= 0, 1, 0)
```

## sum
Do a dot product with the ones array. Effectively perform `(1 x i) @ (i x 1) => 1`.
```python
def sum(a: TT["i"]) -> TT[1]:
	return ones(a.shape[0]) @ a[:, None]
```

## outer
`(i x 1) @ (1 x j) => (i x j)`.
```python
def outer(a: TT["i"], b: TT["j"]) -> TT["i", "j"]:
	return a[:, None] @ b[None, :]
```

## diag
We can use indexing here to grab every value along the matrix diagonal.
```python
def diag(a: TT["i", "i"]) -> TT["i"]:
	return a[arange(a.shape[0]), arange(a.shape[0])]
```

## eye
Use broadcasting to check each `arange` value with itself, so that the diagonal has all ones.
```python
def eye(j: int) -> TT["j", "j"]:
	return where(arange(j)[:, None] == arange(j), 1, 0)
```

## triu
Same as `eye` except we take the entire upper triangle instead of just the diagonal.
```python
def triu(j: int) -> TT["j", "j"]:
	return where(arange(j)[:, None] <= arange(j), 1, 0)
```

## cumsum
For each value `i` in the array, we want the sum from `0` to `i`. We can multiply by the `triu` pattern to achieve exactly this.
```python
def cumsum(a: TT["i"]) -> TT["i"]:
	return a @ triu(a.shape[0])
```

## diff
Simply use indexing against the current and previous values, except for at the beginning where we don't compute a difference.
```python
def diff(a: TT["i"], i: int) -> TT["i"]:
	return where(arange(i) > 0, a[arange(i)] - a[arange(i) - 1], a[arange(i)])
```

## vstack
Create a `2 x i` array by broadcasting `[[0], [1]]` with the ones array. This works because for the first row you end up with all `True` and the second row is all `False`. Then conditionally set elements based on if we're on row 0 or 1.
```python
def vstack(a: TT["i"], b: TT["i"]) -> TT[2, "i"]:
	return where(arange(2)[:, None] != ones(a.shape[0]), a, b)
```

## roll
Increment and index the array, "rolling" around using a modulo.
```python
def roll(a: TT["i"], i: int) -> TT["i"]:
	return a[(arange(i) + 1) % i]
```

## flip
Reverse index the array. The indices `[0, 1, 2]` become `[2, 1, 0]`.
```python
def flip(a: TT["i"], i: int) -> TT["i"]:
	return a[i - arange(i) - 1]
```

## compress
In the `compress` function, the goal is to select elements from `v` based on a boolean mask `g`. This is done by creating a matrix where each column corresponds to an index in the input vector `v`. The `cumsum(1 * g)` operation computes a cumulative sum over the mask, effectively numbering the `True` elements. The expression `arange(i) == cumsum(1 * g)[:, None] - 1` creates a matrix where each row corresponds to one of the `True` indices in the mask. This matrix is then multiplied with the input vector `v`, effectively selecting and arranging the elements according to the mask.
```python
def compress(g: TT["i", bool], v: TT["i"], i:int) -> TT["i"]:
	return v @ where(g[:, None], arange(i) == cumsum(1 * g)[:, None] - 1, 0)
```

## pad_to
In `pad_to`, the goal is to extend a vector `a` to a specified length `j` by padding with zeros. This is achieved by creating a matrix where each column is a one-hot vector representing an index in the original array `a`. Multiplying `a` with this matrix effectively pads the array with zeros up to the desired length `j`.
```python
def pad_to(a: TT["i"], i: int, j: int) -> TT["j"]:
	return a @ (1 * (arange(i)[:, None] == arange(j)))
```

## sequence_mask
The `sequence_mask` operation is used to mask a 2D tensor based on specified lengths for each row. The `length[:, None] > arange(values.shape[1])` operation creates a boolean mask where each row is masked according to its corresponding value in the `length` vector. Values beyond the specified length in each row are set to zero. This mask is then applied to the `values` tensor using the `where` function, effectively zero-padding or truncating each row of the tensor based on the lengths specified in the `length` tensor.
```python
def sequence_mask(values: TT["i", "j"], length: TT["i", int]) -> TT["i", "j"]:
	return where(length[:, None] > arange(values.shape[1]), values, 0)
```

## bincount
The trick here lies with `eye(j)[a]`. That part just says that for value in `a`, get the corresponding row from the identity matrix. We have a guarantee that `j = max(a) + 1`, so `eye(j)` will contain all of the relevant rows. Now, we can just do a simple matmul with a `ones` array to vertically sum the rows, which counts the occurrences of each value.
```python
def bincount(a: TT["i"], j: int) -> TT["j"]:
	return ones(a.shape[0]) @ eye(j)[a]
```

## scatter_add
We use a similar technique from `bincount` except now we multiply by `values` since we're trying to sum to a specific position in a new array. The `link` tells us where to go and using the `eye` matrix lets us know which values to pull out for each position.
```python
def scatter_add(values: TT["i"], link: TT["i"], j: int) -> TT["j"]:
	return values @ eye(j)[link]
```

## flatten
We want to use indexing here to iterate across the entire 2D array. We can do this by grabbing the list of numbers from `0 - i * j` and using `//` to count how many `i`'s have passed and `%` to get the current `j` position. This converts `arange(i * j)` to a list of indices.
```python
def flatten(a: TT["i", "j"], i:int, j:int) -> TT["i * j"]:
	return a[arange(i * j) // j, arange(i * j) % j]
```

## linspace
Copy the `linspace` spec and use `arange(n)` instead of looping over the output array.
```python
def linspace(i: TT[1], j: TT[1], n: int) -> TT["n", float]:
	return (i + (j - i) * arange(n) / max(1, n - 1))
```

## heaviside
Once again just copy the spec, subbing in `where` for the conditional and `arange` for the loop.
```python
def heaviside(a: TT["i"], b: TT["i"]) -> TT["i"]:
	return where(a[arange(a.shape[0])] == 0, b[arange(a.shape[0])], a[arange(a.shape[0])] > 0)
```

## repeat (1d)
This effectively does an outer product, copying the initial row `i` times.
```python
def repeat(a: TT["i"], d: TT[1]) -> TT["d", "i"]:
	return ones(d)[:, None] @ a[None, :]
```

## bucketize
We do an outer product to get boolean values that tell us which buckets a value could fit into. Then we can just sum across the columns to get the index to bucket a value into. For instance, if we had the boundaries `[3, 5]` and a value `10`, it would be greater than all the boundaries so it would be at index `2`.
```python
def bucketize(v: TT["i"], boundaries: TT["j"]) -> TT["i"]:
	return (1 * (v[:, None] >= boundaries)) @ ones(boundaries.shape[0])
```