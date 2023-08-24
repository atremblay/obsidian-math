---
aliases:
    - "matrix exponential"
---

Let $\mathbf{X}$ be an $n\times n$ real or complex matrix. The exponential of $\mathbf{X}$, denoted by $e^{\mathbf{X}}$ or $\exp(\mathbf{X})$, is the $n \times n$ matrix given by the [[../Calculus/Maclaurin Series#^f3c038|power series]]

$$
e^{\mathbf{X}} = \sum_{k=0}^{\infty} \frac{1}{k!}\mathbf{X}^k=\mathbf{I} + \sum_{k=1}^{\infty} \frac{1}{k!}\mathbf{X}^k
$$


> [!Note]- Discovery versus Definition
> This seems to be summoned out of nowhere. There is a difference between discovery and definition. The equality between $e^x$ and its power series was a discovery
> 
> Discovered
> $$\boxed{e^x=1+ x + \frac{x^2}{2!} + \frac{x^3}{3!} + \ldots}$$
> 
> People then tried to plug all sorts of things in the power series instead of the scalar $x$, such a matrices. An abuse of notation followed and a new definition was born
> 
> Definition
> $$\boxed{e^\mathbf{X}=1+ \mathbf{X} + \frac{\mathbf{X}^2}{2!} + \frac{\mathbf{X}^3}{3!} + \ldots}$$

# Property

1. If $\mathbf{ST}=\mathbf{TS}$ then $e^{\mathbf{S+T}}=e^{\mathbf{S}}e{^\mathbf{T}}$  ^06052f

Proof:
Use binomial theorem
$$
(S+T)^n=n! \sum_{j+k=n} \frac{S^jT^k}{j!k!}
$$

2. Matrix exponential integral
![[Matrix exponential integral#^68432f]]

