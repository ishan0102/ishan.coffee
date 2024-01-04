---
title: Tensor Puzzles
date: 2024-01-01
tags:
  - seed
enableToc: true
---
My solutions to Sasha Rush's [tensor puzzles](https://github.com/srush/Tensor-Puzzles). This is useful as a quick refresher for things you can do with vanilla tensor operations.

## arange and where
```python
def arange(i: int):
	"Use this function to replace a for-loop."
	return torch.tensor(range(i))

def where(q, a, b):
	"Use this function to replace an if-statement."
	return (q * a) + (~q) * b
```

## ones
```python
def ones(i: int) -> TT["i"]:
	return where(arange(i) >= 0, 1, 0)
```

## sum
```python
def sum(a: TT["i"]) -> TT[1]:
	return ones(a.shape[0]) @ a[:, None]
```

## outer
```python
def outer(a: TT["i"], b: TT["j"]) -> TT["i", "j"]:
	return a[:, None] @ b[None, :]
```

## diag
```python
def diag(a: TT["i", "i"]) -> TT["i"]:
	return a[arange(a.shape[0]), arange(a.shape[0])]
```

## eye
```python
def eye(j: int) -> TT["j", "j"]:
	return where(arange(j)[:, None] == arange(j), 1, 0)
```

## triu
```python
def triu(j: int) -> TT["j", "j"]:
	return where(arange(j)[:, None] <= arange(j), 1, 0)
```

## cumsum
```python
def cumsum(a: TT["i"]) -> TT["i"]:
	return a @ triu(a.shape[0])
```

## diff
```python
def diff(a: TT["i"], i: int) -> TT["i"]:
	return where(arange(i) > 0, a[arange(i)] - a[arange(i) - 1], a[arange(i)])
```

## vstack
```python
def vstack(a: TT["i"], b: TT["i"]) -> TT[2, "i"]:
	return where(arange(2)[:, None] != ones(a.shape[0]), a, b)
```

## roll
```python
def roll(a: TT["i"], i: int) -> TT["i"]:
	return a[(arange(i) + 1) % i]
```

## flip
```python
def flip(a: TT["i"], i: int) -> TT["i"]:
	return a[i - arange(i) - 1]
```

## compress
```python
return NotImplementedError
```

## pad_to
```python
return NotImplementedError
```

## sequence_mask
```python
return NotImplementedError
```

## bincount
```python
return NotImplementedError
```

## scatter_add
```python
return NotImplementedError
```

## flatten
```python
return NotImplementedError
```

## linspace
```python
return NotImplementedError
```

## heaviside
```python
return NotImplementedError
```

## repeat (1d)
```python
return NotImplementedError
```
