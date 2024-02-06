@def title = "Bayesian Kalman Filter"
@def tags = ["Kalman filter", "Bayesian"]
@def hasmath = true

A large contribution to Bayesian state space models was from Pole, West and Harrison. Both Petris and Durbin and Koopman reference this book as an authority for a Bayesian analysis of Gaussian state space models. Given the sequential/recursive nature of the Kalman filter, it fits nicely into a Bayesian approach. Thinking of the previous state distribution as a prior state distribution seems like an obvious choice. The subjective prior is less controversial here.

In machine learning, the Kalman filter is often introduced as a graphical model. It is analogous to the Hidden Markov Model (HMM). Where an HMM has discrete states, the Kalman filter has continuous. Both assume discrete time steps. A directed graphical model shows the relationship between the hidden and observed states.

We assume a Markov dependence between the nodes in the graph. Each state only depends on previous states. Here, the circles represent random variables. In the case of the Kalman filter, these are Gaussian. The shaded random variables are observable. While, the unshaded variables are latent or unobservable. An arrow between states indicates a dependence. A lack of an arrow represents a conditional independence between states. This is an important feature of the graph. It allows us to compute the joint distribution as a product of Gaussian distributions.

The standard general state space model used in Kalman filters is typically defined as,

$$
\begin{align}
y_{t} = F_t\theta_{t} + v_{t}, v_{t} \sim N(0,v_{t})  \\
\theta_{t} = G_t\theta_{t-1} + w_{t}, w_{t} \sim N(0,W_{t})
\end{align}
$$


Using the Kalman filter involves a cycle of steps to predict and correct. The pattern of predicting and then correcting is essentially what makes up the Kalman filter. As shown in Petris, without assumptions about the distribution of the state estimate, filtering follows a number of steps. First, we want to compute the one-step predictive density for the state before an observation at time step t.

$$
\begin{equation*}
f\left( \theta_t | y_{1:t-1} \right) = \int f\left(\theta_t | \theta_{t-1} \right)
f\left( \theta_{t-1} | y_{1:t-1} \right) d\theta_{t-1}
\end{equation*}
$$

Then we compute the one-step predictive density for the observation also before an observation at time step t.

$$
\begin{equation*}
f\left( y_t | y_{1:t-1} \right) = \int f\left( y_t | \theta_t \right) f\left( \theta_t | y_{1:t-1} \right) d\theta_{t}
\end{equation*}
$$

Finally, after observing the value at time t, we compute the filtering density which represents our beliefs about the value of at time t.

$$
\begin{equation*}
f\left( \theta_t | y_{1:t} \right) = \frac{ f\left( y_t | \theta_t \right) f \left( \theta_t | y_{1:t-1} \right) }
{ f \left( y_t | y_{1:t-1} \right) }
\end{equation*}
$$


This value is the starting point in the next time step.

These filtering equations can be difficult to solve. The Kalman filter makes the assumption of Gaussian distributions for the state and the observation . The nice mathematical properties of the Gaussian distribution help simplify the equations. We just need the mean and variances at each time step to fully describe the distributions. Since all distributions are Gaussian, the standard update equations for conjugate Gaussian distributions can be used to compute the new values given the previous means and covariances. Using the distribution of the state value from the previous time step as a prior which is distributed as , along with the state space equation defined earlier, we can compute any time step as follows.

First, we compute , which is Gaussian, using the state transition equation from the state space model. This is the the one-step predictive distribution of the state given the observations up to t-1.

$$
\begin{align*}
f\left(\theta_{t}|y_{1:t-1}\right) &\sim N(a_{t},R_{t}) \\ \\
a_{t} &= E[G_t\theta_t + w_t| y_{1:t-1}] = G_tm_{t-1} \\
R_{t} &= Var[G_t\theta_t + w_t | y_{1:t-1}] = G_tC_{t-1}G_t' + W_t
\end{align*}
$$

Then we predict the next observation, , which is also Gaussian. This prediction step is where we incorporate the dynamics of the model specified in the observation portion of the state space equation.

$$
\begin{align*}
f\left(Y_{t}|y_{1:t-1}\right) &\sim N(f_{t},Q_{t}) \\ \\
f_{t} &= E[F_t\theta_t + v_t| y_{1:t-1}] = F_ta_{t} \\
Q_{t} &= Var[F_t\theta_t + v_t| y_{1:t-1}] = F_tR_{t}F_t' + V_t
\end{align*}
$$

Both of the previous steps are before the observation at time t is used. When observation $y_t$ is known, we can compute using the results from the previous steps, and the new observation. The filtering distribution for the state estimate after observing .

$$
\begin{align*}
f\left( \theta_t | y_t \right) &\propto f\left(Y_t|\theta_t, y_{t-1} \right) f\left( \theta_t | y_{t-1}\right) \\
&\propto N(m_t,C_t) \\
m_t &= E[\theta_t | y_{1:t}] \\
C_t &= Var[\theta_t | y_{1:t}]
\end{align*}
$$

So the normal distribution that fully describes our estimate of at time t (for the univariate case) is

$$
\begin{align*}
f(\theta_t|y_t) &= N(m_t,C_t) \\
&= N(\frac{vm_{t-1} + C_{t-1}y_t}{v_t
+ C_{t-1}} , \frac{ v_tC_{t-1} }{v_t + C_{t-1}} )
\end{align*}
$$


If we inspect the mean of the new distribution, we see that it is a weighted average of the previous mean and the new observation (Petris) (using Bayesian updating).


$$
\begin{align*}
m_t &= \frac{v_tm_{t-1} + C_{t-1}y_t}{v_t + C_{t-1}} \\
&= \frac{ v_t }{ v_t + C_{t-1} }m_{t-1} + \frac{ C_{t-1} }{ v_t + C_{t-1} }y_t \\
&= \frac{ C_{t-1} }{ v_t + C_{t-1} }y_t + \left( 1 - \frac{ C_{t-1} }{ v_t + C_{t-1} }m_{t-1} \right) \\
&= m_{t-1} + \frac{ C_{t-1} }{ C_{t-1} + v_t } \left(y_{t} - m_{t-1} \right) \\
\end{align*}
$$

So the prediction error, is weighted by a factor
$$
\begin{equation*}
K_{t} = \frac{ C_{t} }{ v_t + C_{t} }
\end{equation*}
$$

This factor, is called the Kalman gain. There will be more weight when we are more uncertain about the prediction. The Kalman gain is what makes the Kalman filter adaptive. It determines how much we "trust" the observations. Therefore, it determines how much to correct them. The smaller is with respect to , the bigger the correction.

I highly recommend Petris if you are interested in learning more about Kalman Filtering (and you are familiar with R).