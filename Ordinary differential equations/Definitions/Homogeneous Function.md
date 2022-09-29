---
aliases:
    - "homogeneous function"
---

Let $z=f(x,y)$ define $z$ as a function of $x$ and $y$ in a [[region]] $R$. The function $f(x,y)$ is said to be **homogeneous of order n** if it can be written as 
$$f(x,y) = x^ng(u)$$
where $u=\frac{y}{x}$ and $g(u)$ is a function of $u$. Alternatively
$$f(x,y) = y^nh(u)$$
where $u=\frac{x}{y}$ and $h(u)$ is a function of $u$. Alternatively
$$f(tx,ty) = t^nf(x,y)$$


> [!Example]-
> $$
> \begin{align}
> f(x, y) &= x^2 + y^2log \frac{y}{x} \\
> &= x^2\left(1 + \left(\frac{y}{x}\right)^2 \log \frac y x\right) \\
> &= x^2\left(1 + u^2 \log u\right) \\
> &= x^2g(u)
> \end{align}
> $$
> 
> Homogeneous function of order 2

> [!Example]-
> Use same function as above
> $$
> \begin{align}
> f(tx, ty) &= t^2x^2 + t^2y^2log \frac{ty}{tx} \\
> &= t^2\left(x^2 + y^2 log \frac{y}{x}\right) \\
> &= t^2f(x,y)
> \end{align}
> $$
> Homogeneous function of order 2
