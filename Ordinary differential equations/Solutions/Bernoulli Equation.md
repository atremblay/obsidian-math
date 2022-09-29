---
tags: 
    - "first_order" 
    - "solution"
---

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
y'+P(x)y = Q(x)y^n
$$

Subsitute $\textcolor{orange}{u=y^{1-n}}$ (this is thing to remember, the rest can be derived from it)
Derive 
$$
\begin{align}
\frac{du}{dy}&=u'=(1-n)y^{-n}y' \\
\iff \textcolor{violet}{\frac{y'}{y^n}} &= \textcolor{violet}{\frac{u'}{1-n}}
\end{align}
$$
Reorganizing
$$
\begin{align}
y'+P(x)y &= Q(x)y^n \\
\textcolor{violet}{\frac{y'}{y^n}}+P(x)\textcolor{orange}{y^{1-n}} &= Q(x) \\
\textcolor{violet}{\frac{u'}{1-n}}+P(x)\textcolor{orange}{u} &= Q(x) \\
u'+(1-n)P(x)u &= (1-n)Q(x) \\
\end{align}
$$
We can now use the [[Integrating factor|integrating factor]]


> [!Example]-
> $$
> y'-5y=\frac{-5}{2}xy^3
> $$
> The necessary leg work
> $P(x)=-5$, $Q(x)=\frac{-5x}{2}$
> $\textcolor{orange}{u=y^{1-n}=y^{1-3}=y^{-2}}$
> $\textcolor{violet}{u'=\frac{-2}{y^3}}$
> 
> Plug everything in
> $u'+10u = 5x$
> 
> Then use the [[integrating factor]]
> 
> $p(x)=10$, $f(x)=5x$
> $r(x)=e^{\int p(x)dx}=e^{\int 10 dx} = e^{10x}$
> 
> $$
> \begin{align}
> y&=\frac{1}{r(x)} \int r(x) f(x)dx \\
> &= \frac{1}{e^{10x}} \int e^{10x} 5xdx \\
> &= \frac{5}{e^{10x}} \textcolor{lime}{\int e^{10x}x dx}
> \end{align}
> $$
> Integration by parts
> $$
> \frac{5}{e^{10x}}\left(\textcolor{lime}{x\frac{e^{10x}}{10} - \frac{e^{10x}}{100} +C}\right) = \frac{x}{2} - \frac{1}{20} +C\frac{5}{e^{10x}}
> $$
> And since C is some arbitrary constant, we merge the 5 with it.
> 
> $$\boxed{\frac{x}{2} - \frac{1}{20} +\frac{C}{e^{10x}}}$$


