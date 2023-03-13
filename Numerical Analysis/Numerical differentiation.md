Given a function (or data) $f(t)$, the [[Calculus/Derivative|derivative]] on the right is given by
$$\frac {\mathrm{d}f}{\mathrm{d}t} = \lim_{\Delta t \to 0} \frac {f(t+\Delta t) - f(t)}{\Delta t}$$

> [!NOTE]- Derivative on the left and middle
> on the left by 
> $$\frac {\mathrm{d}f}{\mathrm{d}t} = \lim_{\Delta t \to 0} \frac {f(t) - f(t-\Delta t)}{\Delta t}$$
> from the middle
> $$\frac {\mathrm{d}f}{\mathrm{d}t} = \lim_{\Delta t \to 0} \frac {f(t+\Delta t) - f(t-\Delta t)}{2\Delta t}$$
 
But we are approximating with
$$
\frac {\mathrm{d}f}{\mathrm{d}t} \approx \frac {f(t+\Delta t) - f(t)}{\Delta t}
$$

> [!NOTE]- Approximation on the left and middle
> From the left
> $$
> \frac {\mathrm{d}f}{\mathrm{d}t} \approx \frac {f(t) - f(t-\Delta t)}{\Delta t}
> $$
> 
> From the middle
> $$
> \frac {\mathrm{d}f}{\mathrm{d}t} \approx \frac {f(t+\Delta t) - f(t-\Delta t)}{2\Delta t}
> $$

If we can calculate $\frac {\mathrm{d}f}{\mathrm{d}t}$, then we have an update rule for $f(t+\Delta t)$ as
$$f(t+\Delta t) \approx f(t) + \Delta t\frac {\mathrm{d}f}{\mathrm{d}t}$$

> [!NOTE]- Update rule on the left and middle
> Left
> $$f(t) \approx f(t-\Delta t) + \Delta t\frac {\mathrm{d}f}{\mathrm{d}t}$$
> 
> Middle
> $$f(t+\Delta t) \approx f(t-\Delta t) + 2\Delta t\frac {\mathrm{d}f}{\mathrm{d}t}$$

^458dc5

This is a terrible numerator integrator, it goes rapidly astray. The goal is to estimate how much mistake we are making with this approximation (or any other). 
- Does the error of the approximation scales with $\Delta t$?
- If I make my $\Delta t$ smaller, does my error get smaller? By how much?

## Understanding error with Taylor series

### Forward difference
Derivative from the right
$$
f(t+\Delta t) = f(t) + \Delta t\frac {\mathrm{d}f}{\mathrm{d}t} + \frac{\Delta t^2}{2!}\frac {\mathrm{d}^2f}{\mathrm{d}t^2}
+ \frac{\Delta t^3}{3!}\frac {\mathrm{d}^3f}{\mathrm{d}t^3}
+ \mathrm{O}(\Delta t^4)
$$

$$
\begin{align}
\textcolor{orange}{\frac {\mathrm{d}f}{\mathrm{d}t}} &\approx \frac {f(t+\Delta t) - f(t)}{\Delta t} \\
&= \frac{\left(f(t) + \Delta t\frac {\mathrm{d}f}{\mathrm{d}t} + \frac{\Delta t^2}{2!}\frac {\mathrm{d}^2f}{\mathrm{d}t^2}
+ \frac{\Delta t^3}{3!}\frac {\mathrm{d}^3f}{\mathrm{d}t^3}
+ \mathrm{O}(\Delta t^4)\right) - f(t)}{\Delta t} \\
&= \textcolor{orange}{\frac {\mathrm{d}f}{\mathrm{d}t}} +\underbrace{\textcolor{#B48EAD}{\frac{\Delta t}{2!}\frac {\mathrm{d}^2f}{\mathrm{d}t^2}}
+ \frac{\Delta t^2}{3!}\frac {\mathrm{d}^3f}{\mathrm{d}t^3}
+ \mathrm{O}(\Delta t^3)}_{\text{Error in the order of } \Delta t}
\end{align}
$$

Plugging in the Taylor expansion and cancelling all the terms, we are left with the desired <mark class="hltr-orange">value</mark> plus some terms that are the error of approximation. This error is on the order of its largest value $\Delta t$. $\frac{\Delta t}{2!}\frac {\mathrm{d}^2f}{\mathrm{d}t^2}$ is the <mark class="hltr-purple">leading order error term</mark>

So if we want a better approximation, we can reduce $\Delta t$. The error will be linear with $\Delta t$. 

### Backward difference
Derivative from the left
$$
f(t-\Delta t) = f(t) - \Delta t\frac {\mathrm{d}f}{\mathrm{d}t} + \frac{\Delta t^2}{2!}\frac {\mathrm{d}^2f}{\mathrm{d}t^2}
- \frac{\Delta t^3}{3!}\frac {\mathrm{d}^3f}{\mathrm{d}t^3}
+ \mathrm{O}(\Delta t^4)
$$
$$
\begin{align}
\textcolor{orange}{\frac {\mathrm{d}f}{\mathrm{d}t}} &\approx \frac {f(t)-f(t-\Delta t)}{\Delta t} \\
&= \frac{f(t)-\left(f(t) - \Delta t\frac {\mathrm{d}f}{\mathrm{d}t} + \frac{\Delta t^2}{2!}\frac {\mathrm{d}^2f}{\mathrm{d}t^2}
- \frac{\Delta t^3}{3!}\frac {\mathrm{d}^3f}{\mathrm{d}t^3}
+ \mathrm{O}(\Delta t^4)\right)}{\Delta t} \\
&= \textcolor{orange}{\frac {\mathrm{d}f}{\mathrm{d}t}} -\underbrace{\textcolor{#B48EAD}{\frac{\Delta t}{2!}\frac {\mathrm{d}^2f}{\mathrm{d}t^2}}
+ \frac{\Delta t^2}{3!}\frac {\mathrm{d}^3f}{\mathrm{d}t^3}
+ \mathrm{O}(\Delta t^3)}_{\text{Error in the order of } \Delta t}
\end{align}
$$
Same scheme as the forward difference. If we could cancel the term $\frac{\Delta t}{2!}\frac {\mathrm{d}^2f}{\mathrm{d}t^2}$ then the leading order error term would be $\mathcal{O}(\Delta t^2)$.

### Central difference

> [!summary]
> By numerically computing the derivative with the central difference scheme, we can approximate the derivative up to an error term of order $\mathcal{O}(\Delta t^2)$
> $$
> \begin{align}
> \frac {\mathrm{d}f}{\mathrm{d}t} \approx \frac {\mathrm{d}f}{\mathrm{d}t} + \mathcal{O}(\Delta t^2) 
> \end{align}
> $$

We use the central definition of the derivative and plug in the Taylor expansions

$$
\begin{align}
\frac {\mathrm{d}f}{\mathrm{d}t} &\approx \frac {f(t+\Delta t) - f(t-\Delta t)}{2\Delta t} \\
&= \frac{\left(f(t) + \Delta t\frac {\mathrm{d}f}{\mathrm{d}t} + \frac{\Delta t^2}{2!}\frac {\mathrm{d}^2f}{\mathrm{d}t^2}
+ \frac{\Delta t^3}{3!}\frac {\mathrm{d}^3f}{\mathrm{d}t^3} + \ldots\right)- \left(f(t) - \Delta t\frac {\mathrm{d}f}{\mathrm{d}t} + \frac{\Delta t^2}{2!}\frac {\mathrm{d}^2f}{\mathrm{d}t^2}
- \frac{\Delta t^3}{3!}\frac {\mathrm{d}^3f}{\mathrm{d}t^3} + \ldots\right)}{2\Delta t} \\
&= \frac{ 2\Delta t\frac {\mathrm{d}f}{\mathrm{d}t} + 
 2\frac{\Delta t^3}{3!}\frac {\mathrm{d}^3f}{\mathrm{d}t^3} + \mathcal{O}(\Delta t^5)}{2\Delta t} \\
&= \frac {\mathrm{d}f}{\mathrm{d}t} + 
 \textcolor{#B48EAD}{\frac{\Delta t^2}{3!}\frac {\mathrm{d}^3f}{\mathrm{d}t^3}} + \mathcal{O}(\Delta t^4) 
\end{align}
$$

The <mark class="hltr-purple">leading order error term</mark> is now in the order $\mathcal{O}(\Delta t^2)$. So if we reduce $\Delta t$ by 10 times, then the error is 100 times smaller.  


![[derivative.svg]]


> [!NOTE]- Why not always use the central difference
> The central difference gives a better approximation, so why not always use it? It depends on the use case. If $f(t+\Delta t)$ is not available because we have a real time system, then we cannot use the forward/central difference and we are stuck with backward.


# Second derivative

This sort of manipulation doesn't have to stop at the first derivative. The definition for the second derivative is

$$
\frac {\mathrm{d}^2f}{\mathrm{d}t^2} = \lim_{\Delta t \to 0} \frac {f'(t+\Delta t) - f'(t)}{\Delta t}
$$

but we can reuse the [[Numerical differentiation#Forward difference|forward difference]] and [[Numerical differentiation#Backward difference|backward difference]] creatively to get an approximation for this second derivative

**Forward**
$$
f(t+\Delta t) = f(t) + \Delta t\frac {\mathrm{d}f}{\mathrm{d}t} + \frac{\Delta t^2}{2!}\frac {\mathrm{d}^2f}{\mathrm{d}t^2}
+ \frac{\Delta t^3}{3!}\frac {\mathrm{d}^3f}{\mathrm{d}t^3}
+ \mathrm{O}(\Delta t^4)
$$
**Backward**
$$
f(t-\Delta t) = f(t) - \Delta t\frac {\mathrm{d}f}{\mathrm{d}t} + \frac{\Delta t^2}{2!}\frac {\mathrm{d}^2f}{\mathrm{d}t^2}
- \frac{\Delta t^3}{3!}\frac {\mathrm{d}^3f}{\mathrm{d}t^3}
+ \mathrm{O}(\Delta t^4)
$$

$$
\begin{align}
f(t+\Delta t)+f(t-\Delta t) &= \left(f(t) + \Delta t\frac {\mathrm{d}f}{\mathrm{d}t} + \frac{\Delta t^2}{2!}\frac {\mathrm{d}^2f}{\mathrm{d}t^2}
+ \frac{\Delta t^3}{3!}\frac {\mathrm{d}^3f}{\mathrm{d}t^3}
+ \mathrm{O}(\Delta t^4)\right) + \left(f(t) - \Delta t\frac {\mathrm{d}f}{\mathrm{d}t} + \frac{\Delta t^2}{2!}\frac {\mathrm{d}^2f}{\mathrm{d}t^2}
- \frac{\Delta t^3}{3!}\frac {\mathrm{d}^3f}{\mathrm{d}t^3}
+ \mathrm{O}(\Delta t^4)\right) \\
&= 2f(t) + 2\frac{\Delta t^2}{2!}\frac {\mathrm{d}^2f}{\mathrm{d}t^2}
+ \mathrm{O}(\Delta t^4) \\
\end{align}
$$

Every odd terms cancel out and from this we can isolate the second derivative

$$
\begin{align}
\frac {\mathrm{d}^2f}{\mathrm{d}t^2}
&= \frac{f(t+\Delta t)-2f(t)+f(t-\Delta t)}{\Delta t^2} - \mathrm{O}(\Delta t^4) \\
&\approx  \frac{f(t+\Delta t)-2f(t)+f(t-\Delta t)}{\Delta t^2} 
\end{align}
$$

ExportImage("filename", "variable_input.svg", "type", "svg", "transparent", true)