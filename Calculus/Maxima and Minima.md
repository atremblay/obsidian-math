a. Let $f$ be a function defined in an interval $I$, and suppose there is a point $p \in I$ such that $f(x) \leqslant f(p)$ for all $x \in I$. Then $f(p)$ is called the maximum of $f$ in $I$, and we say that $f$ has this maximum at the point $p$. Similarly, suppose there is a point $q \in I$ such that $f(x) \geqslant f(q)$ for all $x \in I$. Then $f(q)$ is called the minimum of $f$ in $I$, and we say that $f$ has this minimum at the point $q$. We also say that " $f$ takes its maximum value at $p$ and its minimum value at $q . "$ Note that $f$ may well take its maximum (or minimum) value, provided there is one, at more than one point of $I$.

The word extremum refers to either a maximum or a minimum, and the phrase extreme value refers to either a maximum value or a minimum value. The words "maximum," "minimum" and "extremum" have Latin plurals, namely "maxima," "minima" and "extrema." Extrema of a different kind will be introduced in Sec. 3.51, and will be known as local extrema, as opposed to the kind of extrema considered here, which are often called global extrema.

b. For example, if $I$ is the closed interval $[-1,1]$, the function $x^{2}$ has its maximum in $I$, equal to 1 , at both points $x= \pm 1$, and its minimum in $I$, equal to 0 , at the point $x=0$. In the same interval, the function $x^{3}$ has its maximum, equal to 1 , at the point $x=1$, and its minimum, equal to -1 , at the point $x=-1$. A constant function, say $f(x) \equiv k$, has a maximum and a minimum, both equal to $k$, at every point of any interval. On the other hand, the function $x$ has neither a maximum nor a minimum in the open interval $(0,1)$, since there is no largest number less than 1 and no smallest number greater than 0 (Sec. 1.4, Prob. 12), and the same is true of the functions $x^{2}$ and $x^{3}$ in this interval. All three functions $x, x^{2}$ and $x^{3}$ have a minimum, equal to 0 , in the half-open interval $[0,1)$, at the point $x=0$, but again none of them has a maximum in $[0,1)$. As we now see, this failure to have one or both extrema cannot occur if the function is continuous in a closed interval.


### Theorem
If $f$ is continuous in a closed interval $I=[a, b]$, then $I$ contains points $p$ and $q$ such that

$$
f(q) \leqslant f(x) \leqslant f(p) \tag{1}
$$

for all $x \in I$. In other words, $f$ has both a maximum and a minimum in $I$, at the points $p$ and $q$, respectively. ^eef3d5

#### Proof
We can immediately exclude the case of a constant function, for which the theorem is trivially true. Thus, suppose $f$ is not a constant function. Then, by Theorem 3.31a, the range of $f$, namely the set $f(I)$, is a closed interval. Let this interval be $[m, M]$. Then, for all $x \in I$, we have $f(x) \in[m, M]$, or equivalently

$$
m \leqslant f(x) \leqslant M, \tag{2}
$$

by the very meaning of $f(I)$. Let $p$ be any point of $I$ such that $f(p)=M$ and $q$ any point of $I$ such that $f(q)=m$. Again, such points $p$ and $q$ exist by the very meaning of $f(I)$. We can then write (2) in the form (1).

d. Naturally, $M$ is the maximum of $f$ in $I$, and $m$ is the minimum of $f$ in $I$. Interpreted geometrically, the theorem means that the graph of a function $f$ continuous in a closed interval $I$ must have both a "highest point" $P=(p, M)$ and a "lowest point" $Q=(q, m)$, as illustrated by Figure 3. It is important to note that $f$ may take one or both of its extreme values at end points of $I$ (give examples), although this is not the case for the function shown in Figure 3.

![[../Essential Calculus with Applications/Images/Chapter 3/figure3.svg]]