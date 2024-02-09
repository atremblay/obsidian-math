
> [!Summary] 
> A basic system that decays over follows $x(t)=x_0e^{\lambda t}$. The initial value $x_0$ decays over time. But impulse can happen continuously, kind of renewing the initial value, following a function $f(\tau)$. At time $t$ the value of $x(t)$ is the infinite sum of decaying inputs 
> $$x(t)=\int_{\tau=0}^{\tau=t} f(\tau)e^{\lambda (t-\tau)} d\tau$$


To model a very quick impulse to a system we make use of the [[../Calculus/Dirac Delta Function]] and the [[../Calculus/Mean Value Theorem/Mean value theorem#Integral| mean value theorem for the integral]]

$$
\begin{align}
\int_{-\infty}^{\infty} \delta(t)f(t)dt&=\int_{-\epsilon}^{\epsilon} \delta(t)f(t)dt \\
&=\int_{-\epsilon}^{\epsilon} \frac{1}{2\epsilon}f(t)dt \\
&=  \frac{1}{2\epsilon}\int_{-\epsilon}^{\epsilon} f(t)dt \\
&= \frac{1}{2\epsilon} f(c)(\epsilon - -\epsilon) \hspace{1em} \text{mean value} \\
&= \frac{2\epsilon}{2\epsilon} f(c) \\
&= f(c)
\end{align}
$$
Because $f(-\epsilon) \le f(c) \le f(\epsilon)$, as $\epsilon \rightarrow 0 \implies f(c) \rightarrow f(0)$ 
This, of course, can be shifted around. If we want the impulse at time $\tau$, then
$$\lim_{\epsilon \rightarrow 0}\int_{\tau - \epsilon}^{\tau + \epsilon} \delta (t-\tau)f(t)dt=f(\tau)\tag{1}$$

The Dirac delta function essentially select the value $f(\tau)$. 

# Continuous impulse

Of course we could simply take the value at $f(\tau)$ and not be bothered with the rest. Its power comes when there is a continuous input and we want to have the sum of all these inputs over a time span $[a,b]$ (e.g. $[0,t]$).

So if we fix $t$ and let $\tau$ vary, then 
$$\int_{\tau=0}^{\tau=t} \delta(t-\tau)f(\tau)d\tau \tag{2}$$
is the sum of all the inputs $f(\tau)$ between $0 \le \tau \le t$ summed up. 

> [!Important]
> Integration is over $\tau$ and not $t$ like in (1). We are not playing with the same knob. When you read this at a later point you will most likely be confused. 
> (1) integrates over $t$ to observe the behaviour of $d(t-\tau)f(t)$ at a specific point in time $\tau$. 
> (2) uses this behaviour to calculate a sum of inputs over time, not just one input.

# Continuous impulse with decay

Imagine that an impulse function $f(t)$ is provided. Take the impulse at time 0 (basically $f(0)=x_0$, the initial condition) and let it follow a differential equation with solution $x(t)=f(0)e^{\lambda t}$.

The impulse happens at time $\tau$ and we want to know the value at time $t$. The remaining time is $t-\tau$, we then need to calculate
$$f(\tau)e^{\lambda(t-\tau)}$$
![[../Images/variable_input.svg]]
Because there is a function of inputs, we can have the sum of all inputs at time $t$ with
$$\int_{\tau=0}^{\tau=t} f(\tau)e^{\lambda (t-\tau)} \delta(t-\tau) d\tau \tag{3}$$

(3) is basically a bunch of initial conditions at different points in time, all following the same system and all summed up at time $t$.

> [!Important] 
> This gives the **cummulative** impulse response. Because we multiply by $d\tau$, each impulse counts for very little in the end. That why plotting the integral would start at zero. See examples below.



> [!Example] Constant input
> $f(\tau)=2,\lambda=-1$
> $$
> \begin{align}
> \int_0^t f(\tau)\frac{e^{\lambda \tau}}{e^{\lambda t}}d\tau
> &= \frac{2}{e^{t}} \int_0^t e^{\tau}d\tau \\
> &= \frac{2}{e^{t}} (e^{\tau})_0^t \\
> &= \frac{2}{e^{t}} (e^t-1) \\
> &= 2(1-e^{-t}) \\
> \end{align}
> $$
> 
> ```tikz 
> \usepackage{pgfplots, tikz}
> \usetikzlibrary{positioning}
> \begin{document} 
>     \begin{tikzpicture}[scale=2.0] 
>         \draw[thin,color=gray] (0,0) grid (7.0,3.0); 
>         \draw[->] (0,0) -- (7.2,0) node[right] {$t$}; 
>         \draw[->] (0,0) -- (0,3.2) node[above] {$x(t)$}; 
>         \draw[color=green, domain=0:6, thick] 
>             plot[samples=100] (
>                 \x,
>                 {2 * (1 - exp(-\x) ) } 
>             ) 
>             node[above right] {$2(1-{e^{-t}})$};
>     \end{tikzpicture} 
> \end{document} 
> ```
>  $f(\tau)=2,\lambda=-0.5$
> ```tikz 
> \usepackage{pgfplots, tikz}
> \usetikzlibrary{positioning}
> \begin{document} 
>     \begin{tikzpicture}[scale=2.0] 
>         \draw[thin,color=gray] (0,0) grid (7.0,3.0); 
>         \draw[->] (0,0) -- (7.2,0) node[right] {$t$}; 
>         \draw[->] (0,0) -- (0,3.2) node[above] {$x(t)$}; 
>         \draw[color=green, domain=0:6, thick] 
>             plot[samples=100] (
>                 \x,
>                 {2/0.5 * (1 - exp(-0.5*\x) ) } 
>             ) 
>             node[above right] {$4(1-{e^{-0.5t}})$};
>     \end{tikzpicture} 
> \end{document} 
> ```
> $f(\tau)=3,\lambda=-1$
> $$
> \begin{align}
> \int_0^t f(\tau)\frac{e^{\lambda \tau}}{e^{\lambda t}}d\tau
> &= \frac{3}{e^{t}} \int_0^t e^{\tau}d\tau \\
> &= \frac{3}{e^{t}} (e^{\tau})_0^t \\
> &= \frac{3}{e^{t}} (e^t-1) \\
> &= 3(1-e^{-t}) \\
> \end{align}
> $$
> 
> 
> ```tikz 
> \usepackage{pgfplots, tikz}
> \usetikzlibrary{positioning}
> \begin{document} 
>     \begin{tikzpicture}[scale=2.0] 
>         \draw[thin,color=gray] (0,0) grid (7.0,3.0); 
>         \draw[->] (0,0) -- (7.2,0) node[right] {$t$}; 
>         \draw[->] (0,0) -- (0,3.2) node[above] {$x(t)$}; 
>         \draw[color=green, domain=0:6, thick] 
>             plot[samples=100] (
>                 \x,
>                 {3 * (1 - exp(-\x) ) } 
>             ) 
>             node[above right] {$3(1-{e^{-t}})$};
>     \end{tikzpicture} 
> \end{document} 
> ```

> [!Example] Constant input over an interval followed by a decay
> $\lambda=-1,f(\tau)=\begin{cases}
>     k & \text{if } \tau \in [0,a]\\
>     0 & \text{if } \tau \in [a,t]
>     \end{cases}
> $
> $$
> \begin{align}
> \int_0^t f(\tau)e^{\lambda (t-\tau)}d\tau &= 
>     \begin{cases}
>         \int_0^a ke^{\lambda (a-\tau)}d\tau \\
>         e^{\lambda (t-a)}\left(\int_0^a f(\tau)e^{\lambda (a-\tau)}d\tau\right) 
>     \end{cases} \\
> &= \begin{cases}
>         -\frac k \lambda \left(e^{\lambda (a-\tau)}\right)_0^a \\
>         e^{\lambda (t-a)}\left(-\frac k \lambda \left(e^{\lambda (a-\tau)}\right)_0^a\right) 
>     \end{cases} \\
> &= \begin{cases}
>         -\frac k \lambda \left(1-e^{\lambda a}\right) \\
>         e^{\lambda (t-a)}\left(-\frac k \lambda \left(1-e^{\lambda a}\right) \right) 
>     \end{cases} \\
> &= \begin{cases}
>         -\frac k \lambda \left(1-e^{\lambda a}\right) \\
>         -\frac k \lambda \left(e^{\lambda (t-a)}-e^{\lambda t}\right) 
>     \end{cases} \\
> \end{align}
> $$
> Setting some values, $\lambda = -1, k=2, a=3$
> $$
> \begin{cases}
>         2 \left(1-e^{-t}\right) & \text{if } t \in [0,3]\\
>         2 \left(e^{3-t}-e^{-t}\right) & \text{if } t \in [3,t]
>     \end{cases} \\
> $$
> 
> ```tikz 
> \usepackage{pgfplots, tikz}
> \usetikzlibrary{positioning}
> \begin{document} 
>     \begin{tikzpicture}[scale=2.0] 
>         \draw[thin,color=gray] (0,0) grid (7.0,3.0); 
>         \draw[->] (0,0) -- (7.2,0) node[right] {$t$}; 
>         \draw[->] (0,0) -- (0,3.2) node[above] {$x(t)$}; 
>         \draw[color=green, domain=0:3, thick] 
>             plot[samples=100] 
>                 (
>                     \x, 
>                     {2 * (1 - exp(-\x) ) } 
>                 ) 
>             node[above right] {$2(1-{e^{-t}})$};
>         \draw[color=green, domain=3:6, thick] 
>             plot[samples=100] 
>                 (
>                     \x, 
>                     {2 * (exp(3-\x)  - exp(-\x))} 
>                 ) 
>             node[above right] {$2(e^{3-t}-{e^{-t}})$};
>     \end{tikzpicture} 
> \end{document} 
> ```
> $\lambda = -0.5, k=2, a=3$
> $$
> \begin{cases}
>         \frac 2 {0.5} \left(1-e^{-0.5t}\right) & \text{if } t \in [0,3]\\
>         \frac 2 {0.5} \left(e^{0.5(3-t)}-e^{-0.5t}\right) & \text{if } t \in [3,t]
>     \end{cases} \\
> $$
>
> ```tikz 
> \usepackage{pgfplots, tikz}
> \usetikzlibrary{positioning}
> \begin{document} 
>     \begin{tikzpicture}[scale=2.0] 
>         \draw[thin,color=gray] (0,0) grid (7.0,4.0); 
>         \draw[->] (0,0) -- (7.2,0) node[right] {$t$}; 
>         \draw[->] (0,0) -- (0,4.2) node[above] {$x(t)$}; 
>         \draw[color=green, domain=0:3, thick] 
>             plot[samples=100] 
>                 (
>                     \x, 
>                     {4 * (1 - exp(-0.5*\x) ) } 
>                 ) 
>             node[above right] {$4(1-{e^{-0.5t}})$};
>         \draw[color=green, domain=3:6, thick] 
>             plot[samples=100] 
>                 (
>                     \x, 
>                     {4 * (exp(0.5*(3-\x))  - exp(-0.5*\x))} 
>                 ) 
>             node[above right] {$4(e^{0.5(3-t)}-{e^{-0.5t}})$};
>     \end{tikzpicture} 
> \end{document} 
> ```
> $\lambda = -2, k=2, a=3$
> $$
> \begin{cases}
>         \frac 2 {2} \left(1-e^{-2t}\right) & \text{if } t \in [0,3]\\
>         \frac 2 {2} \left(e^{2(3-t)}-e^{-2t}\right) & \text{if } t \in [3,t]
>     \end{cases} \\
> $$
>
> ```tikz 
> \usepackage{pgfplots, tikz}
> \usetikzlibrary{positioning}
> \begin{document} 
>     \begin{tikzpicture}[scale=2.0] 
>         \draw[thin,color=gray] (0,0) grid (7.0,3.0); 
>         \draw[->] (0,0) -- (7.2,0) node[right] {$t$}; 
>         \draw[->] (0,0) -- (0,3.2) node[above] {$x(t)$}; 
>         \draw[color=green, domain=0:3, thick] 
>             plot[samples=100] 
>                 (
>                     \x, 
>                     { 1 - exp(-2*\x) ) } 
>                 ) 
>             node[above right] {$1-{e^{-2t}}$};
>         \draw[color=green, domain=3:6, thick] 
>             plot[samples=100] 
>                 (
>                     \x, 
>                     {(exp(2*(3-\x))  - exp(-2*\x))} 
>                 ) 
>             node[above right] {$e^{2(3-t)}-{e^{-2t}}$};
>     \end{tikzpicture} 
> \end{document} 
> ```


> [!Example] Input as step function
> Very similar to a previous example with the difference that the input does not stop at $t=a$ but changes from $k$ to $m$. This is a more general formula than the one above since we simple have to set $m=0$ to have the special case.
>
> The second term of the second case is the first signal decaying until $t$. The first term of the second case is the second signal accumulating until $t$. The sum of both is the sum of two systems, giving the orange curve below.
>  
> $\lambda=-1,f(\tau)=\begin{cases}
>     k & \text{if } \tau \in [0,a]\\
>     m & \text{if } \tau \in [a,t]
>     \end{cases}
> $
> $$
> \begin{align}
> \int_0^t f(\tau)e^{\lambda (t-\tau)}d\tau &= 
>     \begin{cases}
>         \int_0^a f(\tau)e^{\lambda (a-\tau)}d\tau \\
>         \left(
>             \int_{a}^t f(\tau)e^{\lambda (t-\tau)}d\tau\right)
>             +e^{\lambda (t-a)}\left(\int_0^a f(\tau)e^{\lambda (a-\tau)}d\tau
>         \right)  
>     \end{cases} \\
>  &= \begin{cases}
>         -\frac k \lambda \left(1-e^{\lambda a}\right) \\
>         -\frac m \lambda  \left(1-e^{\lambda (t-a)}\right)  -\frac k \lambda \left(e^{\lambda (t-a)}-e^{\lambda t}\right) 
>     \end{cases} \\
> \end{align}
> $$
> Similar to one of the example above, except that at time $t=3$ the input shifts to 3 instead of decaying.
> $\lambda=-1,f(\tau)=\begin{cases}
>     2 & \text{if } \tau \in [0,3]\\
>     3 & \text{if } \tau \in [3,t]
>     \end{cases}
> $
> ```tikz 
> \usepackage{pgfplots, tikz}
> \usetikzlibrary{positioning}
> \begin{document} 
>     \begin{tikzpicture}[scale=2.0] 
>         \draw[thin,color=gray] (0,0) grid (7.0,4.0); 
>         \draw[->] (0,0) -- (7.2,0) node[right] {$t$}; 
>         \draw[->] (0,0) -- (0,4.2) node[above] {$x(t)$}; 
>         \draw[color=green, domain=0:3, thick] 
>             plot[samples=100] 
>                 (
>                     \x, 
>                     {2*(1-exp(-\x) ) } 
>                 ) 
>             node[below ] {$2(1-{e^{t}})$};
>         \draw[color=orange, domain=3:6, thick] 
>             plot[samples=100] 
>                 (
>                     \x,
>                     {3 * (1-exp(3-\x)) + 2 * (exp(3-\x)  - exp(-\x))}
>                 )
>             node[above ] {$3(1-{e^{3-t}})+2(e^{3-t}-e^{-x}))$};
>     \end{tikzpicture} 
> \end{document} 
> ```


