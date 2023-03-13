
> [!Summary] 
> A system of ODE is a bunch of ODEs expressed in matrix form.  The derivative of the state vector $\mathbf{x}$ is a linear combination of itself.
> 
> $$\boxed{\dot{\mathbf{x}}= \mathbf{A}\mathbf{x}}$$
> 
> $\mathbf{A}$ is the linear combination of the elements of $\mathbf{x}$. 
> - If it's a diagonal matrix, then the system is decoupled. 
> - If not, then the system is coupled.
> 
> The $n$ eigenvalues of $\mathbf{A}$ are the $n$ roots of the [[Characteristic equation]] of $(1)$, meaning $n$ different solutions. 


# Decoupled

Every ODE only depends on its own state. Think population of animals in a zoo where they have no interactions (and all the ressources necessary to grow exponentially).

$\dot{x}_i=k_ix_i$

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
k_1\dot{x}_{1} \\
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
## Solution

We just have to look at them as individual ODE (they are decoupled) and write the solution in matrix form.
$$
\begin{align}
x_1(t) &= e^{k_1 t}x_1(0) \\
x_2(t) &= e^{k_2 t}x_2(0) \\
&\vdots \\
x_n(t) &= e^{k_n t}x_n(0) \\
\end{align}
$$

$$
\begin{bmatrix}
x_{1} \\
x_{2} \\
\vdots \\
x_{n-1} \\
x_{n}
\end{bmatrix}(t) =
\underbrace{
\begin{bmatrix}
e^{k_1 t}& 0  & \ldots & 0  & 0 \\
0 & e^{k_2 t} & \ldots & 0 & 0 \\
\vdots  & \vdots & \ddots & \vdots  & \vdots \\
0 & 0  & \ldots  & e^{k_{n-1} t} & 0 \\
0 & 0 & \ldots  & 0 & e^{k_n t}
\end{bmatrix}
}_{e^{\mathbf{A}t}}
\begin{bmatrix}
x_{1} \\
x_{2} \\
\vdots \\
x_{n-1} \\
x_{n}
\end{bmatrix}(0) \\
$$
With the matrix $\mathbf{A}$ above, we can write this new matrix as a [[Matrix exponential|matrix exponential]] $e^{\mathbf{A}t}$.

# Coupled
## [[Linear ODE#Homogeneous|Homogeneous ODE]] as Coupled System

Given an $n^{th}$ order constant coefficients [[Linear ODE#Homogeneous|homogeneous DE]] 
$$a_{n}x^{(n)}+a_{n-1}x^{(n-1)}+a_{n-2}x^{(n-2)}+\ldots+a_2\ddot{x}+a_1\dot{x}+a_0x=0 \hspace{2em}(1)$$
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

The eigenvalues of $\mathbf{A}$, **in this specific case where we express an $n^{th}$ order ODE as $n$ first order ODE**, are the roots of the [[Characteristic equation]] of $(1)$.

Example here 
https://youtu.be/Vtijyyo5fKI?t=1281


> [!Important]- 
> - Although it's written as a system of ODE, the state vector is ultimately all derivatives of $x(t)$. The eigenvalues  of $\mathbf{A}$ are all solutions to the "original" $n^{th}$ order ODE. 

> [!Question]
> I don't think we can say that $\mathbf{x}(t)=e^{\mathbf{A}}\mathbf{x}(0)$ is a solution. We need to go through decoupling to use the exponential solution through another coordinate system. 

> [!info] Coupled equations
> - The system is **coupled**. Each equation can only be expressed in terms of the other variables.
> - Written as an $n^{th}$ order ODE it was homogeneous. It's still homogeneous when it's written as a system of ODEs.

> [!Example]-
> The following $3^{rd}$ order ODE
> $$\dddot{x} + a_2\ddot{x} + a_1\dot{x} + a_0x=0$$
> can be expressed in matrix form (a.k.a. system of ODE)
> $$
> \begin{align}
>     \frac{d}{dt}\begin{bmatrix}
>         x_1 \\
>         x_2 \\
>         x_3
>     \end{bmatrix} = 
>     \begin{bmatrix}
>         0 & 1 & 0 \\
>         0 & 0 & 1 \\
>         -a_0 & -a_1 & -a_2 \\
>     \end{bmatrix}
>     \begin{bmatrix}
>         x_1 \\
>         x_2 \\
>         x_3
>     \end{bmatrix}
> \end{align}
> $$
> 
> The eigenvalues of the matrix $\mathbf{A}$ are the roots of the of the [[Characteristic equation]] of the ODE
> 
> $\det(\mathbf{A} - \lambda \mathbf{I}) = 0$
> $$ 
> \begin{align}
>     \det\left(
>        \begin{bmatrix}
>         -\lambda & 1 & 0 \\
>         0 & -\lambda & 1 \\
>         -a_0 & -a_1 & -a_2-\lambda\\
>     \end{bmatrix}
>    \right) &= -\lambda(\lambda^2+a_2\lambda+a_1)-1(a_0) + 0 \\
>    &= -\left[\lambda^3 + a_2\lambda^2+a_1\lambda + a_0\right] \\
> &= 0
> \end{align}
> $$
> And the [[Characteristic equation]] is $\lambda^3 + a_2\lambda^2+a_1\lambda + a_0$
> So the values of $\lambda$ satisfying one satisfies the other.
> 
> All those $\lambda$ satisfies the solutions of type $x(t)=e^{\lambda t}$  

## [[Linear ODE#Nonhomogeneous|Nonhomogeneous ODE]] as Coupled system

$$x^{(n)}+a_{n-1}x^{(n-1)}+a_{n-2}x^{(n-2)}+\ldots+a_2\ddot{x}+a_1\dot{x}+a_0x=Q(t) \hspace{2em}(2)$$

$$x^{(n)}=Q(t)-a_{n-1}x^{(n-1)}-a_{n-2}x^{(n-2)}-\ldots-a_2\ddot{x}-a_1\dot{x}-a_0x=Q(t)-\sum_{i=0}^{n-1}a_{i}x^{(i)}$$

$$
\begin{align}
\dot{\mathbf{x}} &= 
\begin{bmatrix}
0 & 1 &  \ldots & 0 & 0 \\
0 & 0 &  \ldots & 0& 0 \\
\vdots &  \vdots & \ddots &\vdots & \vdots \\
0 & 0 &  \ldots & 0 & 1 \\
-a_1 & -a_2 &  \ldots & -a_{n-2} & -a_{n-1} \\
\end{bmatrix}
\begin{bmatrix}
x_{1} \\
x_{2} \\
\vdots \\
x_{n-1} \\
x_{n}
\end{bmatrix} +
\begin{bmatrix}
0 \\ 0 \\ 
\vdots \\
0 \\ Q(t)
\end{bmatrix}\\
&= \mathbf{A}\mathbf{x} + \mathbf{Q}
\end{align}
$$

# Decoupling

See [[Decoupling a system of ODE]]