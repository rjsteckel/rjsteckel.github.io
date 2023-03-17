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
H2: The data came from a negative binomial distribution with $n=5; p=0.85$

If we don't know anything about the data, both hypotheses seem valid. The distributions both represent positive integer data. The "belief" viewpoint of bayesian probability allows to say that one hypothesis seems more likely than others. That there is a stronger belief for one hypothesis. First off, bayes rule lets "reverse" the conditional probability. But this is just due to the definition of conditional probability.

$$ P(A|B) = \frac{P(A,B)}{P(B)} $$

But, also by the conditional probability definition, we could also say

$$ P(B|A) = \frac{P(A,B)}{P(A)} $$

The joint probability $P(A,B)$ is common to both of these. With some rearranging

$$
\begin{aligned} 
    P(A,B) &= \frac{P(B|A)}{P(A)} = \frac{P(A|B)}{P(B)} \\
    P(A|B) &= \frac{P(B|A)P(A)}{P(B)} \\
\end{aligned}
$$

So, from a "belief" perspective, 

$$ P(H=h_1|D) \propto P(D|H=h_1)P(H=h_1) $$ 

However, the right-hand side of statement is not just for a random variable conditioned on a particular observation. In this case, $D$ is the observed data. Typically, it is assumed that each element in $D$ is independent of the other data elements.

$$
P(D) = P(d_1, d_2, ... d_n) = P(d_1)P(d_2)...P(d_n)
$$

This is the likelihood of the data. Our belief about $h1$ after observing this data is

$$
\begin{aligned}
P(H=h1|D) &\propto P(d_1,d_2,...,d_3|H=h_1)P(H=h_1) \\
       &\propto \left[ P(d_1|H=h_1)P(d_2|H=h_1)...P(d_n|H=h_1) \right] P(H=h_1)
\end{aligned}
$$

This is just a likelihood of the observed data multiplied by the probability of $$h_1$$. Often times, one hypothesis is more likely than others. This prior probability lets us model that if we want to. The prior could also be the same for each hypothesis and it cancels out.

So back to our example data and hypotheses about it. Our two hypotheses are:

$$
\begin{aligned}
    P(H=h_1|D) &= \frac{P(D|H=h_1)P(H=h_1)}{P(D)} \\
    P(H=h_2|D) &= \frac{P(D|H=h_2)P(H=h_2)}{P(D)} \\
\end{aligned}
$$

We no longer use the $\propto$ sign. We're dividing by $P(D)$ to make sure each hypothesis is a probability and that $P(H=h_1|D)$ and $P(H = h_2|D)$ sum to 1. This is just the sum of the weighted likelihoods (weighted by a prior) for each hypothesis. 

$$
P(D) = P(D|H=h_1)P(H=h_1) + P(D|H=h_2)P(H=h_2)
$$

When you think about it this way, bayesian probability is really just comparing weighted likelihoods in a way that makes them consistent with prior beliefs. 

So finally, as a concrete example, here's some R code to demonstrate. I assume a equal prior for each hypothesis, so they just cancel out. 

{% highlight R %}
D <- c(3, 3, 2, 6, 0, 0, 1, 0)

h1 <- function(x) { dpois(x, lambda=1.2) }
h2 <- function(x) { dnbinom(x, size=5, prob=0.85) }

prod(h1(D)) / (prod(h1(D)) + prod(h2(D)))
prod(h2(D)) / (prod(h1(D)) + prod(h2(D)))
{% endhighlight %}

The poisson(1.2) hypothesis is $h_1 = 0.613$
The negative binomial(5, 0.85) hypothesis is $h_2 = 0.387$

The data seem more likely to be from a poisson(1.2) distribution. 