@def title = "Regression"

I enjoyed the machine learning classes provided online by Stanford. I think it's great that they provide high quality courses for free. Professor Andrew Ng does a great job of explaining core machine learning concepts. In some of the first lectures, he describes a machine learning framework using a linear regression model. The regression model is one of the most simple data models that exists. For a variable that is modeled by one variable, the linear regression model is simply a line (that includes slope and y-intercept coefficients, typically named with and respectively, in the statistics literature). When two variables model the output variable, the regression model is a plane. And so on.

For the one variable case, the model is

Typically with the simple linear regression model, the parameters are found analytically (using the method of least squares). But, to illustrate the machine learning framework described in the class, we'll use a numerical optimization method typically used in machine learning.

First, I grabbed some sample data that had a linear relationship from here.

Using R, I simply type in the data. I added a column of 1's to handle the intercept parameter

{% highlight R %}
X <- c(3,5,2,3,1,4,6,4,3,2,0,10,7)
X <- cbind(rep(1,length(X)), X)
Y <- c(300,300,500,400,700,400,100,250,450,650,600,0,150)
{% endhighlight %}

Next, we define a model function that implements the linear model.

{% highlight R %}
model <- function(data, weights) {
   data %*% weights
}
{% endhighlight %}

Now, we just need to define the objective function (J).

{% highlight R %}
objective <- function(weights, data) {
   sum( (model(X, weights) - Y)^2 )
}
{% endhighlight %}

Here's where we differ from the ml class. Instead of gradient descent, we can use R's built general purpose optimization function. A number of optimization algorithms are available in optim, but with this simple example, they should all give the same answer.

{% highlight R %}
op <- optim(c(0,0), objective, method='BFGS')
{% endhighlight %}

That's it. The optimized weights are in the R object called 'op'. The y-intercept is 650.27 and the slope is -73.07. To verify the numbers, we can use R's lm function.

{% highlight R %}
fit <- lm(Y~X)
{% endhighlight %}

The lm function returns the exact same values, 650.27 and -73.07.


This same method could be used for logistic regression. We just change the model to the logit function (using appropriate data for logistic regression) and optimize again.

{% highlight R %}
model <- function(data, weights) {
	dtv = data %*% weights
	exp(dtv) / (1 + exp(dtv))
}
{% endhighlight %}

It's a toy example. A simple model like this isn't much use in the real world, but it's a start. More appropriate models can be used that might more realistically model data in higher dimensions. And possibly more efficient optimization algorithms could be used.