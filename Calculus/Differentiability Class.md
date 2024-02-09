---
Wikipedia: https://en.wikipedia.org/wiki/Smoothness
aliases:
  - differentiability class
---

**Differentiability class** is a classification of functions according to the properties of their derivatives. It is a measure of the highest order of derivative that exists and is continuous for a function.

Consider an [[../Ordinary differential equations/Definitions/(Open, Closed) Set|open set]] $A$ on the real line and a function $f$ defined on $A$ with real values. Let _k_ be a non-negative integer. The function $f$ is said to be of differentiability **class** $C^k$  if the derivatives $f, f^\prime, f^{\prime\prime}, \ldots, f^k$ exist and are [[Continuous Function|continuous]] on $A$. If $f$ is *k*-differentiable on $A$, then it is at least in the class $C^{k-1}$ since $f, f^\prime, f^{\prime\prime}, \ldots, f^k$ are continuous on $A$. The function $f$  is said to be **infinitely differentiable**, **smooth**, or of **class** $C^\infty$, if it has derivatives of all orders on $A$. (So all these derivatives are continuous functions over $A$.)


Let $f$ be a function defined in a set $A$ and suppose that $f$ and all its partial derivatives of order less than or equal to $k$ are continuous in a subset $B$ of $A$. Then $f$ is said to be of class $C^{k}$ in $B$. The collection of all functions of class $C^{k}$ in $B$ is denoted by $C^{k}(B)$. Thus, a short way of indicating that $f$ is of class $C^{k}$ in $B$ is by writing $f \in C^{k}(B) . C^{0}(B)$ is the collection of all functions which are continuous in $B$ and $C^{\infty}(B)$ is the collection of all functions which have continuous derivatives of all orders in $B$.

All polynomials of a single variable as well as the functions $\sin x, \cos x$, $e^{x}$ are of class $C^{\infty}$ in $R^{1}$. The function

$$
f(x)=\left\{\begin{array}{lll}
0 & \text { if } & x \le 0 \\
x & \text { if } & x>0
\end{array}\right.
$$

is of class $C^{0}$ in $R^{1}$ but $f$ is not of class $C^{1}$ in $R^{1}$. The function

$$
g(x)= \begin{cases}1, & \text { if } \quad x \le 0 \\ 1+x^{2}, & \text { if } x>0\end{cases}
$$

is of class $C^{1}$ in $R^{1}$ but $g$ is not of class $C^{2}$ in $R^{1}$. These last two examples illustrate that we have the sequence of inclusions

$$
C^{0}(B) \supset C^{1}(B) \supset C^{2}(B) \supset \ldots \supset C^{\infty}(B)
$$

and that each class $C^{j}(B)$ is actually smaller than $C^{j-1}(B)$.