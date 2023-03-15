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

Both hypotheses seem valid. The 

$$ P(H=h1|D) = \frac{P(D|H=h1)P(H=h1)}{P(D)} $$




{% highlight julia %}
test(x)
    x^2
end
{% endhighlight %}