

A direction field the derivative of the solution at different points. All the arrows in the figure below. For different initialization we will get either a different direction field or a different integral curve.

The following example is a good test bed.

$y=e^{x+C_1}-x-C_2$ is the solution to $y'=x+y+C_2-1$

$$
\begin{align}
y'&=e^{x+C_1} - 1\\
&=e^{x+C_1} - x - C_2 + x  + C_2 - 1\\
&= y + x + C_2- 1
\end{align}
$$


Minimum working code. Change the value inside the arctan in the variable `deg`. Keep the `arctan`. This code ensures that the arrows are all of the same length. The norm of the arrows is encoded in the color.

```python
from pylab import *
xmax = 4.0
xmin = -xmax
D = 20
ymax = 4.0
ymin = -ymax
x = linspace(xmin, xmax, D)
y = linspace(ymin, ymax, D)
X, Y = meshgrid(x, y)

# plot the direction field (the differential equation)
C2 = 4
deg = arctan(X+Y+C2-1)
QP = quiver(X,Y,cos(deg),sin(deg), tan(deg))

# plot a few specific solutions
xx = linspace(xmin, xmax, 100)
for C1 in [0,1,2]:
    plt.plot(xx, exp(xx+C1) - xx - C2)
  
plt.ylim((ymin, ymax))
plt.xlim((xmin, xmax))
plt.colorbar()
show()
```

We see three [[Integral curve|integral curves]] corresponding to 3 different values of $C_1$ while $C_2$ is kept fixed (left and right). $C_1$ will shift the curve, but will not affect the directional field. $C_2$ on the other hand will shift the directional field.


$C_2=4$ | $C_2=2$
:---:|:---:
![[../Calculus/Images/Pasted image 20211121100624.png]] | ![[../Calculus/Images/Pasted image 20211121100534.png]]


##### Isoclines

If $y'=F(x,y)$, then each curve for which $F(x,y)=k$ where $k$ is any number, will be an isocline of the direction field.

Not sure how often this is used, but an isocline is a line that joins all points with the same derivative. In the example below, you can spot isoclines by going from top left to bottom right. All the arrows will points in the same direction. On the blue, orange and green lines, all the derivatives have the same value.

<span class='centerImg'>![[../Calculus/Images/isoclines.png|500]]</span>

The isoclines will not necessarily be straight lines.

For example, the direction field defined by $F(x,y) = x^2 + 4y^2$ will have the isoclines defined by the elipses 
$$
\begin{align}
x^2 + 4y^2 &= k\\
\iff y &= \pm \sqrt{\frac{k-x^2}{4}}
\end{align}
$$

The following image overplots the isoclines for $k=1,3,4$.
<span class='centerImg'>![[../Calculus/Images/Pasted image 20211121141845.png|500]]</span>


```python
from pylab import *
from matplotlib.patches import Ellipse
fig, ax = plt.subplots(1,1)
xmax = 2.0
xmin = -xmax
D = 20

ymax = 2.0
ymin = -ymax

x = linspace(xmin, xmax, D)
y = linspace(ymin, ymax, D)
X, Y = meshgrid(x, y)

xxx = [X.flatten()]
yyy = [Y.flatten()]
for k in [1,3,5]:
    xx = linspace(-k**0.5, k**0.5, 3*D)
    yy = ((k - xx**2)/4)**0.5
    xxx.append(xx)
    yyy.append(yy)
    yy = -yy
    xxx.append(xx)
    yyy.append(yy)

xx = np.concatenate(xxx)
yy = np.concatenate(yyy)
# plot the direction field (the differential equation)
deg = arctan(xx**2+4*yy**2)
QP = quiver(xx,yy,cos(deg),sin(deg), tan(deg))

# plot a few specific solutions
plt.ylim((ymin, ymax))
plt.xlim((xmin, xmax))
plt.colorbar()
plt.title('$F(x,y)=x^2+4y^2=k$')
show()
```