> [!Summary]
> $$\dot{x}=kx+b$$
> $b$ is a constant input

# Use [[Undetermined Coefficient]]

## Homogeneous version

$$\dot{x}=kx$$

Use [[Separation of variables]]

$$
\begin{align}
\dot{x}&=kx \\
\frac {1}{kx} \frac {dx}{dt} &= 1 \\
\int \frac {1}{kx} \frac {dx}{dt} dt &= \int dt \\
\int \frac {1}{kx} dx &= \int dt
\end{align}
$$
> [!note]- Change of variable
> Bit more details for future reference, when you feel dumb. Use a [[Substitution rule|change of variable]] $u=kx$ and $du=kdx\iff \frac{du}{k}=dx$
>  $$
>  \begin{align}
>  \int \frac {1}{kx} dx &= \frac 1 k \int \frac 1 u du \\
>  &= \frac 1 k \log u + c \\
>  &= \frac 1 k \log kx
>  \end{align}
>  $$

$$
\begin{align}
\frac {1}{k} \log{kx} &= t+c \\
x &= \frac 1 k e^{k(t+c)} \\
x &= \frac 1 k e^{kt}e^{kc} \\
x_h &= Ce^{kt} \\
\end{align}
$$


## Nonhomogeneous version


$$\dot{x}=kx+b$$

Use [[Separation of variables]]

$$
\begin{align}
\dot{x}&=kx + b \\
\frac {1}{kx+b} \frac {dx}{dt} &= 1 \\
\int \frac {1}{kx+b} \frac {dx}{dt} dt &= \int dt \\
\int \frac {1}{kx+b} dx &= \int dt
\end{align}
$$
> [!note]- Change of variable
> Bit more details for future reference, when you feel dumb. Use a [[Substitution rule|change of variable]] $u=kx+b$ and $du=kdx\iff \frac{du}{k}=dx$
>  $$
>  \begin{align}
>  \int \frac {1}{kx+b} dx &= \frac 1 k \int \frac 1 u du \\
>  &= \frac 1 k \log u + c \\
>  &= \frac 1 k \log (kx+b)
>  \end{align}
>  $$

$$
\begin{align}
\frac {1}{k} \log{kx+b} &= t+c \\
x &= \frac {e^{k(t+c)} - b} k  \\
x &= \frac {e^{kt}e^{kc} - b} k  \\
x &= \frac {e^{kt}e^{kc}} k - \frac b k  \\
x_p &= Ce^{kt} - \frac b k  \\
\end{align}
$$

## Combine

$$
\begin{align}
x&=cx_h+x_p  \\
&= cC_1e^{kt} + C_2e^{kt} - \frac b k \\
&= Ce^{kt} - \frac b k
\end{align}$$

If we have an initial condition $x(0) = x_0$

$$
\begin{align}
x(0) = x_0 &= Ce^{0k} - \frac b k \\
&= C - \frac b k \\
\iff C &= x_0 + \frac b k \\
\implies x(t) &= \left(x_0 + \frac b k\right)e^{kt} - \frac b k \\
&= \boxed{x_0e^{kt} + \frac b ke^{kt} - \frac b k}
\end{align}$$


# Linear DE with forcing

Adapting for the simpler case of a 1D system. 
$$
x(t)=x_0e^{kt} + \int_{\tau=0}^t e^{k(t-\tau)}bd\tau
$$
Complete the integral
$$
\begin{align}
\int_{\tau=0}^t e^{k(t-\tau)}bd\tau &= be^{kt} \int_0^t e^{-k\tau}d\tau \\
&=\frac {-be^{kt}} {k}\left(e^{-k\tau} \biggm\lvert _0^t\right) \\
&=\frac {-be^{kt}} {k} \left(e^{-kt} -1\right) \\
&=\frac {b} {k} e^{kt} -\frac {b} {k} \\
\end{align}
$$

Putting it back in the solution

$$
\begin{align}
x(t)&=x_0e^{kt} + \int_{\tau=0}^t e^{k(t-\tau)}bd\tau \\
&=\boxed{x_0e^{kt} + \frac {b} {k} e^{kt} -\frac {b} {k}} 
\end{align}
$$

# Plot 

Plot with $x_0=1.5$, $b=2$ and $k=-3$
Orange: Without forcing
Green : Forcing (input)
Cyan: With Forcing

```tikz 
\usepackage{pgfplots, tikz}
\usetikzlibrary{positioning}
\begin{document} 
    \begin{tikzpicture}[scale=2.0] 
        \draw[thin,color=gray] (0,0) grid (7.0,2.0); 
        \draw[->] (0,0) -- (7.2,0) node[right] {$t$}; 
        \draw[->] (0,0) -- (0,2.2) node[above] {$x(t)$}; 
        \draw[color=cyan, domain=0:7, ultra thick] 
            plot[samples = 100] (\x,
                {exp(-3*\x)*1.5 + 2/3*(1-exp(-3*\x))}
            )
            node[above] {$1.5e^{-3t}+\frac 2 3(1-e^{-3t})$}; 
        \draw[color=orange, domain=0:7, ultra thick] 
            plot[samples = 100] (\x,
                {exp(-3*\x)*1.5}
            )
            node[above] {$1.5e^{-3t}$}; 
        \draw[color=green, domain=0:7, ultra thick] 
            plot[samples = 100] (\x,
                {2/3*(1-exp(-3*\x))}
            )
            node[right] {$b=2$}; 
    \end{tikzpicture} 
\end{document} 
```


> [!Question] 
> I would have expected the cyan line to start at 3.5 ($x_0+b=1.5+2$) at $t=0$. Why does it start at the exact same point? The math says that at $t=0$ two terms cancel, leaving only the initial condition, but it does not fit my intuition.
> Because the constant input is not $b=2$, that is just a by product of the  input. The actual input is $\frac 2 3 (1-e^{-3t})$ and it's not constant, although it pretty much is when $t$ gets large. 
