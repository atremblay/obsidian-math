---
tags: 
    - "first_order" 
    - "solution"
---

> [!summary]
> Standard form of a [[Ordinary differential equation#Order|first order]] [[Linear ODE|linear ODE]]
> $$
> y'+p(x)y = f(x)
> $$
> Solution: $$\boxed{y=\frac{1}{\textcolor{green}{r(x)}}\int \textcolor{green}{r(x)}f(x)dx}$$
> where $\textcolor{green}{r(x)}=e^{\int p(x)dx}$


The standard form of a first order linear ODE is written as 

$$y'+p(x)y = f(x)$$

Even if $y'$ has a coefficient, we simply divide everything by it to fall back on the standard form. Multiply both sides by the unknown function $\textcolor{green}{r(x)}$, which will be the integrating factor. 
$$\textcolor{green}{r(x)}y'+\textcolor{green}{r(x)}p(x)y = \textcolor{green}{r(x)}f(x)$$
We would like to have (no idea why or how the inventor thought about that)
$$\frac{d}{dx}\left[r(x)y\right]=r(x)f(x)$$
So that we can integrate both sides with respect to $x$

$$
\begin{align}
\textcolor{orange}{\int}\frac{d}{dx}\left[r(x)y\right]\textcolor{orange}{dx}&=\textcolor{orange}{\int} r(x)f(x)\textcolor{orange}{dx}\\
\iff r(x)y &=\int r(x)f(x)dx \\
\iff y &=\frac{1}{r(x)}\int r(x)f(x)dx
\end{align}
$$
Using the product rule of derivative
$$
\begin{align}
\frac{d}{dx}\left[r(x)y\right]=r(x)y'+r'(x)y=\underbrace{r(x)y'+r(x)p(x)y = r(x)f(x)}_{\textit{From above}}
\end{align}
$$

$$
\begin{align}
\cancel{r(x)y'}+r'(x)y&=\cancel{r(x)y'}+r(x)p(x)y \\
\iff r'(x)\cancel{y}&=r(x)p(x)\cancel{y} \\
\iff r'(x)&=r(x)p(x) \\
\iff \frac{r'(x)}{r(x)}&=p(x) \\
\end{align}
$$

Integrate both sides w.r.t. $x$ and isolate $r(x)$

$$
\begin{align}
\int \frac{r'(x)}{r(x)}dx&=\int p(x)dx \\
\iff \ln{r(x)} &= \int p(x)dx \\
\iff r(x) &= e^{\int p(x)dx} \\
\end{align}
$$
Combine this result with the one from above, we get the solution to this first order linear ODE by using the integrating factor $r(x)$
$$y =\frac{1}{r(x)}\int r(x)f(x)dx$$where $r(x)=e^{\int p(x)dx}$


-----

Slightly more detailed by keeping the integration constant

$$
\begin{align}
\int \frac{r'(x)}{r(x)}dx&=\int p(x)dx \\
\iff \ln{r(x)} + C_1 &= \int p(x)dx \\
\iff \ln{r(x)} &= \int p(x)dx - C_1\\
\iff r(x) &= e^{\int p(x)dx - C_1} \\
\iff r(x) &= e^{-C_1}e^{\int p(x)dx} \\
\iff r(x) &= Ce^{\int p(x)dx} \\
\end{align}
$$

It will just cancel out here
$$
\begin{align}
y &=\frac{1}{r(x)}\int r(x)f(x)dx \\
y &=\frac{1}{\cancel{C}e^{\int p(x)dx}}\int \cancel{C}e^{\int p(x)dx}f(x)dx \\
\end{align}
$$

So we can simply say that $r(x)=e^{\int p(x)dx}$ and not bother with the integration constant.


> [!Example]-
> $$y'+4y=e^{-x},\hspace{2em}y(0)=\frac{4}{3}$$
> 
> 
> $$y'+\underbrace{4}_{p(x)}y=\underbrace{e^{-x}}_{f(x)}$$
> 
> 
> $$
> \begin{align}
> \textcolor{green}{r(x)}&=e^{\int p(x)dx} \\
> &= e^{\int 4dx} \\
> &= e^{4x}
> \end{align}
> $$
> 
> $$
> \begin{align}
> y&=\frac{1}{\textcolor{green}{r(x)}}\int \textcolor{green}{r(x)}f(x)dx \\
> &= \frac{1}{\textcolor{green}{e^{4x}}}\int \textcolor{green}{e^{4x}}e^{-x}dx \\
> &= \frac{1}{e^{4x}}\int e^{3x}dx \\
> &= \frac{1}{e^{4x}} \left( \frac{1}{3} e^{3x} + C \right) \\
> &= \frac{1}{3e^x}  + \frac{C}{e^{4x}} \\
> \end{align}
> $$
> Plugin the initial condition to get the value of C
> 
> $$
> \begin{align}
> y(0)&= \frac{1}{3e^0}  + \frac{C}{e^{0}} = \frac{4}{3} \\
> \iff C &= 1
> \end{align}
> $$
> 
> 
> $$
> \boxed{
> y=\frac{1}{3e^x}  + \frac{1}{e^{4x}} 
> }
> $$