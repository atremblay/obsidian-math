
> [!Summary] 
> A system of ODE is a bunch of ODEs expressed in matrix form.  The derivative of the state vector $\mathbf{x}$ is a linear combination of itself.
> 
> $$\boxed{\dot{\mathbf{x}}= \mathbf{A}\mathbf{x}}$$
> 
> $\mathbf{A}$ is the linear combination of the elements of $\mathbf{x}$. If it's a diagonal matrix, then the system is decoupled. If not, then the system is coupled.


## Decoupled

Every ODE only depends on its own state

$\dot{x_i}=k_ix_i$

$$
\begin{align}
\mathbf{x}&=
\begin{bmatrix}
x_{1} \\
x_{2} \\
\vdots \\
x_{n-1} \\
x_{n}
\end{bmatrix} \\
\dot{\mathbf{x}}&=
\begin{bmatrix}
k_1\dot{x_{1}} \\
k_2\dot{x}_{2} \\
\vdots \\
k_{n-1}\dot{x}_{n-1} \\
k_n\dot{x}_{n}\\
\end{bmatrix} =

\begin{bmatrix}
k_1& 0  & \ldots & 0  & 0 \\
0 & k_2  & \ldots & 0 & 0 \\
\vdots  & \vdots & \ddots & \vdots  & \vdots \\
0 & 0  & \ldots  & k_{n-1} & 0 \\
0 & 0 & \ldots  & 0 & k_n 
\end{bmatrix}
\begin{bmatrix}
x_{1} \\
x_{2} \\
\vdots \\
x_{n-1} \\
x_{n}
\end{bmatrix} \\
&= \mathbf{A}\mathbf{x}
\end{align}
$$

$$\boxed{\dot{\mathbf{x}}= \mathbf{A}\mathbf{x}}$$

## Coupled

Given an $n^{th}$ order constant coefficients [[Linear ODE#Homogeneous|homogeneous DE]] 
$$a_{n}x^{(n)}+a_{n-1}x^{(n-1)}+a_{n-2}x^{(n-2)}+\ldots+a_2\ddot{x}+a_1\dot{x}+a_0x=0$$
we can turn it into $n$ **coupled** first order linear ODE. First let's rewrite the ODE
$$x^{(n)}=-a_{n-1}x^{(n-1)}-a_{n-2}x^{(n-2)}-\ldots-a_2\ddot{x}-a_1\dot{x}-a_0x=-\sum_{i=0}^{n-1}a_{i}x^{(i)}$$
We have also divided by $a_n$ to make our lives easier. The division is simply absorbed in the other coefficients. Second, introduce the state vector $\mathbf{x}$ as the new variables $x_i=x^{(i-1)}$.

$$
\begin{align}
\mathbf{x}&=
\begin{bmatrix}
x_{1} \\
x_{2} \\
x_{3} \\
\vdots \\
x_{n-2} \\
x_{n-1} \\
x_{n}
\end{bmatrix} =
\begin{bmatrix}
x  \\
\dot{x} \\
\ddot{x} \\
\vdots \\
x^{(n-3)} \\
x^{(n-2)} \\
x^{(n-1)} \\
\end{bmatrix} \\
\end{align}
$$

> [!Tip]-
> Based on this notation, these two formulation are equivalent
> $$
> \begin{align}
> x^{(n)}&=-a_{n-1}x^{(n-1)}-a_{n-2}x^{(n-2)}-\ldots-a_2\ddot{x}-a_1\dot{x}-a_0x=-\sum_{i=0}^{n-1}a_{i}x^{(i)} \\
> &=-a_{n-1}x_{n}-a_{n-2}x_{n-1}-\ldots-a_2x_3-a_1x_2-a_0x_1=-\sum_{i=0}^{n-1}a_{i}x_{i+1}
> \end{align}
> $$

$$
\begin{align}
\dot{\mathbf{x}}&=
\begin{bmatrix}
\dot{x}_{1} \\
\dot{x}_{2} \\
\dot{x}_{3} \\
\vdots \\
\dot{x}_{n-2} \\
\dot{x}_{n-1} \\
\dot{x}_{n}
\end{bmatrix}=
\begin{bmatrix}
\dot{x} \\
\ddot{x} \\
x^{(3)} \\
\vdots \\
x^{(n-2)} \\
x^{(n-1)} \\
x^{(n)}
\end{bmatrix} =
\begin{bmatrix}
x_{2}  \\
x_{3} \\
x_{4} \\
\vdots \\
x_{n-1} \\
x_{n} \\
-\sum_{i=0}^{n-1}a_{i}x_{i+1}
\end{bmatrix}
\end{align}
$$

And turn this into matrix form
$$
\begin{align}
\dot{\mathbf{x}} &= 
\begin{bmatrix}
0 & 1 & 0 &  \ldots & 0 & 0 & 0 \\
0 & 0 & 1 &  \ldots & 0 & 0& 0 \\
0 & 0 & 0 &  \ldots & 0 & 0& 0 \\
\vdots &  \vdots & \vdots & \ddots & \vdots & \vdots \\
0 & 0 & 0 &  \ldots & 0 & 1 & 0 \\
0 & 0 & 0 &  \ldots & 0 & 0 & 1 \\
-a_1 & -a_2 & -a_3 &  \ldots & -a_{n-3} & -a_{n-2} & -a_{n-1} \\
\end{bmatrix}
\begin{bmatrix}
x_{1} \\
x_{2} \\
x_{3} \\
\vdots \\
x_{n-2} \\
x_{n-1} \\
x_{n}
\end{bmatrix} \\
&= \mathbf{A}\mathbf{x}
\end{align}
$$

$$\boxed{\dot{\mathbf{x}}= \mathbf{A}\mathbf{x}}$$
We now have $n$ coupled ($\mathbb{R}^{n\times n}\times \mathbb{R}^{n \times 1}$ ) first order linear ODE. First order because we are taking the first derivative of the newly introduced variables $x_i$.  



> [!info] Coupled equations
> Important, the system is **coupled**. Each equation can only be expressed in terms of the other variables.
