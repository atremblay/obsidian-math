
> [!Summary] 
> This page is a multivariate version of [[Impulse]]
> $$
> \mathbf{\dot{x}} = \mathbf{Ax} + \mathbf{Bu} \implies \mathbf{x}(t)=e^{\mathbf{A}t}\mathbf{x}(0) + \int_{\tau=0}^T e^{\mathbf{A}(t-\tau)}\mathbf{Bu}(\tau)d\tau
> $$

Start from [[Impulse]], that covers the case of a single dimension and single equation. This page generalizes that concepts to a system of equations.

As seen in [[System of ODE]], a [[System of ODE#Linear ODE Homogeneous Homogeneous ODE as Coupled System|homogeneous system]] is expressed as
$$\mathbf{\dot{x}}=\mathbf{Ax}\tag{1}$$
with the solution being 
$$\mathbf{x}(t)=e^{\mathbf{A}t}x(0) \tag{2}$$

While a [[System of ODE#Linear ODE Nonhomogeneous Nonhomogeneous ODE as Coupled system|nonhomogeneous system]] was expressed as
$$\mathbf{\dot{x}} = \mathbf{Ax} + \mathbf{Q}$$
where $\mathbf{Q}$ is simply a vector, we can generalize this concept a little bit more with
$$
\mathbf{\dot{x}} = \mathbf{Ax} + \mathbf{Bu}\tag{3}
$$
As long as $\mathbf{Bu}$ yields a vector, it works as an input/forcing vector. 
- If $\mathbf{B}$ is a matrix, then $\mathbf{u}$ is a vector. 
- If $\mathbf{B}$ is a vector, then $\mathbf{u}$ is a scalar. 

# Decompose $(3)$

## No input
Obviously if the input/forcing vector is zero then we have the normal base case $(1)$.

```tikz 
\usepackage{pgfplots, tikz}
\usetikzlibrary{positioning}
\begin{document} 
    \begin{tikzpicture}[scale=2.0] 
        \draw[thin,color=gray] (0,0) grid (7.0,3.0); 
        \draw[->] (0,0) -- (7.2,0) node[right] {$t$}; 
        \draw[->] (0,0) -- (0,3.2) node[above] {$x(t)$}; 
        \draw[color=orange, domain=0:6, thick] plot (\x,{3*exp(-\x)}) node[above right] {$x(t) = 3\mathrm e^t$}; 

        \draw[color=orange] (0,3)  node[left] {$x_0$};
    \end{tikzpicture} 
\end{document} 
```

## No initial condition, one single input
Suppose that the initial condition is zero in $(1)$, then nothing happens forever and ever because things never gets off the ground in the solution $(2)$.

But with a system that has forcing, or input, then the state of the system can change, be it at time zero or at a later time. If I have a [[Mecanical vibration#Higher order DE|mass on a spring]] that is not initially stretched or compressed (no initial condition) and I whack it with a hammer at some other time, then the system will start to move.

To understand the input dynamics, it's better to look at an input at a specific time. To model this we need the [[Dirac Delta Function]] and set 
$$\mathbf{u}(\tau) = \delta(t-\tau)$$
and set $\mathbf{B}$ to some sort of initial conditions that we will call $\mathbf{b}_0$ (basically the same thing as $\mathbf{x}_0$). The [[Dirac Delta Function]] acts as a switch. At that specific point in time, an input is given to the system, very rapidly. 

Now, because there is an input to the system at some time $\tau$, $\mathbf{\dot{x}}(t)$ is no longer zero and it follows the dynamics of $\mathbf{Ax}$. In this diagram $x$ is represented as a scalar, but it's a vector.

```tikz 
\usepackage{pgfplots, tikz}
\usetikzlibrary{positioning}
\begin{document} 
    \begin{tikzpicture}[scale=2.0] 
        \draw[thin,color=gray] (0,0) grid (7.0,3.0); 
        \draw[->] (0,0) -- (7.2,0) node[right] {$t$}; 
        \draw[->] (0,0) -- (0,3.2) node[above] {$x(t)$}; 
        \draw[color=green, domain=2:6, thick] plot (\x,{1.5*exp(-(\x-2))}) node[above right] {$x(t) = 3\mathrm e^{-t}$};
        \draw[color=green] (2,0)  node[below] {$\tau$};
        \draw[color=green, dashed, thick]  (2,0) -- (2,1.5) node[midway, left] {$b_0$}; 
    \end{tikzpicture} 
\end{document} 
```

The dynamics of the system then takes over. Everything is shifted by $\tau$. The initial decay starts at $\tau$, nothing happens before that.

## Initial condition, one single input

Combine the two previous section where we have an initial response plus and input. We claim this superposition based on [[Superposition]], but it does not fully compute in my head.

> Drawing the input response in green to show what the superposition looks like (magenta).

```tikz 
\usepackage{pgfplots, tikz}
\usetikzlibrary{positioning,arrows}
\begin{document} 
    \begin{tikzpicture}[scale=2.0] 
        \draw[thin,color=gray] (0,0) grid (7.0,3.0); 
        \draw[->] (0,0) -- (7.2,0) node[right] {$t$}; 
        \draw[->] (0,0) -- (0,3.2) node[above] {$x(t)$}; 
        \draw[color=orange, domain=0:2, thick] 
            plot (\x,{3*exp(-\x)}) 
            node[right] {$x(t) = 3\mathrm e^{-t}$}; 
        \draw[color=magenta, domain=2:6, thick] 
            plot (\x,{3*exp(-\x) + 2*exp(-(\x-2))}) 
            node[color=magenta, above right] {
                $x(t) = 3\mathrm e^t + 2\mathrm e^{(t-\tau)}$
            };
        \draw[color=green, opacity=0.5, domain=2:6, thick] 
            plot (\x,{1.5*exp(-(\x-2))});
        
        \draw[color=orange] (0,3) node[left] {$x_0$};
        \draw[color=magenta] (2,0) node[below] {$\tau$};
        \draw[color=green, dashed,thick]  
            (2,0.406) -- (2,2.4) 
            node[midway, left] {$b_0$}; 
    \end{tikzpicture} 
\end{document} 
```

## Initial condition, a few inputs

It's just the same as the previous section, but with more inputs over time. It bumps up the whole system.

```tikz 
\usepackage{pgfplots, tikz}
\usetikzlibrary{positioning}
\begin{document} 
    \begin{tikzpicture}[scale=2.0] 
        \draw[thin,color=gray] (0,0) grid (7.0,3.0); 
        \draw[->] (0,0) -- (7.2,0) node[right] {$t$}; 
        \draw[->] (0,0) -- (0,3.2) node[above] {$x(t)$}; 
        \draw[color=orange, domain=0:2, thick] plot (\x,{3*exp(-\x)}) node[right] {$x(t) = 3\mathrm e^t$}; 
        \draw[color=magenta, domain=2:3, thick] plot (\x,{3*exp(-\x) + 1.5*exp(-(\x-2))});

        \draw[color=magenta, domain=3:5, thick] plot (\x,{3*exp(-\x) + 1.5*exp(-(\x-2))+ 1.5*exp(-(\x-3))});

        \draw[color=magenta, domain=5:7, thick] plot (\x,{3*exp(-\x) + 1.5*exp(-(\x-2))+ 1.5*exp(-(\x-3))+ 2*exp(-(\x-5))});
        
        \draw[color=magenta, dashed,thick]  (2,0.406) -- (2,1.906) node[midway, left] {$1.5$}; 
        \draw[color=magenta, dashed,thick]  (3,0.701) -- (3,2.2) node[midway, left] {$1.5$}; 
        \draw[color=magenta, dashed,thick]  (5,0.298) -- (5,2.298) node[midway, left] {$2$}; 
        \draw[color=orange] (0,3)  node[left] {$x_0$};
    \end{tikzpicture} 
\end{document} 
```

## Initial condition, continuous input

```tikz 
\usepackage{pgfplots, tikz}
\usetikzlibrary{positioning}
\begin{document} 
    \begin{tikzpicture}[scale=2.0] 
        \draw[thin,color=gray] (0,0) grid (7.0,3.0); 
        \draw[->] (0,0) -- (7.2,0) node[right] {$t$}; 
        \draw[->] (0,0) -- (0,3.2) node[above] {$x(t)$}; 
        \draw[color=green, domain=0:7, ultra thick] 
            plot[samples = 100] (\x,
                { %coefficients were obtained by linear regression
                % of 4th order over this array
                % [1,2,3,2,1,3,2,3]
                    2.54175685*(\x+1)
                    -1.31912879*(\x+1)^2
                    +0.24179293*(\x+1)^3
                    -0.01420455*(\x+1)^4
                }
            )
            node[right] {$u$}; 
    \end{tikzpicture} 
\end{document} 
```

```tikz 
\usepackage{pgfplots, tikz}
\usetikzlibrary{positioning}
\begin{document} 
    \begin{tikzpicture}[scale=2.0] 
        \draw[thin,color=gray] (0,0) grid (7.0,4.0); 
        \draw[->] (0,0) -- (7.2,0) node[right] {$t$}; 
        \draw[->] (0,0) -- (0,4.2) node[above] {$x(t)$}; 
        \draw[color=cyan, domain=0:7, ultra thick] 
            plot[samples = 100] (\x,
                {exp(-3*\x)*1.5+ 2/3*(1-exp(-3*(\x)))}
            )
            node[above] {$3e^{-3t}+\frac 2 3$}; 
        \draw[color=orange, domain=0:7, ultra thick] 
            plot[samples = 100] (\x,
                {exp(-3*\x)*1.5}
            )
            node[above] {$2e^{-3t}$}; 
        \draw[color=green, domain=0:7, ultra thick] 
            plot[samples = 100] (\x,
                {2}
            )
            node[above] {$b=2$}; 
    \end{tikzpicture} 
\end{document} 
```


![[forcing3.svg]]


# Ressources
[Steve Brunton](https://www.youtube.com/watch?v=q1phVuNCuf0&list=PLMrJAkhIeNNTYaOnVI3QpH7jgULnAmvPA&index=33)
