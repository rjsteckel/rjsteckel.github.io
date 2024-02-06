@def title = "Gaussian Processes"


## Gaussian Processes

When first learning about Gaussian Processes, the term Gaussian can be a bit misleading. The first thing I thought was, "does my data need to be Gaussian"?

X - Test data
Y - Train data

Want to learn $P_{x,y}$ - the joint distribution (which is $\mid X \mid + \mid Y \mid$ dimensions)

We're interested in $P_{x|y}$ which is also gaussian

We want to predict N test points ($\mid X\mid$)

Construct a kernel, $\Sigma$

Bayesian because we're defining a prior over a space of functions.




