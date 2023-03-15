---
layout: post
title:  "Bayesian Beliefs"
date:   2023-03-14 18:39:34 -0500
categories: statistics probability
---

The bayesian view of probability has many nice properties. 

Suppose you have multiple hypothesis that could explain the data you have. Let's use some sample data to make this concrete.

Data = [3, 3, 2, 6, 0, 0, 1, 0]

Some possible hypotheses are:

H1: The data came from a poisson distribution with a $\lambda=1.2$
H2: The data came from a negative binomial distribution with $n=2; p=0.2$

If we don't know anything about the data, both hypotheses seem valid. The distributions both represent positive integer data. The "belief" viewpoint of bayesian probability allows to say that one hypothesis seems more likely than others. 

First off, bayes rule lets "reverse" the conditional probability. 

$$ P(H=h1|D) \propto P(D|H=h1)P(H=h1) $$ 

But this is just due to the definition of conditional probability.

$$ P(A|B) = \frac{P(A,B)}{P(B)} $$
But, also by the conditional probability definition, we could also say
$$ P(B|A) = \frac{P(A,B)}{P(A)} $$
With some rearranging
$$
\begin{align}
    P(A,B) = \frac{P(B|A)}{P(A)} = \frac{P(A|B)}{P(B)}    
\end{align}
$$


