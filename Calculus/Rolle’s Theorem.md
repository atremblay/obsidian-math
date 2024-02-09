
### Theorem
Let $f$ be continuous in the closed interval $[a, b]$ and differentiable, with derivative $f^{\prime}$, in the open interval $(a, b)$. Suppose that $f(a)=f(b)=k$. Then there is a point $c \in(a, b)$ such that $f^{\prime}(c)=0$.

#### Proof

> [!blank|right-medium]
> ![[Images/Chapter 3/figure4.svg]]
>

Do not be put off by the formal language. A more informal way of stating the theorem is the following: Let $f$ be continuous in a closed interval $I=[a, b]$ and differentiable at every interior point of $I$. Suppose $f$ takes the same value at both end points of $I$. Then the derivative $f^{\prime}$ vanishes at some interior point of $I$.

To prove the theorem, we first observe that $f$ has both a maximum $M$ and a minimum $m$ in $I=[a, b]$, by [[Maxima and Minima#Theorem|Theorem]], where $m \leqslant k \leqslant M$, since $f$ equals $k$ at the points $a$ and $b$. If $m=k=M$, then $f$ reduces to the constant function $f(x) \equiv k$, whose derivative vanishes at every interior point of $I$, and the theorem is proved. Otherwise, we have either $m<k$ or $M>k$. Suppose $M>k$, and let $c$ be a point of $I$ such that $f(c)=M$. Then $c \in(a, b)$, that is, $c$ is an interior point of $I$, so that the derivative $f^{\prime}(c)$ exists. If $f^{\prime}(c) \neq 0$, then $f^{\prime}(c)$ is either positive or negative. In the first case, $f$ is increasing in some neighborhood of $c$, by Sec. 3.41a, while in the second case, $f$ is decreasing in some neighborhood of $c$, by Sec. 3.41b. In either case, the neighborhood contains values of $f$ larger than $M$, so that $M$ cannot be the maximum of $f$. It follows that $f^{\prime}(c)=0$. The proof for $m<k$ is almost identical (give the details).

