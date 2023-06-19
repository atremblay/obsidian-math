
> [!Summary]
> A combination of both [[Gradient Operator]] and [[Divergence Operator]]
> Given a function $f(x,y)$
> $$\nabla f = \begin{bmatrix} \frac{\partial f}{\partial x} \\ \frac{\partial f}{\partial y}\end{bmatrix}$$, the Laplacian is the second partial derivative
> $$
> \begin{align}
>     \begin{bmatrix} 
>         \frac{\partial^2 f}{\partial x^2} \\ \frac{\partial^2 f}{\partial y^2}
>     \end{bmatrix} \\
> \end{align}$$

Successive application of [[Gradient Operator]] and [[Divergence Operator]]
$$
\begin{align}
\vec{\nabla}\nabla f &=
    \vec{\nabla} 
    \begin{bmatrix} 
        \frac{\partial f}{\partial x} \\ \frac{\partial f}{\partial y}
    \end{bmatrix} \\
    &=
    \begin{bmatrix} 
        \frac{\partial}{\partial x} \\ \frac{\partial }{\partial y}
    \end{bmatrix} 
    \begin{bmatrix} 
        \frac{\partial f}{\partial x} \\ \frac{\partial f}{\partial y}
    \end{bmatrix} \\
    &= 
    \begin{bmatrix} 
        \frac{\partial^2 f}{\partial x^2} \\ \frac{\partial^2 f}{\partial y^2}
    \end{bmatrix} \\
\end{align}$$