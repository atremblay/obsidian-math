In Sec. $3.41 \mathrm{c}$ we showed that if $f$ is differentiable at a point $x_{0}$, with derivative $f^{\prime}\left(x_{0}\right)$, and if $f^{\prime}\left(x_{0}\right)$ is positive, then $f$ is increasing in some neighborhood of $x_{0}$. We now use the mean value theorem to establish the following closely related result: If $f$ is differentiable in an interval $I$, with derivative $f^{\prime}$, and if $f^{\prime}$ is positive at every interior point of $I$, then $f$ is increasing in $I$. To see this, let $x_{1}$ and $x_{2}$ be any two points of $I$ such that $x_{1}<x_{2}$. Then, by the mean value theorem,

$$
f\left(x_{2}\right)-f\left(x_{1}\right)=f^{\prime}(c)\left(x_{2}-x_{1}\right) \tag{1}
$$

for some point $c$ between $x_{1}$ and $x_{2}$. Therefore $c$ is an interior point of $I$, so that $f^{\prime}(c)>0$. The right side of $(1)$ is positive, being the product of two positive numbers, and hence $f\left(x_{2}\right)-f\left(x_{1}\right)$ is also positive. In other words, $x_{1}<x_{2}$ implies $f\left(x_{1}\right)<f\left(x_{2}\right)$, which means that $f$ is increasing in $I$, as claimed.

Virtually the same argument shows that if $f$ is differentiable in an interval $I$, with derivative $f^{\prime}$, and if $f^{\prime}$ is negative at every interior point of $I$, then $f$ is decreasing in $I$. Give the details.

For example, the function $f(x)=(x-1)^{3}$ shown in Figure 6A is increasing in both intervals $(-\infty, 1]$ and $[1, \infty)$, since $f^{\prime}(x)=3(x-1)^{2}>0$ if $x<1$ or $x>1$. Therefore $f(x)$ is increasing in the whole interval $(-\infty, \infty)$. Similarly, the function $g(x)=1-x^{4}$ shown in Figure 6B is increasing in $(-\infty, 0]$, since $g^{\prime}(x)=-4 x^{3}>0$ if $x<0$, and decreasing in $[0, \infty)$, since $g^{\prime}(x)=-4 x^{3}<0$ if $x>0$.
