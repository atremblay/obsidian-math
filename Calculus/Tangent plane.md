

> [!summary]
> The tangent plane to a surface $z=f(x,y)$ at point $P(x_0, y_0, z_0)$ is
> 
> $$
> \frac{\partial z}{\partial {x_0}}(x-x_0) + \frac{\partial z}{\partial {y_0}}(y-y_0) -(z-z_0) = 0
> $$


At a point $P(x_0, y_0, z_0)$ on a surface $z=f(x,y)$ there is a [[Equation of a plane|tangent plane]] with the equation
$$
A(x-x_0) + B(y-y_0) + C(z-z_0) = 0
$$

Fixing the value of $y=y_0$, we have the plane

$$
\begin{align}
A(x-x_0) + B(y_0-y_0) + C(z-z_0) = A(x-x_0) + C(z-z_0) = 0
\end{align}
$$
From this we get the slope of the tangent 

$$
\frac{z-z_0}{x-x_0} = -\frac A C
$$


We also know that the corresponding [[Partial derivative|partial derivative]] is the slope
$$
\frac{\partial z}{\partial {x_0}} = \frac{z-z_0}{x-x_0} = -\frac A C
$$

Similarly if we fix $x=x_0$ 
$$
\frac{\partial z}{\partial {y_0}} = \frac{z-z_0}{y-y_0} = -\frac B C
$$

A simple rewrite of the equation of a plane is
$$
\frac A C (x-x_0) + \frac B C (y-y_0) + (z-z_0) = 0
$$

Filling in the partial derivatives

$$
-\frac{\partial z}{\partial {x_0}} (x-x_0) - \frac{\partial z}{\partial {y_0}} B C (y-y_0) + (z-z_0) = 0
$$

The [[Direction Cosines|direction numbers]] of the normal to this plane are
$$
\left\langle -\frac{\partial z}{\partial {x_0}}, -\frac{\partial z}{\partial {y_0}}, 1 \right\rangle
$$
or (multiply the equation of the plane by -1)
$$
\left\langle \frac{\partial z}{\partial {x_0}}, \frac{\partial z}{\partial {y_0}}, -1 \right\rangle
$$
