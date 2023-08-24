
> [!summary]
> If $x=g(\theta)$ then we have this equivalence by the [[../Derivative/Chain Rule|chain rule]]
> 
> $$
> \int f(x)dx = \int f(g(\theta))d g(\theta) = \int f(g(\theta))\frac{dg(\theta)}{d\theta} d\theta
> $$
> In integration, if we can find something that can be used as $g(\theta)$ and $\frac{dg(\theta)}{d\theta}$ then we can subsitute and get the same integral 


By the [[../Derivative/Chain Rule|chain rule]]

$$
\frac{dF(g(x))}{dx} = \frac{d F(g(x))}{d g(x)} \frac{d g(x)}{dx}
$$

We take the integral with respect to x on both sides

$$
\begin{align}
\int \frac{d}{dx} F(g(x)) dx &= \int \frac{d F(g(x))}{d g(x)} \frac{d g(x)}{dx} dx \\
F(g(x)) + C &= \int \frac{d F(g(x))}{d g(x)} \frac{d g(x)}{dx} dx
\end{align}
$$
Substitute $u=g(x)$

$$
\begin{align}
F(g(x)) + C &= \int \frac{d F(g(x))}{d g(x)} \frac{d g(x)}{dx} dx \\
F(u) + C &= \int \frac{d F(u)}{du} \frac{du}{dx} dx
\end{align}
$$
While we used the antiderivative above, we come back to an integral form, but with respect to $u$

$$
\begin{align}
F(g(x)) + C = \underbrace{F(u) + C  = \int \frac{d F(u)}{du}du}_{\textit{Antiderivate\\Derivative}} = \int \frac{d F(u)}{du} \frac{du}{dx} dx
\end{align}
$$
This seems a bit pointless, but it's just convenient to go from $\int \frac{d F(g(x))}{dx} dx$ to $\int \frac{d F(u)}{du} du$

$$
\underbrace{\int \frac{dF(g(x))}{dx}dx = F(g(x)) +C}_{\textit{Derivative/Antiderivate}} = \underbrace{F(u) + C = \int \frac{d F(u)}{du}du}_{\textit{Derivative/Antiderivate}}
$$
Putting all of this together we get

$$
\int \frac{d F(u)}{du}du = \int \frac{d F(u)}{du} \frac{du}{dx} dx
$$

The point of this is that if we can find a u-substitution to get in the form of the right-hand side, then we can do the integral on the left-hand side and get the result.


> [!Tip]
> Notice how the integrands on both side is the same thing as the chain-rule $$\frac{d F(u)}{du} = \frac{d F(u)}{du} \frac{du}{dx}$$

Matching up both sides we have $du = \frac{du}{dx}dx$

Example

$$\int 2x \sin(x^2)dx$$

Set $u = x^2$ and $du = 2xdx$

> [!question] 
> Why can we treat $\frac{du}{dx}$ like a fraction? What sort of gymnastic allows this? Is this like the [[../../Ordinary differential equations/Solutions/Separation of variables]] where treating it like a fraction is a convenient fiction?





$$\int 2x \sin(x^2)dx= \int sin(u)du = -\cos(u) + C = -\cos(x^2) + C$$

```dataview
table from #substitution_rule 
```
