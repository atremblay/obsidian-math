> [!blank|right-medium]
> ![[Images/Chapter 3/figure7.svg]]
> > Figure 1

The figure 1 shows the graph of a function $f$ continuous in a closed interval [a, b].  $f$ takes its maximum in $[a, b]$, equal to $M$, at the "highest point" of the graph, namely $P=(p, M)$, and its minimum in $[a, b]$, equal to $m$, at the "lowest point" of the graph, namely $A=(a, m)$, which this time happens to be an end point of the graph. But now there also seems to be something special about the behavior of the graph at certain other points, namely $Q, R$ and $S$. In fact, $Q$ is "higher" than all "nearby points" of the graph, although not as high as $P$, while each of the points $R$ and $S$ is "lower" than all "nearby points" of the graph, although not as low as $A$.

b. We now make these qualitative notions precise. Let $f$ be a function defined in an interval $I$, and suppose there is a point $p \in I$ such that $f(x) \leqslant f(p)$ for all $x$ "sufficiently near" $p$, that is, for all $x$ in some neighborhood of $p$. Then $f$ is said to have a local maximum, equal to $f(p)$, at the point $p$. Similarly, suppose there is a point $r \in I$ such that $f(x) \geqslant f(r)$ for all $x$ in some neighborhood of $q$. Then $f$ is said to have a local minimum, equal to $f(r)$, at the point $r$. The term local extremum refers to a local maximum or a local minimum.


A function can have several distinct (that is, different) local maxima or minima. For example, the function $f$ shown in Figure 1 has distinct local maxima at the points $p$ and $q$, and distinct local minima at the points $r$ and $s$. The global minimum of $f$ at the point $a$ is not a local minimum, since $f$ is not defined in a neighborhood of $a$ (it is not enough to be defined on only one side of $a$ ). For the same reason, a function defined in an interval $I$ can have local extrema only at interior points of $I$, that is, at points of $I$ other than the end points of $I$. The function $f$ in Figure 7 has neither a global extremum nor a local extremum at the point $b$, but $b$ is still special in the sense that it is an end point of the interval $[a, b]$. Note that $f$ has both a global maximum and a local maximum at the point $p$. In fact, if a function $f$ is defined in an interval $I$, then any global extremum of $f$ at an interior point of $I$ is automatically a local extremum of f.

A local maximum $f(p)$ is said to be strict if $f(x)<f(p)$, with $<$ instead of $\leqslant$, for all $x$ "sufficiently near" $p$ but not equal to $p$, that is, for all $x$ in some deleted neighborhood of $p$ (Sec. 1.63a). Similarly, a local minimum $f(q)$ is said to be strict if $f(x)>f(q)$, with $>$ instead of $\geqslant$, for all $x$ in some deleted neighborhood of $q$. For example, all four local extrema of the function in Figure 7 are strict. On the
other hand, a constant function has both a local maximum and a local minimum at every point, but none of these extrema is 

### Theorem

> [!blank|right-small]
> ![[../../Essential Calculus with Applications/Images/Chapter 3/figure8a.svg]]
> > Figure 2. We will often assume that $f$ is continuous at $p$. 
> 
> ![[../../Essential Calculus with Applications/Images/Chapter 3/figure8b.svg]]
> > Figure 3.

If $f$ has a local extremum at a point $p$, then either $f$ fails to have a derivative at $p$, or $f^{\prime}(p)$ exists and equals zero. (That point is called a [[../Critical Point]])

#### Proof (by contradiction)
Either $f^{\prime}(p)$ exists or it does not. Suppose $f^{\prime}(p)$ exists and is nonzero. Then $f^{\prime}(p)$ is either positive or negative. In the first case, $f$ is increasing in some neighborhood of $p$, while in the second case, $f$ is decreasing in some neighborhood of $p$. In either case, this neighborhood, or any smaller neighborhood, contains values of $f$ larger than $f(p)$ and values of $f$ smaller than $f(p)$, so that $f(p)$ cannot be either a local maximum or a local minimum of $f$. It follows that $f^{\prime}(p)=0$.

This argument, closely resembles that used in the proof of [[../Rolle’s Theorem|Rolle’s Theorem]].

### Geometrical Interpretation


The theorem says that if a function $f$ has a local extremum at a point $p$, then either the graph of $f$ has no tangent at the point $P=(p, f(p))$, if $f^{\prime}(p)$ fails to exist, or the graph of $f$ has a horizontal tangent at $P$, if $f^{\prime}(p)=0$. These two possibilities are illustrated in Figure 2, where each of the functions has a (strict) local maximum at $p$. Note that it’s not because a point has no derivative that it’s a local extrema. See blue line in figure 3

