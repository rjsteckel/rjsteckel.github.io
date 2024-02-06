def title = "Conditional Probability Models"

Conditional Probability Models

There some very nice lectures from a Bloomberg machine learning seminar. 

Map a vector X in $R^D$ to the parameters of a distribution

X -> Bernoulli {0..1}, $\theta$

X -> Poisson {0,1,2....}, $\lambda$

X -> Gaussian {$-\infty .. \infty$} $\mu$ (assuming $\sigma$ is known)

For linear models, we're just multiplying each element of X by some weight, $w^tX$ and summing (dot product). There's one more catch. The weight sum of the data might not satisfy the constraints of the parameters of the distribution we're interested in. 

Say we're modeling the parameter of a Poisson distribution, $\lambda$. The $\lambda$ needs to be non-negative. One way to satisfy this is to put $w^tX$ in the $\exp$ function ($e^{(w^tX)}$). And when we're intereested in the Bernoulli distribution, we could use $\frac{1}{1+{e^{-w^tX}}}$. This ensures that the weighted and summed data results in a number between 0 and 1, as a parameter for a Bernoulli distribution. For the gaussian distribution it's just the identity function. 

This is known as a Generalized Linear Model (GLM). But, I really liked the way that the Bloomberg tutorial built up the model from basic principals rather than jumping to "these are GLMs". 

In fact, from these basic principals, we can think of variations. Maybe the data-to-parameter function is some black-box function (i.e. a neural network). 



We just replace the parameters of a distribution we're interested in with $f(x^tX)$. 

#### Poisson Example

$$
P(x=k) = \frac{\lambda^ke^{-\lambda}}{k!}
$$

Taking log so that the math is easier
$$
log P(x=k) = log(\lambda^ke^-\lambda) - log(k!)
$$

$$
= klog(\lambda) + -\lambda log(e) - log(k!)
$$

Now we replace $\lambda$ with our $f(w^tX)$

$$
= klog(e^{w^tX}) + -e^{w^tX} log(e) - log(k!)
$$

Now, we can maximize w.r.t to w across all the data. 