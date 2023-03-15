---
layout: post
title:  "Confidence Intervals for Proportions"
date:   2023-03-14 18:39:34 -0500
categories: statistics
---

Proportions occur in many places.

A common inference method for a proportion, from a frequentist viewpoint, is estimating confidence intervals. For a frequentist, the parameters are fixed and the data varies. The dataset you're analyzing now is just one possible dataset of many that explain the phenomena that you're interested in. Governing this phenomeon is some parameter that you like to infer. You may never know the true value, but you can learn things about it.

For a proportion, you have a count ($x$) of the number of times the thing you're interested in ocurred out of the possible times it could have happened ($n$). This is what the binomial distribution is for. One estimate for the proportion is simply, $\frac{x}{n}$.

This is one of the most basic "estimators" of a statistic. It is estimating a true statistic, that we'll call $p$. $p$ is the true parameter that we can only get glimpses of through sampled datasets.

However, we can get an estimate of the difference between our estimator and the tru value. If we say that we want the difference of our estimator and the true value, $p$, to be less than $\alpha$, then there's randomness in that inequality. But, we can cap that randomness with a probability. For example, $0.95$.

$$
\begin{align}
P \{ \lvert \frac{x}{n} - p \rvert < α \} = 0.95
\end{align}
$$

This is the probability of:

$$
\frac{x}{n} - \alpha < p < \frac{x}{n} + \alpha
$$

As often is the case in a frequentist setting, we rely on the central limit theorem for help. If we assume, the ratio $\frac{x}{n}$ is an estimator for the probability parameter in the binomial distribution, we can use some helpful facts. The binomial distribution tends toward a normal distribution (given enough observations, and a reasonable p). So, our goal here is use this information and construct standard normals for each end of our interval.

Multiple both sides of the inequality by n

$$
\begin{align}
P \{ \lvert \frac{x}{n} - p \rvert < α \}
\end{align}
$$

$$
P \{ \lvert x - np \rvert < nα \}
$$

Divide through by the standard deviation of a binomial variable ($\sqrt{np(1-p)}$). This gets us closer to the standard normal form $\frac{x - \mu}{\sigma}$

$$
P \{ \lvert x - np \rvert < nα \} = P \{ \lvert \frac{x - np}{\sqrt{np(1-p)}} \rvert < \frac{nα}{\sqrt{np(1-p)}} \}
$$


