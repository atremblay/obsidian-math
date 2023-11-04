```python
import numpy as np
import matplotlib.pyplot as plt

v, w = 500, 100
k = w/v
a = 2500

# Create a grid of x and y values
x = np.linspace(0, a, 4000)
y = np.linspace(0, 250, 4000)
X, Y = np.meshgrid(x, y)

# Define f(x,y)
def f(x, y, alpha):    
    alpha = np.radians(alpha)
    u = y/x
    
    # Evaluate the difference between the functions over the grid
    Z = (1 + u**2)**0.5 + np.sin(alpha) + u*np.cos(alpha)
    Z = Z/((1 + u**2)**0.5 - np.sin(alpha) - u*np.cos(alpha))
    Z = Z*(1-np.sin(alpha))/(1+np.sin(alpha))
    Z = Z - (x*np.cos(alpha)/(a * (np.cos(alpha) - u*np.sin(alpha))))**(2*k)
    return Z

# Plot using contour at the level 0
fig, ax = plt.subplots(figsize=(8, 6))
# The contour plot will plot the contour at f(x,y) = 0
# So if you have f(x,y) = g(x,y) then just do f(x,y) - g(x,y)=0
ax.contour(X, Y, f(X,Y,alpha=0), levels=[0], colors='olive')
ax.contour(X, Y, f(X,Y,alpha=30), levels=[0], colors='violet')
ax.contour(X, Y, f(X,Y,alpha=45), levels=[0], colors='navy')
ax.contour(X, Y, f(X,Y,alpha=60), levels=[0], colors='orange')

# Change font size of tick labels
ax.tick_params(axis='both', which='major', labelsize=14, labelcolor='blue')

# Change font size of axis labels
ax.set_xlabel('x', fontsize=16, color='blue')
ax.set_ylabel('y', fontsize=16, color='blue')

plt.grid(True)
plt.savefig("plot.svg", format="svg")
plt.show()
```
