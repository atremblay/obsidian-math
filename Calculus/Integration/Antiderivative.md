### Theorem
Let $F(x)$ be any antiderivative of $f(x)$ in an interval $I$. Then every other antiderivative of $f(x)$ in $I$ is of the form 
$$
G(x) \equiv F(x)+C \tag{1}
$$

where $C$ is an arbitrary constant.

#### Proof
Let $G(x)$ be any other antiderivative of $f(x)$ in $I$, and let $H(x)=$ $G(x)-F(x)$. Then

$$
H^{\prime}(x)=G^{\prime}(x)-F^{\prime}(x)=f(x)-f(x)=0
$$

for every $x \in I$, that is, the derivative $H^{\prime}$ vanishes identically in $I$. It follows from the [[Derivation of a Constant]] that $H$ has the same value, call it $C$, at every point of $I$. In other words,

$$
H(x)=G(x)-F(x) \equiv C,
$$

which is equivalent to (1).

---

Given a function $f(x)$ defined in an interval $I$, suppose $F(x)$ is another function defined in $I$ such that

$$
\frac{d F(x)}{d x}=F^{\prime}(x)=f(x)
$$

for every $x \in I$. Then $F(x)$ is said to be an antiderivative of $f(x)$, in the interval $I$. For example, $\frac{1}{3} x^{3}$ is an antiderivative of $x^{2}$ in $(-\infty, \infty)$, since

$$
\frac{d}{d x} \frac{1}{3} x^{3}=x^{2}
$$

for every $x \in(-\infty, \infty)$. If $F(x)$ is an antiderivative of $f(x)$ in $I$, then so is (1) since

$$
\frac{d G(x)}{d x}=\frac{d}{d x}[F(x)+C]=\frac{d F(x)}{d x}+\frac{d C}{d x}=\frac{d F(x)}{d x}=f(x) .
$$



