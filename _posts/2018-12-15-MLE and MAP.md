---
published: false
---
## Maximum Likelihood Estimate
Suppose you have a data D generated from some model m and this model have some parameters θ.
P(D|θ,m) is called the likelihood of the model.Likelhood means probability of data D for a given value of parameter θ.Our main aim is to learn the parameter θ using the observed data D.
The most easy way is to maximize the likelihood P(D|θ,m), which means estimating the parameter θ for which the data is most likely to be observed. This is a point estimate as it will give single value of θ.

**Maximum Likelihood Estimation for Linear Regression**

Suppose we have some data D={(x,y)} in a 2 dimensional plane and we are trying to fit a linear model y=mx+b on that data.
![data.png]({{site.baseurl}}/_posts/data.png)

Now we want to fit a linear model y=wx+ε on this data,where ![first term](http://latex.codecogs.com/gif.latex?%5Cinline%20%5Cepsilon%20%5Csim%5Cmathcal%7BN%7D%5Cleft%20%280%2C%5Csigma%5E%7B2%7D%20%5Cright%20%29). we can write y = wx+ε as
![first equation](http://latex.codecogs.com/gif.latex?%5Cinline%20y%5Csim%5Cmathcal%7BN%7D%5Cleft%20%28%20w%5E%7BT%7Dx%2C%5Csigma%5E%7B2%7D%20%5Cright%20%29)
Now, we can model P(y|w,x) as a Gaussian 
![second equation](http://latex.codecogs.com/gif.latex?%5Cinline%20P%28y%7Cw%2Cx%29%20%3D%20%5Cmathcal%7BN%7D%28w%5E%7BT%7Dx%2C%5Cbeta%5E%7B-1%7D%29%2C%5Cbeta%5E%7B-1%7D%3D%5Csigma%5E%7B2%7D)

![third equation](http://latex.codecogs.com/gif.latex?%5Cinline%20P%28y%7Cw%2Cx%29%20%3D%20%5Cmathcal%7BN%7D%28w%5E%7BT%7Dx%2C%5Cbeta%5E%7B-1%7D%29%20%3D%20%5Csqrt%7B%5Cfrac%7B%5Cbeta%7D%7B2%5Cpi%7D%7D%5Cexp%7B%5Cleft%20%5B%20-%5Cfrac%7B%5Cbeta%7D%7B2%7D%28y-w%5E%7BT%7Dx%29%5E%7B2%7D%20%5Cright%20%5D%7D)
For all the data points, it can be written as:
![fourth equation](http://latex.codecogs.com/gif.latex?%5Cinline%20P%28y%7Cw%2CX%29%20%3D%20%5Cprod_%7Bn%3D1%7D%5E%7BN%7DP%28y_%7Bn%7D%7Cw%2Cx_%7Bn%7D%29%20%3D%20%5Cleft%20%28%20%5Cfrac%7B%5Cbeta%7D%7B2%5Cpi%7D%20%5Cright%20%29%5E%7B%5Cfrac%7BN%7D%7B2%7D%7Dexp%5Cleft%20%5B%20-%5Cfrac%7B%5Cbeta%7D%7B2%7D%5Cleft%20%28%20y_%7Bn%7D-w%5E%7BT%7Dx_%7Bn%7D%20%5Cright%20%29%5E%7B2%7D%20%5Cright%20%5D)
