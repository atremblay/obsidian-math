---
aliases:
  - open set
  - closed set
  - set
---


> [!definition]+ Open Set
> A set $A$ of points in $R^{n}$ is called open if for every $x \in A$ there is a [[(Open, Closed) Ball|ball]] with center at $x$ which is contained in $A$. 

^97b1eb

> [!definition]+ Closed Set
> A set in $R^{n}$ is called closed if its complement is an open set. 

> [!definition]+ Boundary point of a Set ($\partial A$)
> A point $x$ is called a boundary point of a set $A$ if every [[(Open, Closed) Ball|ball]] with center at $x$ contains points of $A$ and points of the complement of $A$. The set of all boundary points of $A$ is called the boundary of $A$ and is denoted by $\partial A$.

^505957


> [!definition]+ (Set) Neighborhood
> - A neighborhood of a point in $R^{n}$ is any [[(Open, Closed) Set#^97b1eb|open set]] containing the point. 
> - A point has neighborhoods which are arbitrarily small. 
> - An [[(Open, Closed) Set#^97b1eb|open set]] is a neighborhood of each of its points.

^3c19bf



> [!definition]+ Connected Set
> An [[#^97b1eb|open set]] $A$ in $R^{n}$ is called connected if any two points of $A$ can be connected by a polygonal path which is contained entirely in $A$. 

^5803c3


> [!definition]+ Domain of a Set $\Omega$
> An [[#^97b1eb|open]] and [[#^5803c3|connected set]] in $R^{n}$ is called a domain. A domain will be usually denoted by the letter $\Omega$.


> [!definition]+ Bounded Set
> A set $A$ in $R^{n}$ is called bounded if it is contained in some ball of finite radius centered at the origin.


## Example
In $\mathbb{R}^2$
$$
\left\{\left(x_{1}, x_{2}\right): x_{2}>0\right\}
$$

is open, while the set

$$
\left\{\left(x_{1}, x_{2}\right): x_{2} \ge 0\right\}
$$

is closed. The boundary of both of these sets is the $x_{1}$-axis. Also in $\mathbb{R}^2$, the rectangle defined by the inequalities

$$
a<x_{1}<b, \quad c<x_{2}<d
$$

is open, while the rectangle

$$
a \le x_{1} \le b, \quad c \le x_{2} \le d
$$

is closed. However, the inequalities

$$
a \le x_{1}<b, \quad c<x_{2}<d
$$

define a set which is neither open nor closed.