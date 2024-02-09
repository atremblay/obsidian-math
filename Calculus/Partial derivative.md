---
aliases:
  - partial derivative
cssclasses:
  - cornell-right
  - cornell-border
---


> [!summary] 
> Treat every variables as constants and do a normal derivative with respect to the variable you are differentiating. 


> [!NOTE]-
> The definition of the partial derivatives of $f$ at a point $(x,y)$ of its domain $A$ involves the values of $f$ at points in a neighborhood of $(x, y)$, we must assume either that the domain $A$ is open or that $(x,y)$ is an interior point of $A$ (see [[../Ordinary differential equations/Definitions/(Open, Closed) Set|(Open, Closed) Set]]). In either case the points $(x+\Delta x,y)$ and $(x, y+\Delta y)$ with sufficiently small $|\Delta x|$ and $|\Delta y|$ belong to $A$. 


We say that the partial derivative of $f$ with respect to $x$ exists at $(x,y)$ if the limit

$$
\lim _{\Delta x \rightarrow 0} \frac{f\left(x+\Delta x, y\right)-f\left(x, y\right)}{\Delta x}
$$

exists. Then the value of this limit is the value of the derivative,

$$
\frac{\partial f}{\partial x}(x, y)=\lim _{\Delta x \rightarrow 0} \frac{f\left(x+\Delta x, y\right)-f\left(x, y\right)}{\Delta x} .
$$

The short notation can be one of these
$$
D_{1} f \text { or } \frac{\partial f}{\partial x} \text { or } f_{x}
$$

More generally,

$$
\frac{\partial f}{\partial x_i} \left(\ldots, x_{i-1}, x_i, x_{i+1},\ldots \right)=\lim_{\Delta x_i \rightarrow 0} \frac{f\left(\ldots, x_{i-1}, x_i+\Delta x_i, x_{i+1},\ldots \right)-f\left(\ldots, x_{i-1}, x_i, x_{i+1},\ldots \right)}{\Delta x_i} .
$$

This would look better in a vector notation. 

In general, $D_{j}$ will stand for the differentiation operator with respect to the $j$ th variable in $R^{n}$,

$$
D_{j}=\frac{\partial}{\partial x_{j}} ; \quad j=1, \ldots, n
$$

If the partial derivatives themselves have partial derivatives, these are called the second order derivatives of the original function. Thus a function $f$ of two variables may have four partial derivatives of the second order,

$$
\begin{array}{cc}
D_{1}^{2} f=\frac{\partial^{2} f}{\partial x^{2}}, & D_{2}^{2} f=\frac{\partial^{2} f}{\partial y^{2}} \\
D_{2} D_{1} f=\frac{\partial^{2} f}{\partial y \partial x}, & D_{1} D_{2} f=\frac{\partial^{2} f}{\partial x \partial y}
\end{array}
$$

It is not hard to show that if $D_{1} f, D_{2} f, D_{1} D_{2} f$ and $D_{2} D_{1} f$ are continuous functions in an open set $A$ then the mixed derivatives are equal in $A$,

$$
D_{2} D_{1} f=D_{1} D_{2} f
$$