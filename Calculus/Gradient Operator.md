

> [!summary]
> Gradient is the operator $\vec{\nabla}$ applied to a function $f(x,y)$
> 
> $$\vec{\nabla} f(x,y) = \begin{bmatrix} \vec{i} & \vec{j} 
> \end{bmatrix}
> \begin{bmatrix} 
> \frac{\partial f(x,y)}{\partial x} \\
> \frac{\partial f(x,y)}{\partial y} \\
> \end{bmatrix} 
> =\frac{\partial f(x,y)}{\partial x} \vec{i} + \frac{\partial f(x,y)}{\partial y} \vec{j}
> $$
> Turns a scalar field to a vector field. Typically we write $\nabla$ instead of $\vec{\nabla}$
> 


Gradient is an operator, defined as
$$\nabla = \begin{bmatrix} \frac{\partial}{\partial x} \\ \frac{\partial}{\partial y} \\ \end{bmatrix} $$
that takes a scalar field and turns it into a vector field.

It has not been applied to any function yet, it's just an operator. Then you can apply it to a function.
$$\nabla f(x,y) = \begin{bmatrix} \frac{\partial f(x,y)}{\partial x} \\ \frac{\partial f(x,y)}{\partial y} \\ \end{bmatrix} $$
This applies the operator $\nabla$ to the function $f(x,y)$.

$f(x,y)$ has a value for each set of $(x,y)$. Applying the gradient operator yields a vector that goes by $\partial f(x,y) / \partial x$ in the $\vec{i}$ and by $\partial f(x,y) / \partial y$ in the $\vec{j}$.

$$\nabla f(x,y) = \begin{bmatrix} \vec{i} & \vec{j} 
\end{bmatrix}
\begin{bmatrix} 
\frac{\partial f(x,y)}{\partial x} \\
\frac{\partial f(x,y)}{\partial y} \\
\end{bmatrix} 
=\frac{\partial f(x,y)}{\partial x} \vec{i} + \frac{\partial f(x,y)}{\partial y} \vec{j}
$$

Based on [[Directional derivative]], the gradient operator is always in the steepest direction.

## Properties
### Linearity
The gradient operator is linear
**Addition**
$$
\nabla(f_1(x,y) + f_2(x,y)) = \nabla f_1(x,y)  + \nabla f_2(x,y) 
$$
**Scalar multiplication**
$$
\nabla\left(\alpha f(x,y)\right) = \alpha \nabla f(x,y) 
$$

