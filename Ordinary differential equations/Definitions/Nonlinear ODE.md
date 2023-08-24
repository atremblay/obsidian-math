---
aliases:
    - "nonlinear ODE"
tags:
    - "nonlinear"
---

# Around fixed points
Linearizing a Nonlinear System at a fixed point. A generic nonlinear system is expressed as
$$\mathbf{\dot{x}}=\mathbf{f}(\mathbf{x})$$
$\frac d {dt}$ of each dimensions of the state vector $\mathbf{x}$ is a function of the whole state vector.
$$
\dot{\begin{bmatrix}
x_1 \\
x_2 \\
\vdots \\
x_n
\end{bmatrix}} =
\begin{bmatrix}
f_1(x_1,x_2,\ldots,x_n) \\
f_2(x_1,x_2,\ldots,x_n) \\
\vdots \\
f_n(x_1,x_2,\ldots,x_n)
\end{bmatrix}
$$
A [[../System of ODE]] can still be expressed this way, but since each $\frac{dx_i}{dt}$ is a linear combination of all other states, we write is as $\dot{\mathbf{x}}= \mathbf{A}\mathbf{x}$.

Much of the tricks that we know for linear systems, such as [[Linear ODE#Principle of Superposition|superposition]], do not work. But we if zoom in on [[Fixed point|fixed points]], written as $\mathbf{\bar{x}}$, then we can use them. At $\mathbf{\bar{x}}$, the system does not change anymore
$$\mathbf{\dot{\bar{x}}}=\mathbf{f}(\mathbf{\bar{x}})=\mathbf{0}$$
To achieve this zoom, we need to do a [[../../Calculus/Taylor's theorem#Generalization|Taylor expansion]] around a fixed point $\mathbf{\bar{x}}$ at $\mathbf{x}=\mathbf{\bar{x}} + \mathbf{\Delta x}$. $\mathbf{x}$ is arbitrarily close (by $\mathbf{\Delta x}$) to $\mathbf{\bar{x}}$.

$$
\begin{align}
\mathbf{\dot{x}}=\mathbf{f(x)}&=\mathbf{f(\bar{x}+\Delta x)} \\
&= \underbrace{\mathbf{f(\bar{x})}}_{=0} + \mathbf{\frac{Df}{Dx}\Delta x}+ \underbrace{\frac 1 2\mathbf{\Delta x^T\frac{D^2f}{Dx^2}\Delta x}+ O(\mathbf{\Delta x^3})}_{\text{very small, so we ignore}} \\
\mathbf{\dot{x}} &\approx \mathbf{\frac{Df}{Dx}\Delta x}
\end{align}
$$
The rate of change at $\mathbf{x}$ is now approximately a linear ODE in terms of $\mathbf{\Delta x}$. 

> [!todo] Taylor expansion
> The [[../../Calculus/Taylor's theorem|taylor expansion]] does not cover the exact same notation as here. That's because that example only uses one function of x and y, but here we have n such functions. So just expand on that.


> [!Important]
> See [[../Hartman-Grobman theorem]], not every fixed point can be linearlized.


> [!important]
> While the Jacobian $\mathbf{\frac{Df}{Dx}}$ are functions of the state vector and these functions can be nonlinear, the system is linear in $\mathbf{\Delta x}$. 
> Carefully look at the [[Linear ODE|generic definition]] of a linear ODE. The coefficients are functions, but the whole system is still linear


