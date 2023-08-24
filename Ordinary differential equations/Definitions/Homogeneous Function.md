---
aliases:
    - "homogeneous function"
---

Let $z=f(t,x(t))$ define $z$ as a function of $x(t)$ and $t$ in a [[Region]] $R$. The function $f(t,x(t))$ is said to be **homogeneous of order n** if it can be written as 
###### Alternative 1

^dc5cc9

$$f(t,x(t)) = t^ng(u)$$
where $u=\frac{x(t)}{t}$ and $g(u)$ is a function of $u$. 


> [!Example]
> $$
> \begin{align}
> f(t, x(t)) &= t^2 + (x(t))^2log \frac{x(t)}{t} \\
> &= t^2\left(1 + \left(\frac{x(t)}{t}\right)^2 \log \frac {x(t)} t\right) \\
> &= t^2\left(1 + u^2 \log u\right) \\
> &= t^2g(u)
> \end{align}
> $$
> 
> Homogeneous function of order 2


###### Alternative 2
$$f(t,x(t)) = x(t)^nh(u)$$
where $u=\frac{t}{x(t)}$ and $h(u)$ is a function of $u$. 

###### Alternative 3
$$f(wt,wx(t)) = w^nf(t,x(t))$$

This is sort of a variable substitution.

> [!Example]
> Use same function as above
> $$
> \begin{align}
> f(wt, wx(t)) &= w^2t^2 + w^2x(t)^2log \frac{wx(t)}{wt} \\
> &= w^2\left(t^2 + x(t)^2 log \frac{x(t)}{y}\right) \\
> &= w^2f(t,x(t))
> \end{align}
> $$
> Homogeneous function of order 2
