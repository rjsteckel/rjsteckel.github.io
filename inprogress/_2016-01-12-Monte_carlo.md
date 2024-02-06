@def title = "Monte Carlo"


The concept of Monte Carlo estimation is simple and it's a great demonstration of the laws of probability. Often times, it's introduced by computing .

Using Python, we can create a function to demonstrate the concept of estimating using the Monte Carlo method. The function creates a random 2 dimensional point in the ranges 0 to 1. If we treat this as a quarter of a full circle (with radius 1), we can use the pythagorean theorem to determine if the random point is inside or outside the quarter of the circle.

If , then the random point is inside the circle. Otherwise, it's outside. As we create more and more random points, we keep track of the number that fall inside the circle. Since we're only using the first quadrant of the circle, the area is . Since we used a circle with radius of 1, this is just . So, the ratio of the number of points in the circle to the number of points generated approaches .

{% highlight ruby %}
def estimate_pi(N):
   count = 0
   for i in range(N):
      p1 = random.random(); p2 = random.random()
      if math.sqrt(p1**2 + p2**2) <= 1:
         count = count + 1
   return 4*(count/float(N))
{% endhighlight %}

Now we can call the function a few times with different values.

|estimate_pi(100)  |  estimate_pi(100000)  |  estimate_pi(100000000)|
| ------------- |:-------------:| -----:|
|3.08000|3.14755|3.14156|
|3.04000|3.14631|3.14161|
|3.12000|3.13276|3.14172|
|3.24000|3.14584|3.14138|
|2.91999|3.14596|3.14154|

Is this useful for anything? Not really. There are much better ways to calculate . But, I think it is a good demonstration of the Monte Carlo concept. The same principal is used to calculate probability densities.