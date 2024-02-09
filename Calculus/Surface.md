---
aliases:
  - surface
---
A useful way of visualizing a function $f$ of one variable $x$ is by drawing its graph in the $(x, y)$-plane. We write $y=f(x)$ and draw the locus of points of the form $(x, f(x))$ where $x$ varies over the domain of $f$. Usually the graph of $f$ is a curve in the $(x, y)$-plane. However not every curve in the $(x, y)$ plane is the graph of some function. For example a circle is not the graph of any function.

In principle, we should be able to draw the graph of a function of any number of variables but we are limited by our inability to draw figures in more than three dimensions. Consequently we can actually draw the graphs of functions of up to two variables only. If $f$ is a function of $x$ and $y$, we write $z=f(x, y)$ and draw in $(x, y, z)$-space the locus of points of the form $(x, y, f(x, y))$ where $(x, y)$ varies over the domain of $f$. Although the graph of $f$ is usually a surface, not every surface in three dimensions is the graph of some function of two variables. For example, a sphere is not the graph of any function. We will now  describe a class of surfaces more general than the surfaces obtained as graphs of functions. For simplicity we restrict the discussion to the case of three dimensions.

Let $\Omega$ be a domain in $R^{3}$ and let $F(x, y, z)$ be a function in the [[Differentiability Class|class]] $C^{1}(\Omega)$. The [[Gradient Operator|gradient]] of $F$, written grad $F$, is a vector valued function defined in $\Omega$ by the formula

$$
\operatorname{grad} F=\left(\frac{\partial F}{\partial x}, \frac{\partial F}{\partial y}, \frac{\partial F}{\partial z}\right) \tag{1}
$$

The value of $\operatorname{grad} F$ at a point $(x, y, z) \in \Omega$ is a vector with components the values of the partial derivatives of $F$ at that point. It is convenient to visualize grad $F$ as a field of vectors (vector field), with one vector, grad $F(x, y, z)$, emanating from each point $(x, y, z)$ in $\Omega$.

We now make the assumption

$$
\operatorname{grad} F(x, y, z) \neq(0,0,0) \tag{2}
$$

at every point of $\Omega$. This means that the partial derivatives of $F$ do not vanish simultaneously at any point of $\Omega$. Under the assumption (2), the set of points $(x, y, z)$ in $\Omega$ which satisfy the equation

$$
F(x, y, z)=c \tag{3}
$$

for some appropriate value of the constant $c$, is a surface in $\Omega$. This surface is called a level surface of $F$. The appropriate values of $c$ are the values of the function $F$ in $\Omega$. For example, if $\left(x_{0}, y_{0}, z_{0}\right)$ is a given point in $\Omega$ and if we take $c=F\left(x_{0}, y_{0}, z_{0}\right)$, the equation

$$
F(x, y, z)=F\left(x_{0}, y_{0}, z_{0}\right)
$$

represents a surface in $\Omega$ passing through the point $\left(x_{0}, y_{0}, z_{0}\right)$. For different values of $c$, equation (3) represents different surfaces in $\Omega$. Each point of $\Omega$ lies on exactly one level surface of $F$ and any two points $\left(x_{0}, y_{0}, z_{0}\right)$ and $\left(x_{1}, y_{1}, z_{1}\right)$ of $\Omega$ lie on the same level surface if and only if

$$
F\left(x_{0}, y_{0}, z_{0}\right)=F\left(x_{1}, y_{1}, z_{1}\right) .
$$

Thus, $\Omega$ can be visualized as being laminated by the level surfaces of $F$.

Quite often we consider $c$ in equation (3) as a parameter and we say that equation (3) represents a one-parameter family of surfaces in $\Omega$. Through each point in $\Omega$ passes a particular member of this family corresponding to a particular value of the parameter $c$.

As an example let

$$
F(x, y, z)=x^{2}+y^{2}+z^{2} .
$$

Then

$$
\operatorname{grad} F(x, y, z)=(2 x, 2 y, 2 z)
$$

and if $\Omega$ is the whole of $R^{3}$ except for the origin, condition (2) is satisfied at every point of $\Omega$. The level surfaces of $F$ are spheres with center the origin. As another example, let

$$
F(x, y, z)=z
$$

Then grad $F(x, y, z)=(0,0,1)$ and condition (2) is satisfied at every point of $R^{3}$. The level surfaces are planes parallel to the $(x, y)$-plane.

Let us consider now a particular level surface $S_{c}$ given by equation (3) for a fixed value of $c$. Under our assumptions on $F$, there is a tangent plane to $S_{c}$ at each of its points. At the point of tangency the value of $\operatorname{grad} F$ is a vector normal to the tangent plane. For this reason we say that, at each point of $S_{c}$, the value of grad $F$ is a vector normal to $S_{c}$.
