
> [!Summary] 
> Equations involving **one** independant variable with its first or higher order derivatives
> e.g. $\frac{d^2y}{dx^2}=\frac{1}{1-x^2}$
> 
> [[Partial differential equation]] involves more than one independant variable. 


Let $f(x)$ define a function of $x$ on an interval $I: a<x<b$. By an ordinary differential equation we mean an equation involving $x$, the function $f(x)$ and one or more of its derivates.

Q: How important is the exclusion of the boundaries on the interval?

#### Order
The order of the diffrential equation is the order of the higest derivate involved in the equation. e.g.

**[[First Order DE|First order]]**

- $\frac{dy}{dx}+y=0$
- $y'=e^x$

**Second order**

- $\frac{d^2y}{dx^2}=\frac{1}{1-x^2}$
- $f'(x) = f''(x)$

**Third order**

- $(y''')^2 + (y'')^4 + y' = x$

## Explicit solution

Let $y=f(x)$ define $y$ as a function of $x$ on an interval $I: a<x<b$. We say that the function $f(x)$ is an **explicit solution** or simply a **solution** of an ordinary differential equation involving $x$, $f(x)$, and its derivatives, if it satisfies the equation for *every* $x$ in $I$, i.e., if we replace $y$ by $f(x)$, $y'$ by $f'(x)$, ..., $y^{(n)}$ by $f^{(n)}(x)$, the differential equation reduces to a function with only $x$ as the independant variable. In mathematical symbols the definitions says: the function $f(x)$ is a solution of the differential equation

> #TODO Make the link with [[Existence and uniqueness]]

$$F(x,y,y\,...,y^{(n)})=0,$$
if
$$F[x, f(x), f'(x), ..., f^{(n)}(x)]=0$$
for *every x* in $I$.

Have to be careful, an equation is not always a function. We could very well have an equation be a solution to an ODE, but it can be invalid if the solution is not a [[Function|function]].

e.g.

$y=\sqrt{-(1+x^2)}$ does not define a function. We could use it to verify that $x+yy'=0$, but it would be meaningless as $y$ is not a valid function.

$y'=\frac{-x}{\sqrt{-(1+x^2)}}$, $x+yy'= x + \frac{-x\sqrt{-(1+x^2)}}{\sqrt{-(1+x^2)}}=x-x=0$

Q: What about complex numbers? Cannot be a solution?

## Implicit solution
Similar to the explicit solution, but the solution is an [[Implicit function|implicit function]].

A relation $f(x,y)=0$ will be called an **implicit solution** of the differential equation
$$F(x,y,y\,...,y^{(n)})=0$$
on an interval $I: a<x<b$, if
1. it defines $y$ as an implicit function of $x$ on $I$, i.e., if there exists a function $g(x)$ defined on $I$ such that $f[x, g(x)]=0$ for *every x* in *I*, and if 
2. $F[x, g(x), g'(x), ..., g^{(n)}(x)]=0$ for *every x* in *I*.

An implicit solution could be defined over an interval, but be a solution only on part of it.

Typically to test whether an implicit function is a solution, we would simply, for a first order differential equation, differentiate it implicitly and if it gives the ODE then we can say that it is a solution. But there could be a bunch of caveat where part of the interval needs to be excluded or where we have an implicit equation that is not a [[Function|function]].

e.g. This implicit function is an implicit solution under certain conditions,
$$xy^2-e^{-y}-1=0$$

of the differential equation
$$(xy^2 + 2xy -1)y'+y^2=0$$

The derivative of the implicit solution is 
$$2xyy'+y^2+e^{-y}y'=(2xy+e^{-y})y'+y^2=0$$
and by substituting (from the solution) $e^{-y}=xy^2-1$ in it, $(2xy+xy^2-1)y'+y^2$ giving the differential equation.

That required a bit of fucking around to find that. It was not obvious at first. Not as straight forward as the explicit solution. Better get used to it, that's where the fun is.

But then, the implicit solution may not be valid everywhere. It can be rewritten as $y=\pm\sqrt{\frac{1+e^{-y}}{x}}$. Since $e^{-y}$ is always positive, then *y* is defined only for $x>0$. So the interval of the solution must exclude $x \le 0$.

Because we cannot isolate *y* easily, we have to resort to a graph (which would not be possible in higher dimension, but for now it will do).

![[../../Calculus/Images/Pasted image 20211024184006.png]]

```python
from sympy import Eq, symbols, plot_implicit
from sympy.functions.elementary.exponential import exp
plot_implicit(Eq(x*y**2 - exp(-y)-1))
```

We can see that this is not a valid function because for some values of *x* there are multiple values of *y*. We **need** to specify which branch to use.


> [!Example]-
> First, show that $y^2-1 = (x+2)^2$ is an implicit function. Then show that it is an implicit solution of $y^2-1-(2y+xy)y'=0$.
> 
>  $$y^2-1 = (x+2)^2$$
>  
>  $$\iff y = g(x) = \pm\sqrt{(x+2)^2-1}$$
>  
>  $$f(x, g(x)) = (x+2)^2 - g(x)^2 + 1 = (x+2)^2 - \left(\pm\sqrt{(x+2)^2-1}\right)^2 + 1 = 0$$
>  
>  If we chose only one sign, then it is an [[Implicit function|implicit function]]. 
>  
>  Then we have to show that it is an implicit solution. We first need to differentiate the implicit function and then do a few adjustements to end up on $y^2-1-(2y+xy)y'=0$.
>  
>  $$\frac{d}{dx}y^2-1 = \frac{d}{dx}(x+2)^2$$
>   $$\iff 2yy' = 2(x+2)$$
>   $$\iff yy' = x+2$$
>   $$\iff (x+2)yy' = (x+2)^2 = y^2-1$$
>   $$\iff y^2-1 - (x+2)yy' = 0$$
>  
>  Since we managed to arrive to the desired function, we can conclude that this is an implicit solution.



> [!Example]-
> Show that $e^{2y} + e^{2x} = 1$ is an implicit function, then show that it is an implicit solution of $e^{x-y} + y'e^{y-x}=0$
> 
> $$e^{2y} + e^{2x} = 1$$
>  $$\iff e^{2y} = 1 - e^{2x}$$
>  
>  This is possible if $x \ne 0$, otherwise we would have $e^{2y} = 1 -1=0$ which is not possible.
>  
>  Next, differentiate the implicit function.
>  
> $$\frac{d}{dx}e^{2y} + e^{2x} = 0$$
>  $$\iff 2y'e^{2y} + 2e^{2x} = 0$$
>  $$\iff y'e^{2y} + e^{2x} = 0$$
>  $$\iff \frac{y'e^{2y} + e^{2x}}{e^{x+y}} = 0$$
>  $$\iff y'e^{y-x} + e^{x-y} = 0$$