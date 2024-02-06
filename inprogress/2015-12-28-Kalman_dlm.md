@def title = "Kalman DLMs"


The most difficult part of using the Kalman filter is finding the best state space model. Since the Kalman filter is so efficient to compute, the maximum likelihood estimates for the variance parameters are relatively easy to find. However, checking the assumptions of the Kalman filter for your chosen state space model can be time consuming.

There are a number of R packages for Kalman filtering (sspir, dlm, StructTS). I prefer the dlm package. Mostly because the book associated with the package is very good.

In R, we can experiment with the dlm package and the built-in data set called 'co2' to run some examples of Kalman filtering.

One of the great things about the dlm package is that it simplifies the building of state space models with the dlmMod(*) functions. These function allow you to define components of a state space model, such as trend or seasonal, and then lets you add the components to get the final state space model.

For example, we can use the dlmModPoly function to build a simple state space model with two parameters. One is V, the observation variance and the other is W, the process variance. Both of these are typically estimated using the dlmMLE function which performs maximum likelihood to get the estimates. It only requires a function that provides the state space model. For example:

{% highlight R %}
model1 <- function(par) {
   dlmModPoly(1, dV = exp(par[1]), dW = exp(par[2]))
}
{% endhighlight %}

This model just sets the variance parameters with the vector passed in. With this simple model, we can estimate parameters (using dlmMLE) and filter the data (using dlmFilter). Next, we can plot the filter and check the assumptions of the Kalman filter using the residuals.

Applying this model gives us these results:

Model 1 (no trend, no seasonal)

It's difficult to see the difference between the filtered values and the data values because the filtered values are basically right on top of the data values. The "adaptive" nature of the Kalman filter adapts to the up and down seasonal trends, but the trends aren't expected in the model. We can see this in the residual density estimate. Just like with a standard regression model, we expect the density of the residuals to be Gaussian. The QQ plot and density plot shows that's not the case.

Next, we can create a state space model that expects seasonality.

{% highlight R %}
model2 <- function(par) {
   dlmModPoly(1, dV = exp(par[1]), dW = exp(par[2]))
   + dlmModSeas(12)
}
{% endhighlight %}
Model 2 (no trend, with seasonal)
Click to enlarge

With this model, we can see that the residuals look Gaussian now. In fact, this model looks much better. The dashed lines are 2 standard deviation confidence intervals. These intervals look reasonable as well. However, the estimate seems to lag the data a bit.

Our third model accounts for both seasonality and trend.

{% highlight R %}
model3 <- function(par) {
   seasons=12
   polyMod = dlmModPoly(2, dV = exp(par[1]),
                      dW = c(0, exp(par[2])))
   seasMod = dlmModSeas(seasons, dV = exp(par[1]),
                      dW = c(par[2], rep(0,seasons-2)))
   polyMod + seasMod
}
{% endhighlight %}
Model 3 (with trend, with seasonal)
Click to enlarge

This model looks the best. The residuals look Gaussian. The confidence intervals also look better than for the previous model. So far, this model looks good, but the Kalman filter also expects that the residuals are uncorrelated. We can use the tsdiag function to plot the residuals over time and look for autocorrelation.



These plots show that the residuals do not show autocorrelation. So this model seems to work well for the data.

This experimentation is time consuming. But once the state space model is chosen and the parameters are found, you have a powerful model for estimating, smoothing and forecasting.