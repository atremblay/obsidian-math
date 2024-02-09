---
aliases:
  - implicit function theorem
cssclasses:
  - cornell-border
  - tufte-sidenotes
  - cornell-right
---


> [!summary] 
> Say we have a function of two independent variables $x,y$ and one dependent variable $z(x,y)$ expressed in a way where it’s impractical or impossible to isolate $z$ (I.e. definition of an [[../Ordinary differential equations/Definitions/Implicit function|implicit function]]), then the implicit function theorem states, with the help of [[Multivariate chain rule]], that the partial derivative of $z$ with respect to $x$ (or $y$) is 
> $$\frac{\partial z}{\partial x} = -\frac{F_x}{F_z}, \quad \frac{\partial z}{\partial y}=-\frac{F_{y}}{F_{z}}$$


An [[../Ordinary differential equations/Definitions/Implicit function|Implicit function]] (of three variables) has the generic form of 

$$F(x,y,z)=c \tag{1}$$
^tag1

If we treat $z$ as a function of $x,y$ and take the [[Partial derivative|partial derivative]] of both sides with respect to $x$, then the [[Multivariate chain rule|multivariate chain rule]] states (left hand side of [[^tag2|(2)]]) that

$$
\begin{align}
\frac{\partial F(x,y,z)}{\partial x}&=\frac{\partial c}{\partial x}\\
F_x(x,y,z)+F_z(x,y,z)\frac{\partial z}{\partial x} &= 0 \tag{2}\\
\frac{\partial z}{\partial x} &= -\frac{F_x}{F_z}
\end{align}
$$
^tag2

> [!cue] $\displaystyle -\frac{F_x}{F_z}$ Can’t divide by zero
> 

This is only valid if $F_{z}\left(x,y, z\right) \neq 0$. Similar reasoning can be done for the partial derivative with respect to $y$.

Of course, there is nothing special about the variable $z$. If $F_{x}$ does not vanish at $\left(x, y, z\right)$, then we can solve [[#^tag1|(1)]] for $x$ by computing the derivatives of $x$ with respect to $y$ and $z$ by implicit differentiation. In fact, if $F$ satisfies condition

$$\nabla F(x,y,z) \neq (0,0,0)$$ [^1]

[^1]: See [[Gradient Operator]]

at every point of the domain of $F$, then the implicit function theorem asserts that we can always solve equation [[^tag1|(1)]] for at least one of the variables in terms of the other two. 


## Example
As an example let us consider the equation of the unit sphere (this is a [[Surface]])

$$
x^{2}+y^{2}+z^{2}=1 \text {. } \tag{3}
$$
^tag3

> [!cue]  $F_z\neq 0$

At the point $(0,0,1)$ of this surface we have $F_{z}(0,0,1)=2$. By the implicit function theorem, we can solve [[^tag3|(3)]] for $z$ near the point $(0,0,1)$. In fact we have

> [!cue] $x^2+y^2 \lt 1$
> Note the strict inequality of $x^2+y^2\lt 1$. If it was equal to one then $z=0$ and $F_z=0$ which is invalid

$$
z=+\sqrt{1-x^{2}-y^{2}}, \quad x^{2}+y^{2}<1 . \tag{4}
$$
^tag4

In the upper half space $z>0$, equations [[#^tag3|(3)]] and [[^tag4|(4)]] describe the same surface. The point $(0,0,-1)$ is also on the surface [[^tag3|(3)]] and $F_{z}(0,0$, $-1)=-2$. Near $(0,0,-1)$ we have

$$
z=-\sqrt{1-x^{2}-y^{2}}, \quad x^{2}+y^{2}<1 . \tag{5}
$$
^tag5

In the lower half space $z<0$, equations [[^tag3|(3)]] and [[^tag5|(5)]] represent the same surface. On the other hand, at the point $(1,0,0)$ we have $F_{z}(1,0,0)$ $=0$ and it is easy to see that it is not possible to solve [[^tag3|(3)]] for $z$ in terms of $x$ and $y$ near this point. However, it is possible to solve for $x$ in terms of $y$ and $z$. Finally, near the point $(1 / \sqrt{3}, 1 / \sqrt{3}, 1 / \sqrt{3})$ which satisfies [[^tag3|(3)]] it is possible to solve for every one of the variables in terms of the other two.

Time to put on the show. Taking the derivative of [[^tag4|(4)]]

> [!cue] $z=\sqrt{1-x^{2}-y^{2}}$

$$
\begin{align}
\frac{\partial z}{\partial x} &= \frac{\partial }{\partial x} \sqrt{1-x^{2}-y^{2}} \\
&= \frac{-2x}{2\sqrt{1-x^{2}-y^{2}}} \\
&= -\frac{x}{z} \tag{6}
\end{align}
$$

Taking the derivative of [[^tag5|(5)]]

> [!cue] $z=-\sqrt{1-x^{2}-y^{2}}$

$$
\begin{align}
\frac{\partial z}{\partial x} &= -\frac{\partial }{\partial x} \sqrt{1-x^{2}-y^{2}} \\
&= \frac{2x}{2\sqrt{1-x^{2}-y^{2}}} \\
&= -\frac{x}{z} \tag{7}
\end{align}
$$

Using the implicit function theorem
$$
\begin{align}
\frac{\partial z}{\partial x} &= -\frac{F_x}{F_z}\\
&= -\frac{2x}{2z} \\
&= -\frac{x}{z} \tag{8}
\end{align}
$$

[[^tag6|(6)]] and [[^tag7|(7)]] both yield the same thing on the two halves of the unit sphere (note that it does not matter that the radius is 1). This is an easy example where we were fortunate enough to split $z$ in two regions. If it was not possible, then the implicit function theorem would be the only option, yielding [[^tag8|(8)]], which is the same answer. 


## Problems

2.1. Let $F(x, y, z)=z^{2}-x^{2}-y^{2}$

(a) Find grad $F$. What is the largest set in which grad $F$ does not vanish?

(b) Sketch the level surfaces $F(x, y, z)=c$ with $c=0,1,-1$. (Hint: Set $r^{2}=x^{2}+y^{2}$ ).

(c) Find a vector normal to the surface

$$
z^{2}-x^{2}-y^{2}=0
$$

at the point $(1,0,1)$, and the equation of the plane tangent to the surface at that point. What happens at the point $(0,0,0)$ of the surface?

2.2. Sketch the surface described by the equation

$$
\left(z-z_{0}\right)^{2}-\left(x-x_{0}\right)^{2}-\left(y-y_{0}\right)^{2}=0
$$

where $\left(x_{0}, y_{0}, z_{0}\right)$ is a fixed point. Show that if $\mathbf{n}=\left(n_{x}, n_{y}, n_{z}\right)$ is a vector normal to the surface then

$$
n_{z}^{2}-n_{x}^{2}-n_{y}^{2}=0 .
$$

Show that $n$ makes a $45^{\circ}$ angle with the $z$-axis.

2.3. Find the equation of the plane tangent to the paraboloid

$$
z=x^{2}+y^{2}
$$

at the point $(1,1,2)$.

2.4. In $R^{2}$, a level surface of a function $F$ of two variables is a curve (level curve of $F$ ) and a tangent plane is a line. Find the equation of the line tangent to the curve

$$
x^{4}+x^{2} y^{2}+y^{4}=21
$$

at the point $(1,2)$.

2.5. If possible, solve the equation

$$
z^{2}-x^{2}-y^{2}=0 .
$$

for $z$ in terms of $x, y$ near the following points:
(a) $(1,1, \sqrt{2})$;
(b) $(1,1,-\sqrt{2})$;
(c) $(0,0,0)$.

2.6. By implicit differentiation derive formulas (2.9).

2.7. Prove the identities

(a) $\operatorname{grad}(f+g)=\operatorname{grad} f+\operatorname{grad} g$

(b) $\operatorname{grad}(f g)=f \operatorname{grad} g+g \operatorname{grad} f$.