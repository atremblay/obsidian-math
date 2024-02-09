### Theorem
Suppose $f(x)$ and $g(x)$ have indefinite integrals in the same interval $I$. Then

$$
\int[a f(x)+b g(x)] d x=a \int f(x) d x+b \int g(x) d x \tag{1}
$$

where $a$ and $b$ are arbitrary constants.

#### Proof
It follows from (5), applied to the function $a f(x)+b g(x)$, that

$$
\frac{d}{d x} \int[a f(x)+b g(x)] d x=a f(x)+b g(x) .
$$

On the other hand, by the usual rules of differentiation,

$\frac{d}{d x}\left[a \int f(x) d x+b \int g(x) d x\right]=a \frac{d}{d x} \int f(x) d x+b \frac{d}{d x} \int g(x) d x=a f(x)+b g(x)$,

where we use (4) twice more. Thus the two sides of (1) have the same derivative in $I$, and hence can differ only by an arbitrary constant, by the same argument as in the proof of [[Antiderivative#Theorem|this theorem]]. But then (1) holds, since the indefinite integral on the left is defined only to within an arbitrary "additive constant."

By virtually the same argument, you can easily convince yourself of the validity of the more general formula

$$
\begin{aligned}
\int\left[c_{1} f_{1}(x)+c_{2} f_{2}(x)+\cdots+c_{n} f_{n}(x)\right] d x &=c_{1} \int f_{1}(x) d x+c_{2} \int f_{2}(x) d x+\cdots+c_{n} \int f_{n}(x) d x
\end{aligned} \tag{2}
$$

where $f_{1}(x), f_{2}(x), \ldots, f_{n}(x)$ are functions which have indefinite integrals in the same interval, and $c_{1}, c_{2}, \ldots, c_{n}$ are arbitrary constants.

---

Let $F(x)$ be an antiderivative of $f(x)$ in $I$, so that $F^{\prime}(x)=f(x)$ for every $x \in I$. Then, as just shown, the "general antiderivative" of $f(x)$ in $I$ is of the form

$$
F(x)+C,
$$

where $C$ is an arbitrary constant. This expression is also called the indefinite integral of $f(x)$, and is denoted by

$$
\int f(x) d x \tag{3}
$$

Thus

$$
\int f(x) d x=F(x)+C \tag{4}
$$

by definition, so that the indefinite integral is defined only to within an arbitrary "additive constant." The operation leading from the function $f(x)$, called the integrand, to the expression (3) is called (indefinite) integration, with respect to $x$, the argument $x$ is called the variable of integration, and the constant $C$ is called the constant of integration. Note that the expression behind the integral sign in (3) is the product of the integrand $f(x)$ and the differential $d x$ of the variable of integration. Recalling Sec. $2.55 \mathrm{a}$, we recognize this product as the differential

$$
d F(x)=F^{\prime}(x) d x=f(x) d x
$$

of the antiderivative $F(x)$, so that (4) can also be written as

$$
\int d F(x)=F(x)+C
$$

In writing (4), it is tacitly assumed that the formula is an identity for all $x$ in some underlying interval $I$ in which $f(x)$ and $F(x)$ are both defined; however, $I$ is usually left unspecified. The convention is to give $f$ and $F$ the same argument, in keeping with the formula $F^{\prime}(x)=f(x)$, but we could just as well write

$$
\int f(t) d t=F(t)+C
$$

say, replacing $x$ by some other symbol (here $t$ ) in both sides of (4). Differentiating (4), we get

$$
\frac{d}{d x} \int f(x) d x=\frac{d}{d x}[F(x)+C]=\frac{d F(x)}{d x}=F^{\prime}(x),
$$

so that

$$
\frac{d}{d x} \int f(x) d x=f(x) \tag{5} 
$$


Since a function $f(x)$ is obviously an antiderivative of its own derivative $f^{\prime}(x)$, we have

$$
\int f^{\prime}(x) d x=f(x)+C .
$$

This formula can be used to derive an integration formula from any differentiation formula. For example, the formula

$$
\frac{d}{d x} \frac{x^{r+1}}{r+1}=\frac{(r+1) x^{r}}{r+1}=x^{r}
$$

(Sec. 2.74e), valid for arbitrary real $r \neq-1$, leads at once to the formula

$$
\int x^{r} d x=\frac{x^{r+1}}{r+1}+C \tag{6}
$$

if $r \neq-1$. Choosing $r=-\frac{1}{2}$ in (6), we get the formula

$$
\int \frac{d x}{\sqrt{x}}=2 \sqrt{x}+C
$$

valid in the interval $(0, \infty)$, but in no larger interval, while the choice $r=\frac{1}{3}$ gives the formula

$$
\int x^{1 / 3} d x=\frac{3}{4} x^{4 / 3}+C
$$

valid in the whole interval $(-\infty, \infty)$.



### Example

Evaluate

$$
\int\left(5 x^{4}-6 x^{2}+\frac{2}{x^{2}}\right) d x
$$

Solution. By (1), we have

$\int\left(5 x^{4}-6 x^{2}+\frac{2}{x^{2}}\right) d x=5 \int x^{4} d x-6 \int x^{2} d x+2 \int \frac{d x}{x^{2}}=x^{5}-2 x^{3}-\frac{2}{x}+C$,

Note that the constants of integration contributed by each of the three integrals separately can be combined into a single constant of integration $C$.

> [!multi-column]
> > [!blank]
> > ```desmos-graph
> >  left=-0.75; right=3; top=3; bottom=-2; width=400
> > ---
> >  y=(x-1)^3
> > ```
> > > Figure 6a: $(x-1)^3$
> 
> > [!blank]
> > ```desmos-graph
> >  left=-2; right=2; top=1.5; bottom=-1; width=400
> > ---
> >  y=1-x^4
> > ```
> > > Figure 6b: $1-x^4$

