---
title: L1 and L2 Regularization
date: 2023-11-01
tags:
  - seed
enableToc: false
---
Regularization is an important feature for neural networks to avoid overfitting. It's commonly used in linear models as a penalty term to keep parameters small. L1 and L2 regularization are specifically useful for making weights *sparse*, i.e. forcing many terms to be zero. Another form of regularization is called dropout, which is where nodes are randomly selected to have their contributions "dropped out". Overall this targets overfitting and exploding/vanishing gradients.
## L1
- Ridge regularization
- Add $\lambda \sum_{i=0}^N |W_i|$ to cost 
- Leads to sparser solutions as it makes weights go down faster than with L2
	- This is because L1 takes a step size that is linearly proportional to the gradient value
- Diamond shaped, matches absolute value function

## L2
- Lasso regularization
- Add $\lambda \sum_{i=0}^N W_i^2$ to cost
- Circle shaped, matches circle function

## Resources
- https://stats.stackexchange.com/questions/45643/why-l1-norm-for-sparse-models
- https://www.youtube.com/watch?v=QNxNCgtWSaY&t=797
