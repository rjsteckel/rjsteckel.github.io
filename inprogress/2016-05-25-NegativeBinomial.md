@def title = "Uses of the Negative Binomial"


The negative binomial distribution has become an interest of mine recently. Initially, I learned about the distribution in probability courses. To us, its use was motivated by counting the number of failures before r successes occurred. The classic example being the number of coin flips before n heads occurred. 

So, the probability of the k successes (given a probability of success p) is:

$$
\begin{align}
P(Y=k) = { {k - 1}\choose{r - 1} } p^{r-1} q^{k-r}
\end{align}
$$

This is a lot like the standard binomial distribution. However, it says in k-1 failures
Actually, this distribution can be parameterized in multiple ways. John Cook has a nice [article](http://www.johndcook.com/blog/2009/09/22/negative-binomial-distribution) on the most common ways.
.....


Actually, I didn't think much about the distribution since school. It seemed like an odd distribution that didn't apply to many real-world problems. I was wrong. 

In my current job, I deal with lots of counts on categorical data. In school, you learn that the go-to distribution for counts-per-something (i.e. count per day or count per block) is the Poisson distribution. This distribution makes the assumption that the mean count is the same as the variance of the counts. This rarely happens in the real world. It depends on what you're counting, but often times, you're counts included more zeros than expected. This is called over-dispersion. It makes the variance of a Poisson greater than the mean. It's over dispersed. 

To remedy this, people use the negative binomial distribution. Another interpretation of the negative binomial is as a mixture of Poisson and Gamma distributions. 

