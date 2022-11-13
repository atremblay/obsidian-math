---
aliases:
    - 'reduction of order'
tags:
    - "solution"
    - "second_order"
---

> [!summary]
> Assuming we have the solution $y_1$ of
> $$f_2(x)y''+f_1(x)y'+f_0(x)y=0$$
> Then the second solution is
> $$\boxed{y_2=y_1\int\frac{\exp\left\{-\int\frac{f_1(x)}{f_2(x)}dx\right\}}{y_1^2}dx}$$
> $y_2$ is linearly independant from $y_1$, so
> $$y_h=c_1y_1+c_2y_2$$
> as usual

> [!question]-
> It's called 'reduction of order', but why? What order are we reducing?

> [!tip]-
> The following will make use of:
> - [[Separation of variables]]

> [!warning]- 
> In all fairness, don't expect to be able to do this on your own. 

Let's assume that (where the heck does this come from?)
$$y_2=y_1(x)\int u(x)dx$$^c13b31

where $u(x)$ is an unknown function of $x$. The derivatives are 
$$
\begin{align}
y_2'&=y_1u+y_1'\int u(x) dx, \\
y_2''&=y_1u'+y_1'u+y_1'u+y_1''\int u(x)dx \\
&= y_1u'+2y_1'u+y_1''\int u(x)dx
\end{align}
$$
Replacing these in the [[Homogeneous Function|homogeneous function]]
$$
\begin{align}
&f_2(x)y_2''+f_1(x)y_2'+f_0(x)y_2 \\
&=f_2(x)\left(y_1u'+2y_1'u+y_1''\int u(x)dx\right)+f_1(x)\left(y_1u+y_1'\int u(x) dx\right)+f_0(x)\left(y_1(x)\int u(x)dx\right) \\
&=(\cancelto{0}{f_2(x)y_1''+f_1(x)y_1'+f_0(x)y_1})\int u(x)dx + f_2(x)y_1u'+(2f_2(x)y'+f_1(x)y_1)u \\
&= f_2(x)y_1u'+(2f_2(x)y_1'+f_1(x)y_1)u \\
&= 0
\end{align}
$$

Because we assumed that $y_{1}$ is a solution, we get rid of a part of the equation. We are left with
$$
\begin{align}
f_2(x)y_1u'+(2f_2(x)y_1'+f_1(x)y_1)u &= 0  \\
\iff f_2(x)y_1u'+2f_2(x)y_1'u &= -f_1(x)y_1u 
\end{align}
$$
It doesn't look like it, but one more manipulation will get use to a point where we can do a [[Separation of variables|separation of variables]]
$$
\begin{align}
f_2(x)y_1u'+2f_2(x)y_1'u &= -f_1(x)y_1u \\ \\
\iff \frac{f_2(x)y_1u'+2f_2(x)y_1'u}{f_2(x)y_1u} &= \frac{-f_1(x)y_1u}{{f_2(x)y_1u}}  \\
\iff \frac{u'}{u} + \frac{2y_1'}{y_1} &= \frac{-f_1(x)}{{f_2(x)}}
\end{align}
$$
Integrate both sides with respect to $x$

$$
\begin{align}
\int  \frac{1}{u} \frac{\mathrm{d}u}{\mathrm{d}x}\mathrm{d}x + \int  \frac{2}{y_1}\frac{\mathrm{d}y_{1}}{\mathrm{d}x}\mathrm{d}x &= \int  \frac{-f_1(x)}{{f_2(x)}}\mathrm{d}x  \\
\int  \frac{1}{u} \mathrm{d}u + \int  \frac{2}{y_1}\mathrm{d}y_{1} &= \int  \frac{-f_1(x)}{{f_2(x)}}\mathrm{d}x  \\
\log u + 2\log y_{1} &= \int  \frac{-f_1(x)}{{f_2(x)}}\mathrm{d}x  \\
\log (uy_{1}^2) &= \int  \frac{-f_1(x)}{{f_2(x)}}\mathrm{d}x  \\
uy_{1}^2 &= \exp\left\{\int  \frac{-f_1(x)}{{f_2(x)}}\mathrm{d}x\right\}  \\
u &= \frac{\exp\left\{\int  \frac{-f_1(x)}{{f_2(x)}}\mathrm{d}x\right\}}{y_{1}^2}
\end{align}
$$

Now that we have value for $u(x)$, we can plug it in the [[#^c13b31|initial assumption]]

$$
\begin{align}
y_2&=y_1(x)\int u(x)dx  \\
&= y_{1}(x)\int {\frac{\exp\left\{\int  \frac{-f_1(x)}{{f_2(x)}}\mathrm{d}x\right\}}{y_{1}^2}}
\end{align}
$$
###### References
[[@tenenbaum_ordinary_1985]] p.242

