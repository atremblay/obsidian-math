---
tags: 
    - "first_order" 
    - "solution"
---
- [ ] There are mistakes here  see page 95-96

> [!summary]
> Special form of an ODE on which we can use the [[Integrating factor|integrating factor]]
> $$
> \begin{align}
> y'+P(x)y &= Q(x)y^n \\
> \Rightarrow \frac{1}{1-n}u' + P(x)u &= Q(x)
> \end{align}
> $$
> where $u=y^{1-n}$


This form is almost equivalent to the base form for the [[Integrating factor|integrating factor]]
$$
x'+P(y)x = Q(y)x^n
$$

Subsitute $\textcolor{orange}{u=x^{1-n}}$ (this is thing to remember, the rest can be derived from it)
Derive 
$$
\begin{align}
\frac{du}{dx}&=u'=(1-n)x^{-n}x' \\
\iff \textcolor{violet}{\frac{x'}{x^n}} &= \textcolor{violet}{\frac{u'}{1-n}}
\end{align}
$$
Reorganizing
$$
\begin{align}
x'+P(y)x &= Q(y)x^n \\
\textcolor{violet}{\frac{x'}{x^n}}+P(y)\textcolor{orange}{x^{1-n}} &= Q(y) \\
\textcolor{violet}{\frac{u'}{1-n}}+P(y)\textcolor{orange}{u} &= Q(y) \\
u'+(1-n)P(y)u &= (1-n)Q(y) \\
\end{align}
$$
We can now use the [[Integrating factor|integrating factor]]


> [!Example]-
> $$
> x'-5x=\frac{-5}{2}yx^3
> $$
> The necessary leg work
> $P(y)=-5$, $Q(y)=\frac{-5y}{2}$
> $\textcolor{orange}{u=x^{1-n}=x^{1-3}=x^{-2}}$
> $\textcolor{violet}{u'=\frac{-2}{x^3}}$
> 
> Plug everything in
> $u'+10u = 5x$
> 
> Then use the [[integrating factor]]
> 
> $p(y)=10$, $f(y)=5x$
> $r(y)=e^{\int p(y)dy}=e^{\int 10 dy} = e^{10y}$
> 
> $$
> \begin{align}
> x&=\frac{1}{r(y)} \int r(y) f(y)dx \\
> &= \frac{1}{e^{10y}} \int e^{10y} 5ydy \\
> &= \frac{5}{e^{10y}} \textcolor{lime}{\int e^{10y}y dy}
> \end{align}
> $$
> Integration by parts
> $$
> \frac{5}{e^{10y}}\left(\textcolor{lime}{y\frac{e^{10y}}{10} - \frac{e^{10y}}{100} +C}\right) = \frac{y}{2} - \frac{1}{20} +C\frac{5}{e^{10y}}
> $$
> And since C is some arbitrary constant, we merge the 5 with it.
> 
> $$\boxed{x=\frac{y}{2} - \frac{1}{20} +\frac{C}{e^{10y}}}$$


