The derivative of a constant function vanishes everywhere: if $f(x) \equiv$ constant, then $f^{\prime}(x) \equiv 0$. But, does $f^{\prime}(x) \equiv 0$ imply $f(x) \equiv$ constant? Yes, if the domain of $f$ is an interval.

### Theorem
If $f$ is differentiable in an interval $I$, and if the derivative $f^{\prime}$ vanishes identically in $I$, then $f(x) \equiv$ constant in $I$, that is, $f$ has the same value at every point of $I$.

#### Proof
Let $x_{0}$ be a fixed point of $I$, and let $x$ be any other point of $I$. Then, by the mean value theorem,

$$
f(x)-f\left(x_{0}\right)=f^{\prime}(c)\left(x-x_{0}\right),
$$

where $c$ lies between $x_{0}$ and $x$. But $f^{\prime}(c)=0$, since $c$ belongs to $I$, and therefore $f(x)-f\left(x_{0}\right)=0$ or $f(x)=f\left(x_{0}\right)$. Since $x$ is an arbitrary point of $I$, it follows that $f(x)=f\left(x_{0}\right)$ for every $x \in I$.