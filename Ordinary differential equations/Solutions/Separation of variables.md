---
aliases:
    - "separation of variables"
tags: 
    - "first_order" 
    - "solution"
---

> [!summary]
> Separation of variables is one way to solve an [[../Definitions/Ordinary differential equation|ordinary differential equation]].
> 
> $$
> \boxed{
> \begin{align}
> \frac{dy}{dt} &= f(t)g(y) \\
> \Rightarrow \int \frac{1}{g(y)}dy &= \int f(t)dt 
> \end{align}
> }
> $$
> Find two functions $f(t)$ and $g(y)$ then you can find the solution (the initial function $y$) with the above integral.


Since the separation of variables is used in the context of differential equations, we often use $t$, for time, as the independant variables.

If we can write the derivate of $y$ with respect to $t$, which implies that $y$ is a function of $t$ as the product of two functions $f(x)$ and $g(y)$
$$
\frac{dy}{dt} = f(t)g(y)
$$

And rewrite as
$$
\begin{align}
\frac{1}{g(y)} \frac{dy}{dt} &= f(t) \\

\end{align}
$$

We can then integrate both sides with respect to $t$
$$
\int \frac{1}{g(y)} \frac{dy}{dt} dt = \int f(t)dt
$$
By using the [[../../Calculus/Integration/Substitution rule|change of variable]], we can rewrite 
$$
\int \underbrace{\frac{1}{g(y)} \frac{dy}{dt} dt}_{\textit{Change of variable}} = \int \frac{1}{g(y)} dy = \int f(t)dt
$$

We now have an integral with respecto to $y$ and another one with respect to $t$. A convenient fiction is to treat $\frac{dy}{dt}$ as a fraction and simply cancel out $dt$. This is not true, but a convenient shortcut.


> [!Example]-
> 
> $$
> \frac{dy}{dt} = y
> $$
> We have  $g(y) = y$ and $f(t) = 1$ 
> $$
> \begin{align}
> &\int \frac{1}{y} \frac{dy}{dt}dt = \int \frac{1}{y} dt = \int dt \\
> \iff & log |y| = t + C_1 \\
> \iff & y = e^{t + C_1} = e^{C_1}e^{t} = C e^{t}
> \end{align}
> $$
> 
> A constant is just a constant. We have to keep track of it, but we don't need to know it exactly unless we are looking for a specific solution.
> 
> So **a** family of solution is $y = Ce^{t}$. Not to be confused with [[../../Calculus/General solution|general solution]].
> 
