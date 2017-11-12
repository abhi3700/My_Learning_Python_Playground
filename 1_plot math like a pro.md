
# Comparison b/w Numerical and analytical derivative
```python
import numpy
from matplotlib import pyplot
%matplotlib notebook

x = numpy.linspace(0.0, 2 * numpy.pi, 100) # x variable

y  = numpy.exp(numpy.sin(x)) # y variable

# Numerical derivative

"""The function computes the forward differentiation"""
def forward_diff(y, x):
     
        deriv = numpy.empty(y.size-1)
        
        for i in range(deriv.size):
            deriv[i] = (y[i+1] - y[i])/(x[i+1] - x[i])
        return deriv
    
deriv = forward_diff(y, x)

# Analytical derivative
deriv_exact = y * numpy.cos(x)


# Plot the required derivatives
pyplot.figure() # displays the figure 

#plot of numerical derivative
pyplot.plot((x[1:] + x[:-1])/2.0, deriv, 
           linestyle='None', color='gray', marker='.', label='numerical')

#plot of analytical derivative
pyplot.plot(x, deriv_exact, 
           linestyle='-', color='k', label='analytical')

pyplot.xlabel('$x$')
pyplot.ylabel('$\mathrm{d}y/\mathrm{d}x$') 
pyplot.xlim(0.0, 2 * numpy.pi)
pyplot.legend(loc='upper center', numpoints=1)  # numpoints define the no. of dots and length of line


```




### Computing time b/w numpy_deriv and deriv
```python
#Comparison in computing time
%timeit numpy_deriv = numpy.diff(y)/numpy.diff(x)
%timeit deriv = forward_diff(y, x)

# Check whether the code result close?
numpy_deriv = numpy.diff(y)/numpy.diff(x)
print('Are they close? {}'.format(numpy.allclose(numpy_deriv, deriv)))
```

    5.45 µs ± 575 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
    59.4 µs ± 1.68 µs per loop (mean ± std. dev. of 7 runs, 10000 loops each)
    Are they close? True
    

# Comparison b/w Numerical and analytical Integral

```python
import scipy.special
import numpy

x = numpy.linspace(0.0, 2 * numpy.pi, 100)
y  = numpy.exp(numpy.sin(x)) # y variable

# Integral
integral_numerical = 2 * numpy.pi * y[:-1].mean()   # or 2 * numpy.pi * numpy.mean(y[:-1])
integral_analytical = 2 * numpy.pi * scipy.special.iv(0, 1.0)

#Are the integral results close?
error = integral_numerical - integral_analytical # calculating the difference the integrals
print('Error: {}'.format(error))
```

    Error: 0.0
    
