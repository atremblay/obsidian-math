
> [!Summary]
> Integration by parts uses the product rule of derivative by working backward.
> Indefinite integral
> $$
> \int u\frac{dv}{dx}dx = uv - \int v\frac{du}{dx}dx
> $$
> 
> Definite integral
> $$
> \int_a^b u\frac{dv}{dx}dx = uv\biggr\vert_a^b - \int_a^b v\frac{du}{dx}dx
> $$

From the product rule of derivation we have 
$$\frac{d}{dx}u(x)v(x) =  v(x)\frac{du(x)}{dx} + u(x)\frac{dv(x)}{dx}$$
Integrate both sides
$$
\begin{align}
\int \frac{d}{dx}u(x)v(x) dx &= \int v(x) \frac{du(x)}{dx} dx + \int u(x)\frac{dv(x)}{dx} dx \\
u(x)v(x)&= \textcolor{orange}{\int v(x)\frac{du(x)}{dx} dx} + \textcolor{violet}{\int u(x)\frac{dv(x)}{dx} dx} \\
\end{align}
$$

>The above is the indefinite integral. If we look at the definite integral then we have to evaluate, even on the left side since we did the integral
>$$u(x)v(x)\biggr\vert_a^b= \int_a^b v(x)\frac{du(x)}{dx} dx + \int_a^b u(x)\frac{dv(x)}{dx} dx$$


To keep the notation lighter we will only keep $u$ and $v$ so that
$$
uv= \textcolor{orange}{\int v\frac{du}{dx} dx} + \textcolor{violet}{\int u\frac{dv}{dx} dx}
$$
Keep in mind that $u$ and $v$ are functions of $x$.

> We can apply the [[Substitution rule|substitution rule]] to $\textcolor{orange}{\int v\frac{du}{dx} dx}$ and $\textcolor{violet}{\int u\frac{dv}{dx} dx}$ to get $\textcolor{orange}{\int vdu}$ and $\textcolor{violet}{\int udv}$, but I find it easier to keep them as is for now.

We will work with the result above, just rearranged a little bit
$$
\begin{align}
uv&= \textcolor{orange}{\int v\frac{du}{dx} dx} + \textcolor{violet}{\int u\frac{dv}{dx} dx} \\
\textcolor{violet}{\int u\frac{dv}{dx} dx}&= uv -\textcolor{orange}{\int v\frac{du}{dx} dx}  \\
\end{align}
$$
We could have isolated $\textcolor{orange}{\int v\frac{du}{dx} dx}$ instead, it doesn't matter.

So when we face an integral
1. Try to match two functions to $\textcolor{violet}{u}$ and $\textcolor{violet}{\frac{dv}{dx}}$
2. Calculate $\textcolor{cyan}{v=\int \textcolor{violet}{\frac{dv}{dx}}dx}$* and $\textcolor{yellow}{\frac{du}{dx}}$
4. Plug these results in $\textcolor{orange}{\int} \textcolor{cyan}{v}\textcolor{yellow}{\frac{du}{dx}} \textcolor{orange}{dx}$ and do the integral
5. Only thing left is to calculate $uv -\int v\frac{du}{dx} dx$

-----
\* Slighlity more detailed

When doing the integration on $\textcolor{cyan}{v}$, it would normally produce a constant 
$$\textcolor{cyan}{\int \frac{dv}{dx}dx = v + C}$$

$$
\begin{align}
\int u\frac{dv}{dx} dx &= u \boxed{\int\frac{dv}{dx}dx} -\int \left(\boxed{\int \frac{dv}{dx}dx}\right) \frac{du}{dx} dx \\
&= u (v + C) -\int (v + C) \frac{du}{dx} dx \\
&= uv + uC -\int v\frac{du}{dx} dx - \int C\frac{du}{dx} dx \\
&= uv + \cancel{uC} -\int v\frac{du}{dx} dx - \cancel{uC} \\
&= uv  -\int v\frac{du}{dx} dx \\
\end{align}
$$
The integration constant eventually disappears. So when we do $v=\int \frac{dv}{dx}dx$ we can simply ignore the constant. Or since it's an arbitrary constant maybe we can simply set it to 0. See example below.



> [!Example]-
> $$\int e^{10x}xdx$$
> 
> Let's try $u = x$ and $\frac{dv}{dx}=e^{10x}$
> 
> $$
> \frac{du}{dx} = 1
> $$
> 
> $$v = \int \frac{dv}{dx} dx = \int e^{10x}dx = \frac{e^{10x}}{10} + C$$
> 
> 
> $$
> \begin{align}
> \textcolor{orange}{\int v\frac{du}{dx} dx} &= \int \left( \frac{e^{10x}}{10} + C\right) 1 dx \\
> &= \frac{e^{10x}}{100} + Cx + D
> \end{align}
> $$
> $$
> \begin{align}
> uv -\textcolor{orange}{\int v\frac{du}{dx} dx} &= x\left(\frac{e^{10x}}{10} + C\right) - \left(\frac{e^{10x}}{100} + Cx + D\right) \\
> &= x\frac{e^{10x}}{10} + \cancel{xC} - \frac{e^{10x}}{100} - \cancel{Cx} - D \\
> &= x\frac{e^{10x}}{10} - \frac{e^{10x}}{100} - D
> \end{align}
> $$


> [!Example] 
> $$\int \log x dx$$
> $u=\log x$ and $dv=1$
> $$\frac{du}{dx}=\frac 1 x$$
> $$v=\int \frac{dv}{dx}dx=x$$
> $$\textcolor{orange}{\int v\mathrm{d}u\mathrm{d}x}=\int x\frac 1 x \mathrm{d}x=x$$
> $$uv-\textcolor{orange}{\int v\mathrm{d}u\mathrm{d}x}=x\log x - x$$
