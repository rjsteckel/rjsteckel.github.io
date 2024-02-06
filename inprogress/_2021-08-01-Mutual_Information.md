@def title = "Mutual Information"


For two probability distributions, the KL divergence is defined as:

$$
D[p(x) || q(x)] = \sum_x{p(x)log\frac{p(x)}{q(x)}}
$$

Mutual information 

$$
I(X;Y) = D[p(x,y)||p(x)p(y)]
$$

$$
D[p(x|y)||p(x)] = D[p(y|x)||p(y)]
$$

$$
H(X) - H(X|Y)
$$