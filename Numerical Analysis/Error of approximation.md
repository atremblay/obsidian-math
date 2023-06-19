Numerical approximations are just that, approximations. The [[Numerical differentiation|numerical differentiation]] rely on $\Delta t$ and we get better results as it get smaller. Can we make $\Delta t$ arbitrarily small?

No.

Using the [[Numerical differentiation#Central difference|central difference]] scheme to calculate the derivative we get something with an leading error term in the order of $\mathcal{O}(\Delta t^2)$. 

$$
\begin{align}
\frac {\mathrm{d}f}{\mathrm{d}t} &\approx \frac {f(t+\Delta t) - f(t-\Delta t)}{2\Delta t} + \mathcal{O}(\Delta t^2) \\
&=\frac {f(t+\Delta t) - f(t-\Delta t)}{2\Delta t} 
+ \frac{\Delta t^2}{3!}\frac {\mathrm{d}^3f}{\mathrm{d}t^3}
+ \mathrm{O}(\Delta t^4)
\end{align}
$$

Computers will have round-off errors, they can't have infinite precision. If $f(t)$ requires more precision than the computer can offer, then we will have round-off errors $e_r$.
$$
\begin{align}
\frac {\mathrm{d}f}{\mathrm{d}t} &\approx \frac {f(t+\Delta t) - f(t-\Delta t)+2e_r}{2\Delta t} + \mathcal{O}(\Delta t^2) \\
&= \frac {f(t+\Delta t) - f(t-\Delta t)}{2\Delta t} + \underbrace{\frac{e_r}{\Delta t}}_{\text{Roundoff error}} + \underbrace{\mathcal{O}(\Delta t^2)}_{\text{From Taylor Series}} \\
&=\frac {f(t+\Delta t) - f(t-\Delta t)}{2\Delta t} +\frac{e_r}{\Delta t}
+ \underbrace{\frac{\Delta t^2}{3!}\frac {\mathrm{d}^3f}{\mathrm{d}t^3}
+ \mathrm{O}(\Delta t^4)}_{\text{Expand a little bit}}
\end{align}
$$

The crux of this is that $e_r$ is divided by $\Delta t$ and that is smaller and smaller. So the round-off error gets bigger and bigger. There will be a tradeoff where a smaller $\Delta t$ will make $\textcolor{#1A5CA7}{\frac{e_r}{\Delta t}} \gt \textcolor{#5B840D}{\mathcal{O}(\Delta t^2)}$ and a bigger $\Delta t$ will make $\textcolor{#1A5CA7}{\frac{e_r}{\Delta t}} \lt \textcolor{#5B840D}{\mathcal{O}(\Delta t^2)}$.

![[../Images/approximation_error.svg]]


The total error of the approximation has an upper bound of
$$
|\text{Error}| \le \textcolor{orange}{\frac{e_r}{\Delta t} + \frac{m\Delta t^2}{3!}}
$$
where $m$ is the max of $\frac {\mathrm{d}^3f}{\mathrm{d}t^3}$ on the interval $[t-\Delta t, t+\Delta t]$. Take the largest possible value for the upper bound. That gives us a function in terms of $\Delta t$ for the maximum error

> [!Question]-
> Not sure what happens to $\mathcal{O}(\Delta t^4)$. Can we say that by taking the maximum of the third derivative "absorbs" the remaining error? Or maybe because we will eventually drop terms to do an approximation then we can simply drop it?

$$E_{max}(\Delta t) = \frac{e_r}{\Delta t} + \frac{m\Delta t^2}{6}$$
To get the minimum value of that function, you guessed it, take the derivative with respect to $\Delta t$ and set it to zero. We will use a computer precision of $10^{-16}$.
$$
\begin{align}
\frac{\mathrm d E_{max}(\Delta t)}{\mathrm{d}\Delta t}=-\frac{e_r}{\Delta t^2} + \frac{m\Delta t}{3}&=0 \\
\iff \Delta t^3 &= \frac{3e_r } m \\
\iff \Delta t &= \sqrt[3] \frac{3e_r } m \approx \sqrt[3] {10^{-16}} \approx 10^{-5}
\end{align}
$$

The lowest error we can get, due to round-off error, is achieved when $\Delta t \approx 10^{-5}$.


