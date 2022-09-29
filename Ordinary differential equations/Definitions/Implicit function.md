The relation $f(x,y)=0$ defines $y$ as an implicit function of $x$ on the [[Set#Interval|interval]] $I: a<x<b$, if there exists a function $g(x)$ defined on $I$ such that 

$$f(x,g(x))=0$$

for *every x* on *I*. $g(x)$ is more of an imaginary function. It's the function where we isolate $y$ from the rest. But it's not possible in the case of an implicit function. So $g(x)$ does not define an explicit function. 

In other words, $y$ is a function of $x$, but too complex to isolate on its own. It can give rise to an equation that is *not* a [[Function#Definition|function]] because there is more than one value corresponding to some values of $x$. It is then necessary to specify which part of the curve is in the function.

$$x^3+y^3-3xy=0$$
![[Implicit function.png]]

## Differentiation

Differentiating an implicit function is rather simple. Derive it and because $y$ is a function of $x$, apply the [[chain rule]] on $y$. 

$$\frac{d(x^3+y^3-3xy)}{dx}=3x^2+3y^2y'-3y - 3xy')=0$$
$$y'=\frac{y-x^2}{y^2-x}$$