# Morning Lecture

## Cost Functions

Lines with good fits have low cost while lines with bad fit have high cost.

We have to define the cost function first and then find the parameters that minimize that.

We take derivative of cost function with respect to β and set it to 0.

### Linear Regression: try to predict continuous value

RSS is the cost function in linear regression. It is the sum of squared differences between true value and predicted value for each x. We have control over the predicted values and we can change this by changing βs.

How do we find β? __The normal equation (example of closed form equation because there is this exact equation to compute β; not all cost functions will be closed form equations): β = (X<sup>T</sup>X)<sup>-1</sup>X<sup>T</sup>y__

Gradient descent is an iterative way to move βs towards the optimal solution.

### Logistic Regression: try to draw decision boundary via predicting probabilities between 0 and 1

The true value is either 0 or 1 but we get it by predicting probabilities and then deciding on a threshold.

The probability of predicting x correctly is f(x) if y is 1 and 1-f(x) if y is 0. This becomes: __f(x<sub>i</sub>)<sup>yi</sup>(1-f(x<sub>i</sub>))<sup>1-yi</sup>__. Recall that y can only be 0 or 1.

So what is the P(_all_ data points (x<sub>i</sub>) are predicted correctly)? Multiple together the probabilities of each individual point being predicted correctly (we make the implicit assumption here that all events are independent). Therefore we want to _maximize our likelihood_: __Πf(x<sub>i</sub>)<sup>yi</sup>(1-f(x<sub>i</sub>))<sup>1-yi</sup>__
 - This is not technically the cost function because we are trying to maximize this - not minimize it. The cost function is really the negation of this.

This equation is ugly to take the derivative of and your computer will not like something that asymptotically approaches 0. To fix this we take the natural logarithm of the likelihood to make it easier via power rule and product rule.

__Log Likelihood: Σ y<sub>i</sub>ln(f(x<sub>i</sub>)) + (1-y<sub>i</sub>)ln(1-f(x<sub>i</sub>))__

We could prove that there is only one minimum to this function by taking the 2nd derivative and showing its convex.

__Cost function = -Log Likelihood__

## Gradient Descent

You could technically gradient ascent but most software implementations expect minimize.

Gradient descent will still work even if there is not only one minimum but the solution just won't be the global minimum.

Imagine the cost function as a 3D bowl shaped contour. Let's imagine we are camping in this bowl shaped valley and we camped at very bottom and we are lost. How will we get home? Feel all around us and take one step in the direction that is the most 'down'. This will only work, of course, if there is only one bottom and no other little valleys.

The whole idea is that we only need to know our immediate surroundings and then we just go in the direction of the deepest slope down. Mathematically this is the __GRADIENT__. Recall from calculus: Gradient is generalization of derivative to functions of several variables. It is a vector (i.e. has _direction_ and _magnitude_) whose components are the _n_ partial derivatives of _f_. The gradient points in the direction of the greatest rate of increase and its magnitude is the slope of the graph in that direction.

1. Initialize β (choose all values to be 0)

2. Repeat until done

3. β = β - α\*∇f <- where f is cost function

# Morning Breakout

1. What is closed form? Expression can be evaluated in finite number of operations. __Any time you have polynomial of variable on one side and log or exponent on the other there is no closed form because isolation is not possible. This is quite common.__

2. Linear regression cost function be solved in closed form? If so what is it? The cost function is RSS and the solution is the normal equation. To solve the normal it takes approximately n<sup>3</sup> operations. This is quite slow, which is why it's typically not solved via this method even those closed form solution does exist.

3. Logistic regression? Cost function is -log likelihood and does not have closed form solution.

4. How do we optimize a cost function if we don't have closed form? Something like gradient descent.

5. Are there times we use gradient descent even when we have closed form? Yes

6. Why might we sometimes prefer gradient descent over the closed form? If the closed form solution is slow or hard to do computationally.

7. Why might we sometimes prefer the closed form over gradient descent? If requirements for gradient descent are not met because gradient descent will only find solution under certain conditions, i.e. convexity not met.

## Numpy

A huge series of wrappers that calls down into C and Fortran. All numpy arrays are stored in continuous block of memory. This is why numpy.sum is faster than Python's sum(). But conversely, if you iterate over some iterator to make a bunch of numpy arrays and also appending to numpy arrays is very slow. Python lists do not store attribute about length but len is constant time so there must be something stored somewhere. Numpy .shape is probably just as fast.  

```python
import numpy as np
a = np.array([1,2,3]) #shape = (3,)
# will always be (x,) never (,x)
b = np.array([[7,8,9]]) #shape = (1,3)
c = np.array([1,4,7])
c = c[:,np.newaxis] #shape = (3,1)
m =  np.array([[1,2],[3,4]]) #shape = (2,2)

a*b #element wise is the default multiplication for numpy 1*7, 2*8, 3*9
# matrix objects in numpy have matrix multiplication as default
a*c #broadcasting is happening here, only useful for multiplying matrix by weight
a.T #did nothing
a.dot(b) #failed
a.dot(c) #gave 30 and still worked
c.dot(b) # standard matrix multiplication
m[:,1] #gives you second column but now has shape (2,)
# these options all give you shape (2,1)
m[:,np.newaxis,1]
m[:,0:1]
m[:,1:]
```

# Afternoon Lecture

When do we know we're done? When the slope of the cost function is 0. However, if we're doing this iteratively, we're not actually going to get to 0 - just another number that is really small. Really, we should just stop when the incremental change in the cost function is sufficiently small.

## Gradient Descent (Batch)

1. Initialize β (choose all values to be 0)

2. Repeat until |previous cost - current cost| < ε <-- (could be ~1000 times). Cost function calculated (i.e. summed) here.

 * β = β - α\*∇f <- where f is cost function, calculate gradient for all data points
 * _Updated_

__Fewer updates. But each update takes a long time.__

## Stochastic Gradient Descent

1. Initialize β

2. Shuffle order of data points. We will choose a single one at a time in the below loop and it will be random. Repeat until (change in cost function < ε) <-- (~5-10 times). Cost function calculated here.

 * For i = 1 to n (# of data points, e.g. 10,000 data points) <-- data points shuffled above

    * β = β - α\*∇f(x<sub>i</sub>) <-- gradient of a single data point
    * _Updated_

Looks less like going straight towards the minimum because each data point might pull it a little differently.

The inner for loop here (i.e. each update) is much faster here than for batch gradient descent. You will have to do many more updates but the time of each update is much faster so it takes less time overall. You have to do the outer for loop much fewer times for stochastic than for batch.

__Every single time I talk to someone I incorporate their feedback vs. gradient descent I try to talk to everybody and then incorporate all the feedback.__

# Afternoon Breakout

1. Graph of cost function that gradient descent would fail to solve for? Vertical line. Discontinuous.

2. How do we know it will work for logistic regression? Because the cost function is globally convex. Convexity basically means it looks like a big bowl in an arbitrary number of dimensions.

3. What will happen if α (learning rate) is too large? Diverges. If α is very large, every time you step you will not only cross over the optimal but also get further and further away from it.

4. What will happen if α is too small? Will just take forever. However, too small is probably better than too large.

5. How do you determine the optimal level of α? One that gives you the least number of steps to converge. How do we find a good value of α? Don't waste time trying to find best α because the point of a good α is to save you time - which you've just squandered by finding α.

6. What is the difference between stochastic gradient descent (SGD) and batch gradient descent (GD)? In both, we are updating our βs - which correspond to features. _We treat features the same in both. The only difference between the two is in how we treat rows!_
 * Suppose we have 1000 rows

 * Batch GD: 1000 rows in a single update i.e. single iteration

 * Minibatch GD: x rows

 * Stochastic GD: 1 row in a single update

 * An epoch is a single pass through the whole dataset. In GD an epoch = an iteration. Not so in SGD


7. Why is SGD sometimes preferred over GD? If data is too huge. You hope that you don't have do as many epochs as you would have to in GD to reach same level of goodness. We assert that SGD is faster. However, there is no technical reason that SGD should converge. It just tends to.

8. Why is GD sometimes preferred over SGD? It can be harder to send hyperparameters with SGD. Alpha can be hard to find. If dataset is small enough that GD can be run, then why not. The only reason to switch to SGD is if you have reason to suspect full GD will not work.

Lee compares GD to a big lumbering monster with huge steps trying to get to a point that is a non-integer number of huge monster steps away from it. It is going to overstep and have to backtrack. SGD is like a person taking many little steps, which will require many more to reach the point but will not have to backtrack and such.

Also the whole is sometimes just greater than the sum of its parts. Doing the whole thing can be slower than doing each one at a time. 
