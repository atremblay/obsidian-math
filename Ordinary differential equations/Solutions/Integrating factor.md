---
tags: 
    - "first_order" 
    - "solution"
---

> [!summary]
> Standard form of a [[../Definitions/Ordinary differential equation#Order|first order]] [[../Definitions/Linear ODE|linear ODE]]
> $$
> x'+p(y)x = f(y)
> $$
> Solution: $$\boxed{x=\frac{1}{\textcolor{green}{r(y)}}\int \textcolor{green}{r(y)}f(y)dy}$$
> where $\textcolor{green}{r(y)}=e^{\int p(y)dy}$
> The only constant of integration we need to worry anywhere is in the last integral $\int r(y)f(y)dy$


The standard form of a first order linear ODE is written as 

$$x'+p(y)x = f(y)$$

Even if $x'$ has a coefficient, we simply divide everything by it to fall back on the standard form. Multiply both sides by the unknown function $\textcolor{green}{r(y)}$, which will be the integrating factor. 
$$\textcolor{green}{r(y)}x'+\textcolor{green}{r(y)}p(y)x = \textcolor{green}{r(y)}f(y)$$
We would like to have (no idea why or how the inventor thought about that)
$$\frac{d}{dy}\left[r(y)x\right]=r(y)f(y)$$
So that we can integrate both sides with respect to $y$

$$
\begin{align}
\textcolor{orange}{\int}\frac{d}{dy}\left[r(y)x\right]\textcolor{orange}{dy}&=\textcolor{orange}{\int} r(y)f(y)\textcolor{orange}{dy}\\
\iff r(y)x &=\int r(y)f(y)dy \\
\iff x &=\frac{1}{r(y)}\int r(y)f(y)dy
\end{align}
$$
Using the [[../../Calculus/Derivative/Chain Rule|chain rule]] of derivative
$$
\begin{align}
\frac{d}{dy}\left[r(y)x\right]=r(y)x'+r'(y)x=\underbrace{r(y)x'+r(y)p(y)x = r(y)f(y)}_{\textit{From above}}
\end{align}
$$

$$
\begin{align}
\cancel{r(y)x'}+r'(y)x&=\cancel{r(y)x'}+r(y)p(y)x \\
\iff r'(y)\cancel{x}&=r(y)p(y)\cancel{x} \\
\iff r'(y)&=r(y)p(y) \\
\iff \frac{r'(y)}{r(y)}&=p(y) \\
\end{align}
$$

Integrate both sides w.r.t. $y$ and isolate $r(y)$

$$
\begin{align}
\int \frac{r'(y)}{r(y)}dx&=\int p(y)dy \\
\iff \ln{r(y)} &= \int p(y)dy \\
\iff r(y) &= e^{\int p(y)dy} \\
\end{align}
$$
Combine this result with the one from above, we get the solution to this first order linear ODE by using the integrating factor $r(y)$
$$x =\frac{1}{r(y)}\int r(y)f(y)dy$$where $r(y)=e^{\int p(y)dy}$


-----

Slightly more detailed by keeping the integration constant

$$
\begin{align}
\int \frac{r'(y)}{r(y)}dy&=\int p(y)dy \\
\iff \ln{r(y)} + C_1 &= \int p(y)dy \\
\iff \ln{r(y)} &= \int p(y)dy - C_1\\
\iff r(y) &= e^{\int p(y)dy - C_1} \\
\iff r(y) &= e^{-C_1}e^{\int p(y)dy} \\
\iff r(y) &= Ce^{\int p(y)dy} \\
\end{align}
$$

It will just cancel out here
$$
\begin{align}
x &=\frac{1}{r(y)}\int r(y)f(y)dy \\
x &=\frac{1}{\cancel{C}e^{\int p(y)dy}}\int \cancel{C}e^{\int p(y)dy}f(y)dy \\
\end{align}
$$

Similarly, we can drop the constant of integration in the integral of the exponential
$$
e^{\int p(y)dy} = e^{P(y) + C_1}= e^{P(y)}e^{C_1} = Ce^{P(y)}
$$
Where $P(y)$ is the integral of $p(y)$
$$
\begin{align}
x &=\frac{1}{e^{\int p(y)dy}}\int e^{\int p(y)dy}f(y)dy \\
&=\frac{1}{Ce^{P(y)}}\int Ce^{P(y)}f(y)dy \\
&=\frac{1}{\cancel{C}e^{P(y)}}\int\cancel{C}e^{P(y)}f(y)dy \\
\end{align}
$$
So we can simply say that $r(y)=e^{\int p(y)dy}$ and not bother with **any** of the integration constant.


> [!Example]-
> $$x'+4x=e^{-y},\hspace{2em}x(0)=\frac{4}{3}$$
> 
> 
> $$x'+\underbrace{4}_{p(y)}x=\underbrace{e^{-y}}_{f(y)}$$
> 
> 
> $$
> \begin{align}
> \textcolor{green}{r(y)}&=e^{\int p(y)dy} \\
> &= e^{\int 4dy} \\
> &= e^{4y}
> \end{align}
> $$
> 
> $$
> \begin{align}
> x&=\frac{1}{\textcolor{green}{r(y)}}\int \textcolor{green}{r(y)}f(y)dx \\
> &= \frac{1}{\textcolor{green}{e^{4y}}}\int \textcolor{green}{e^{4y}}e^{-y}dy \\
> &= \frac{1}{e^{4y}}\int e^{3y}dy \\
> &= \frac{1}{e^{4y}} \left( \frac{1}{3} e^{3y} + C \right) \\
> &= \frac{1}{3e^y}  + \frac{C}{e^{4y}} \\
> \end{align}
> $$
> Plugin the initial condition to get the value of C
> 
> $$
> \begin{align}
> x(0)&= \frac{1}{3e^0}  + \frac{C}{e^{0}} = \frac{4}{3} \\
> \iff C &= 1
> \end{align}
> $$
> 
> 
> $$
> \boxed{
> x=\frac{1}{3e^y}  + \frac{1}{e^{4y}} 
> }
> $$


```dataview
list from #integrating_factor 
```