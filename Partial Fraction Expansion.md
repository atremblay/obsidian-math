

Basic algebra tells us how to add two fractions
$$
\begin{align}
\frac{2}{x-1} + \frac{3}{x+1} \tag{a}
\end{align}
$$
and get a simplified fraction by finding the least common denominator
$$
\begin{align}
\frac{5x-1}{x^2-1} \tag{b}
\end{align}
$$

But what if we want to go in reverse, from (b) to (a)? 

1. First, we need to factorise the denominator $x^2-1 = (x-1)(x+1)$  
2. Write the simplified fraction as the addition of two fractions with unknown values for $A$ and $B$ 
$$
\frac{5x-1}{x^2-1} = \frac{A}{x-1} + \frac{B}{x+1}\tag{c}
$$
3. Do the least common denominator with the unknowns
$$
\begin{align}
\frac{5x-1}{x^2-1} &= \frac{Ax + A + Bx - B}{x^2-1} \\
&= \frac{x(A+B) + (A - B)}{x^2-1}\tag{d}
\end{align}
$$
4. We can cancel the denominator on both sides of (d)
$$
5x-1 = x(A+B) + (A - B) \tag{e}
$$
5. This is a polynomial. Two polynomials are equal if the coefficients of like powers of $x$ on both sides are equal
$$
\begin{align}\tag{f}
A+B&=5, \\
A-B&=-1
\end{align}
$$
6. Solving (f) yields $A=2$ and $B=3$. Plugging this in (c)
$$
\frac{5x-1}{x^2-1} = \frac{2}{x-1} + \frac{3}{x+1}\tag{g}
$$
which was the original fraction (a)

## A more complex example leading to generalization

The general rule for determining the form of each numerator in a partial fraction expansion of a quotient $P(x)/Q(x)$, where $P(x)$ is of *degree less than $Q(x)$*, will start to fall in place with this example.

Let
$$
Q(x) = (x+a)(x^3+b)(x^2+c)^2(x+d)^3, \tag{h}
$$
and let $P(x)$ be a polynomial of degree less than $Q(x)$.

> [!important] $\text{degree}(P(x)) < \text{degree}(Q(x))$
> This is a requirement

Then the partial fraction expansion of $P(x)/Q(x)$ will have the form
$$
\frac{P(x)}{Q(x)} = \frac{A}{x+a} + \frac{Bx^2 + Cx + D}{x^3 + b} + \frac{Ex+F}{\textcolor{orange}{x^2+c}} + \frac{Gx+H}{\textcolor{cyan}{(x^2+c)^2}} + \frac{I}{\textcolor{violet}{x+d}} + \frac{J}{\textcolor{lime}{(x+d)^2}} + \frac{K}{\textcolor{lightblue}{(x+d)^3}}
$$

> 1. Each numerator is a polynomial of degree one less than the degree of the term **inside** the parenthesis of its denominator. e.g. $\text{degree}(Bx^2 + Cx + D) = \text{degree}(x^3+b) - 1$
> 2. A term such as $(x^2+c)^2$ has the exponent of two **outside** the parenthesis. Hence $(x^2+c)$ appears twice in the denominator, once as $\textcolor{orange}{(x^2+c)}$, the second time as $\textcolor{cyan}{(x^2+c)^2}$
> 3. A term such as $(x+d)^3$ has the exponent three **outside** the parenthesis. Hence $(x+d)^3$ appears three times in the denominator, once as $\textcolor{violet}{(x+d)}$, the second time as $\textcolor{lime}{(x+d)^2}$, and the third time as $\textcolor{lightblue}{(x+d)^3}$.


> [!example] 
> $$
> \begin{align}
> \frac{2}{u^2(2u+1)}&=\frac{A u+B}{u^2}+\frac{C}{2u+1} \\
> \frac{2}{u^2(2u+1)}&=\frac{(Au+B)(2u+1)+C u^2}{u^2(2u+1)} \\
> 2&=2Au^2+Au+2Bu+B+C u^2 \\
> 2&=(2A+C) u^2+(A+2B)u+B \\
> \end{align}
> $$
>  Matching coefficients of powers of $u$ gives
> $$B=2, 2A+C = 0, A+2B=0 \implies A =-4, C=8$$
> Replace
> $$
> \begin{align}
> \frac{-4u+2}{u^2}+\frac{8}{2u+1} &=\frac{(-4u+2)(2u+1)+ 8u^2}{u^2(2u+1)} \\
> &=\frac{-8u^2+4u -4u+2+ 8u^2}{u^2(2u+1)} \\
> &=\frac{2}{u^2(2u+1)} \\
> \end{align}
> $$


> [!NOTE] 
> There are a few more tips and tricks to do partial fraction expansion, but they are not noted here. Check Lesson 26A, p. 284, of [[Reading notes/@tenenbaum_ordinary_1985|@tenenbaum_ordinary_1985]]


```dataview
LIST FROM #partial_fraction 
```