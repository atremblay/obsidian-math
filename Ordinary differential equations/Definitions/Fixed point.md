---
aliases:
    - "fixed point"
---

> [!Summary] 
> A fixed point, written as $\mathbf{\bar{x}}$, is a point where 
> $$\dot{\mathbf{\bar{x}}}=\mathbf{A}\mathbf{\bar{x}}=\mathbf{0}$$
> More generally (this covers the [[Nonlinear ODE]])
> $$\dot{\bar{\mathbf{x}}}=\mathbf{f}(\bar{\mathbf{x}})=\mathbf{0}$$
> meaning that at that point the system does not change anymore. The trivial solution ($\mathbf{x}=\mathbf{0}$) is always a fixed point in a [[Linear ODE]]

In the case of a [[Nonlinear ODE]], the fixed point is often denoted as $\bar{\mathbf{x}}$ and is a point where 
$$\dot{\bar{\mathbf{x}}}=\mathbf{f}(\bar{\mathbf{x}})=\mathbf{0}$$
In other words, when you are at that point you stay fixed. 

## Hyperbolic fixed point

If **all** eigen values $\lambda$ of $\mathbf{A}$ have nonzero real part (i.e. $\lambda=a\pm ib$ for $a\neq 0$). Real part can be positive or negative, as long as none are purely imaginary.

[Wikipedia](https://en.wikipedia.org/wiki/Hyperbolic_equilibrium_point)