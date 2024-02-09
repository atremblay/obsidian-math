
> [!Summary]
> > [!Summary] Derivative
> > There is a point between $x$ and $x + \Delta$ where the derivative is equal to the slope from $f(x)$ to $f(x + \Delta x)$
> > 
> > $$
> > f_x(x_1) = \frac{f(x+\Delta x) - f(x)}{\Delta x}
> > $$
> > 
> > $$
> > \implies f(x+\Delta x) - f(x) = f_x(x_1)\Delta x
> > $$
> > 
> > where $x < x_1 < x + \Delta x$
> > 
> > <span class='centerImg'>![[Images/mean value theorem.png|250]]</span>
>
>
>
> > [!Summary] Integral
> > For a continuous function over the interval $[a,b]$, there is a point $c$ such that the area of the rectangle with base $(b-a)$ and height $f(c)$ is equal to the areas under the function $f(x)$ between $a$ and $b$. 
> > $$\int_a^b f(x) dx = (b-a)f(c)$$
> > 



> [!blank|right-medium]
> ![[../../Essential Calculus with Applications/Images/Chapter 3/figure5.svg]]
> 


If $f(a) \neq f(b)$, then [[Rolle’s Theorem]] doesn’t hold and we can no longer assert that the curve has a horizontal tangent at some intermediate point. However, we can now assert that at some intermediate point the curve has a tangent with the same slope as the chord joining its end points $A=(a, f(a))$ and $B=(b, f(b))$, that is, a tangent with slope.

$$
\frac{f(b)-f(a)}{b-a}
$$




### Theorem
Let $f$ be continuous in the closed interval $[a, b]$ and differentiable, with derivative $f^{\prime}$, in the open interval $(a, b)$. Then there is a point $c \in(a, b)$ such that

$$
f^{\prime}(c)=\frac{f(b)-f(a)}{b-a} \tag{1}
$$

or equivalently

$$
f(b)-f(a)=f^{\prime}(c)(b-a) . \tag{2}
$$

#### Proof
We introduce a new function

$$
g(x)=f(x)-k x
$$

choosing the constant $k$ in such a way that $g(x)$ has the same value at both end points $a$ and $b$. Then $k$ must satisfy the equation

$$
g(a)=f(a)-k a=f(b)-k b=g(b)
$$

with solution

$$
k=\frac{f(b)-f(a)}{b-a}
$$

With this choice of $k, g(x)$ satisfies all the conditions of [[Rolle’s Theorem]]. But then there is a point $c \in(a, b)$ such that

$$
g^{\prime}(c)=f^{\prime}(c)-k=0
$$

that is, such that

$$
f^{\prime}(c)=k=\frac{f(b)-f(a)}{b-a}
$$

### Corollary (Mean value theorem in increment form)
If $f$ is differentiable in an interval I containing the points $x$ and $x+\Delta x$, then the increment of $f$ at $x$ can be written in the form

$$
\Delta f(x)=f(x+\Delta x)-f(x)=f^{\prime}(x+\alpha \Delta x) \Delta x, \tag{3}
$$

where $0<\alpha<1$.

#### Proof
Here it is not necessary to state explicitly that $f$ is continuous in $I$, since this follows automatically from the assumption that $f$ is differentiable in $I$ (Sec. 2.66). Choosing $a=x, b=x+\Delta x$ in (10), we obtain

$$
\Delta f(x)=f^{\prime}(c) \Delta x, \tag{4}
$$

where $c$ lies between $x$ and $x+\Delta x$, regardless of the sign of $\Delta x$. Therefore the number

$$
\alpha=\frac{c-x}{\Delta x}
$$

is always positive and lies in the interval $(0,1)$, or equivalently

$$
c=x+\alpha \Delta x \quad(0<\alpha<1) . \tag{5}
$$

Comparing (4) and (5), we get (3).

