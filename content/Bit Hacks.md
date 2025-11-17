---
title: Bit Hacks
date: 2023-12-12
tags:
  - seed
enableToc: true
---
This assumes you have a decent understanding of bitwise operators. All hacks are from [MIT 6.172, Fall 2018](https://www.youtube.com/watch?v=ZusiKXcz_ac&list=PLUl4u3cNGP63VIBQVWguXxZZi0566y7Wf).

## **Set the kth Bit**
Create an OR mask with a 1 in the position you want to set.
```c
y = x | (1 << k);
```

## **Clear the kth Bit**
Create an AND mask with a 0 in the position you want to clear.
```c
y = x & ~(1 << k);
```

## **Toggle the kth Bit**
Create an XOR mask with a 1 in the position you want to toggle.
```c
y = x ^ (1 << k);
```

## **Extract a Bit Field**
Create a mask with the bits you want to extract and shift them such that the rightmost bit is at the 0 position.
```c
(x & mask) >> shift;
```

## **Set a Bit Field**
Invert the mask to clear the bits (in case of junk), shift the bits you want to the correct position and OR them.
```c
x = (x & ~mask) | (y << shift);
```

## **No-Temp Swap**
If we let $a$ be the initial state of $x$ and $b$ the initial state of $y$, we can expand out the following:
$$
\begin{align*}
	a &= x \oplus y \\
	b &= a \oplus y = (x \oplus y) \oplus y = x \\
	a &= a \oplus b = (x \oplus y) \oplus x = y
\end{align*}
$$
Converting this to C code gives us:
```c
x = x ^ y;
y = x ^ y;
x = x ^ y;
```

Effectively, we mix $x$ and $y$ temporarily and then unmix them, which is a key feature enabled by XOR.

## **Minimum of Two Integers**
The standard technique for finding the minimum involves a branch (`x < y ? x : y`), which can hurt branch prediction, a compiler optimization. We can do something more clever with bit hacks:
```c
r = y ^ ((x ^ y) & -(x < y));
```

This works due to how boolean values work in C, where if `x < y`, `-(x < y)` yields -1, which is just all 1s in 2s complement. Thus, `y ^ (x ^ y)` would just return `x`. If `x > y`, `-(x < y)` yields 0, which after ANDing becomes 0. Thus, `y ^ 0` would just return `y`.

## **Least-Significant 1**
We can compute the 1 bit with the least-significant position by simply ANDing with the inverse of a number and adding 1.
```c
r = x & (-x);
```

This works because the negative of a number is the same as a number's 2s complement form, which provides a mask for all but the least-significant digit.