
> [!summary]
> $$
> \boxed{
> \frac{d u}{d s} = f_x(x_0,y_0,z_0)\cos \alpha +f_y(x_0,y_0,z_0)\cos \beta+f_z(x_0,y_0,z_0) \cos\gamma
> }
> $$
> 
> where $\alpha, \beta, \gamma$ are the direction angles that the directed line $PQ$ makes with the positive x-, y- and z-axes, is the directional derivative of $u$ at $P$  and in the direction determined by $\alpha, \beta,\gamma$
> 
> The steepest direction is
> 
> $$\nabla u = \begin{bmatrix}
>     f_x \\
>     f_y \\
>     f_z
> \end{bmatrix}$$


<span class='centerImg'>![[directional_derivative.svg|400]]</span>

An interesting manipulation is
$$
\begin{align}
\frac{d u}{d s} &= f_x(x_0,y_0,z_0)\cos \alpha +f_y(x_0,y_0,z_0)\cos \beta+f_z(x_0,y_0,z_0) \cos\gamma \\
&= \sqrt{f_x^2 + f_y^2+f_z^2} \left\{\frac{ f_x}{\sqrt{f_x^2 + f_y^2+f_z^2}}\cos \alpha +\frac{f_y}{{\sqrt{f_x^2 + f_y^2+f_z^2}}}\cos \beta+\frac{f_z}{\sqrt{f_x^2 + f_y^2+f_z^2}} \cos\gamma \right\}
\end{align}
$$

The coefficients of the [[Direction Cosines|direction cosines]] $\cos\alpha, \cos\beta,\cos\gamma$ can be seen as the direction cosine of another line, the one defined by the angles
$$\frac{ f_x}{\sqrt{f_x^2 + f_y^2+f_z^2}}, \frac{ f_y}{\sqrt{f_x^2 + f_y^2+f_z^2}}, \frac{ f_z}{\sqrt{f_x^2 + f_y^2+f_z^2}}$$

This is the case because of the [[Direction Cosines#Properties|properties]]
$$
\cos^2 \alpha + \cos^2 \beta + \cos ^2 \gamma = 1
$$
and that
$$
\left( 
    \frac{ f_x}{\sqrt{f_x^2 + f_y^2+f_z^2}}
\right)^2+ 
\left(
    \frac{ f_y}{\sqrt{f_x^2 + f_y^2+f_z^2}}
\right)^2 + 
\left(
    \frac{ f_z}{\sqrt{f_x^2 + f_y^2+f_z^2}}
\right)^2 = 1
$$

Now, based on the [[Direction Cosines#Parallel|parallel]] property,$\frac{\partial u }{\partial s}$ is maximal when both lines point in the same direction.   

$$
\begin{align}
\frac{d u}{d s} &= f_x\cos \alpha +f_y\cos \beta+f_z \cos\gamma \\
&= \sqrt{f_x^2 + f_y^2+f_z^2} \left\{\frac{ f_x}{\sqrt{f_x^2 + f_y^2+f_z^2}}\cos \alpha +\frac{f_y}{{\sqrt{f_x^2 + f_y^2+f_z^2}}}\cos \beta+\frac{f_z}{\sqrt{f_x^2 + f_y^2+f_z^2}} \cos\gamma \right\} \\
&=\sqrt{f_x^2 + f_y^2+f_z^2}
\end{align}
$$

In other words, when the derivative is in the direction of the partial derivative 
$$
\begin{align}
\cos\alpha &= \frac{ f_x}{\sqrt{f_x^2 + f_y^2+f_z^2}} \propto f_x \\
\cos\beta &= \frac{ f_y}{\sqrt{f_x^2 + f_y^2+f_z^2}} \propto f_y \\
\cos\gamma &= \frac{ f_z}{\sqrt{f_x^2 + f_y^2+f_z^2}} \propto f_z
\end{align}
$$
we get the maximum value of $\sqrt{f_x^2 + f_y^2+f_z^2}$. That's why doing the a gradient descent in the direction of $\nabla u$ is the step that will bring us the fastest to a minimum.

