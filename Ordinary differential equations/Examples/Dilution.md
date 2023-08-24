[[../../Reading notes/@tenenbaum_ordinary_1985]] p.124

##### Example ^354736
#example #homogeneous
> [!Example] Statement
> A tank contains 100 gallons of water. In error, 300 pounds of salt are poured into the tank instead of 200 pounds. To correct this condition, a stopper is removed from the bottom of the tank allowing 3 gallons of the brine to flow out each minute. At the same time, 3 gallons of fresh water per minute are pumped into the tank. If the mixture is kept uniform by constant stirring, how long will it take for the brine to contain the desired amount of salt.

The most difficult part is setting up the problem. First we need to figure out what is the quantity to track. As stated in the question, $x$ would be the amount of salt, so $x(t)$ is the amout of salt at time $t$.

To warm up your brain, remember that calculus deals in approximation and when things to super small then it becomes less of an approximation. With that in mind, how much salt will come out during $\Delta t$ minutes if 3 gallons go out in 1 minute?
- $3\Delta t$ gallons of water come out.
- There is $\frac{x(t)}{100}$ pounds per gallons
- That's approximately (operative word being approximately) $-3\Delta t \frac{x(t)}{100}$ pounds of salt coming out of the tank

$$\Delta x = \frac{3x}{100}\Delta t$$
Taking the limit $$\lim_{\Delta t \rightarrow 0}\frac{\Delta x}{\Delta t} = \frac{\mathrm{d}x}{\mathrm{d}t} = \frac{-3x}{100}$$
Using the most common technique [[../Solutions/Separation of variables]]
$$x(t) = 300e^{-0.03t}$$

The problem is not yet solved. We want to know **when** the tank will have 200 pounds of salt
$$
\begin{align}
200 = 300e^{-0.03t} \\
\log\left(\frac 2 3\right) = -0.03t \\
\boxed{t = \frac{-\log\left(\frac 2 3\right)}{0.03} = 13.5\text{ minute}} \\
\end{align}
$$

> [!Note]-
> The fact that the concentration is kept uniform is immensly helpful. We don't have to worry about distribution of salt near the bottom of the tank.


```tikz 
\usepackage{pgfplots}
\pgfplotsset{compat=1.13,width=15cm}
\begin{document} 
    \begin{tikzpicture}
    \begin{axis}[axis lines=middle,xlabel=$t$,ylabel=$x(t)$,enlargelimits]
        \addplot[domain=0:50, thick] {300*exp(-0.03*x)};
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (0,200) (13.5,200)};
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (13.5,0) (13.5,200)};
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```


##### Example ^e9c5ad
#example #constant_input #non_homogeneous 
> [!Example] Statement
> A tank contains 100 gallons of brine whose salt concentration is 3 pounds per gallon. Three gallons of brine whose salt concentration is 2 pounds per gallon flow into the tank each minute, and at the same time 3 gallons of mixture flow out each minute. If the mixture is kept uniform by constant stirring, find the salt content of the brine as a function of the time $t$.

You could be tempted to measure the concentration in the tank instead of the amount of salt in total. To do that, you would still need to have the amount of salt at time $t$ divided by 100 gallons. That comes back to tracking the amount of salt in total.
This requires two parts:
- One to measure the amount of salt going out; the first part is like the previous example.
- One to measure the amount of salt coming in.

The amount of salt coming in is constant over time, simply 2 pounds of salt per gallon. So if 3 gallons are coming in per minutes, $6\Delta t$ pounds are coming in in $\Delta t$ minutes.
$$\Delta x = \frac{-3x}{100}\Delta t + 6\Delta t$$
Again using [[../Solutions/Separation of variables]]
$$\boxed{x(t) = 100e^{-0.03t}+200}$$

> [!Note]-
> More details. As usual, the constant $C$ absorbs everything.
> $$
> \begin{align}
>     \frac{1}{-0.03x + 6}\frac{dx}{dt} &= 1 \\
>     \int \frac{1}{-0.03x + 6}\frac{dx}{dt}dt &= \int dt \\
>     \frac{-100}{3}\log\left(-0.03x + 6\right)&= t + C\\
>     \log\left(-0.03x + 6\right)&= -0.03t + C\\
>     -0.03x + 6&= e^{-0.03t + C}\\
>     x&= \frac{100}{3}(Ce^{-0.03t} + 6)\\
>     x&= Ce^{-0.03t} + 200\\
>     x&= 100e^{-0.03t} + 200\\
> \end{align}
> $$
> With the initial condition that $x(0) = 300$ we find that $C=100$.

> [!Tip]
> This is a [[../Definitions/Linear ODE#Nonhomogeneous|nonhomogeneous equation]]
> $$\dot{x} = -0.03x + 6$$
> We have a constant forcing over time of 6. This is a direct application of [[Constant input#Linear DE with forcing]]
> $$x(t)=x_0e^{kt} + \frac {b} {k} e^{kt} -\frac {b} {k}$$
> With $k=-0.03$, $b=6$ and $x(0)=300$. Filling in those values we find the exact same solution
> $$x(t)= 100e^{-0.03t} + 200$$

```tikz 
\usepackage{pgfplots}
\pgfplotsset{compat=1.13,width=15cm}
\begin{document} 
    \begin{tikzpicture}
    \begin{axis}[
        axis lines=middle,
        xlabel=$t$,
        ylabel=$x(t)$,
        enlargelimits
    ]
        \addplot[domain=0:200, thick] {exp(-0.03*x)*100 + 200};
        \addplot[dashed] plot coordinates {(0,200) (200,200)};
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```


##### Example ^1b9c9c
#example 
> [!Example] Statement
> A tank contains 100 gallons of brine whose salt concentration is 3 pounds per gallon. Three gallons of fresh water flow into the tank each minute, and at the same time 5 gallons of mixture flow out each minute. If the mixture is kept uniform by constant stirring, find the salt content of the brine as a function of the time $t$.

Very similar to the [[#^354736|first example]], but the overall quantity in the tank goes down over time. Again, track $x(t)$ as the amount of salt at time time $t$

This time, we cannot calculate the concentration per gallon by considering a constant amount of 100 gallons as it lowers over time. Every minute the tank loses 5 gallons and gains 3, so a delta of 2 gallons. Over a period of $t$ that's $2t$ gallons lost. So the amount of salt per gallon at time $t$ is $\frac{x(t)}{100-2t}$

This gives a more interesting problem because it's one of those rare times where we have something on the right of the [[../Solutions/Separation of variables]]
$$\Delta x = \frac{-5 x \Delta t}{100-2t} $$
$$
\begin{align}
    \int \frac{-1}{5x}\frac{dx}{dt}dt &= \int \frac{1}{100-2t}dt \\
    \frac{-1}{5} \log\left(-5x\right) &= \frac {-1} 2 \log\left(100-2t\right) + C \\
    \log\left(-5x\right)^{-\frac 1 5} &= \log\left(100-2t\right)^{- \frac 1 2} + C \\
    \left(-5x\right)^{-\frac 1 5} &= \left(100-2t\right)^{- \frac 1 2}e^C \\
    x&= C\left(100-2t\right)^{\frac 5 2} \\
\end{align}
$$

With the initial condition
$$
\begin{align}
    x(0) &= 300 = C \left(100-2t\right)^{\frac 5 2} \\
    \iff C &= \frac {300}{100^{\frac 5 2}} = \frac 3 {1000}
\end{align}
$$
$$\boxed{x= \frac 3 {1000} \left(100-2t\right)^{\frac 5 2}}$$

```tikz 
\usepackage{pgfplots}
\pgfplotsset{compat=1.13}
\begin{document} 
    \begin{tikzpicture}[scale=1.5]     
    \begin{axis}[
        axis lines=middle,
        xlabel=$t$,
        ylabel=$x(t)$,
        enlargelimits
    ]
        \addplot[domain=0:50, thick] {3/1000*(100-2*x)^(5/2)};
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```


##### Example
#example #homogenous 
> [!Example] Statement
> A tank initially holds 100 gallons of brine containing 30lb of dissolved salt. Fresh water flows into the tank at the rate of 3gal/min and brine flows  out at the same rate.


> - $x(t)$ is the amount of salt in the tank at time $t$
- There $\frac {x(t)}{100}$ pounds of salt per gallon 
- $3\Delta t$ gallons were ejected from the tank during a period of length $\Delta t$
- **Approximately** $-\frac {3x(t)}{100}\Delta t$ pounds of salt are ejected during a period of lenght $\Delta t$
$$\Delta x = -\frac {3x(t)}{100}\Delta t$$
Taking the limit and using [[../Solutions/Separation of variables|separation of variables]]
$$
\begin{align}
\lim_{\Delta t \rightarrow 0} \frac{\Delta x}{\Delta t} = \frac{\mathrm{d}x}{\mathrm{d}t}&=-0.03x(t)\\
\implies x(t)=Ce^{-0.03t}
\end{align}
$$
At $t=0$, we have $x_0=30$ lbs, so
$$x(0)=30=Ce^{-0.03t}\implies C=30$$
The solution to the differential equation is
$$x(t)=30e^{-0.03t}$$


> [!Example] (a)
> Find the sal content of the brine at the end of 10 minutes

So to answer the question, the salt content at of the brine at the end of 10 minutes is
```tikz 
\usepackage{pgfplots}
\pgfplotsset{compat=1.16}
\begin{document} 
    \begin{tikzpicture}[scale=1.5]     
    \begin{axis}[axis lines=middle,xlabel=$t$,ylabel=$x(t)$,enlargelimits]
        \newcommand\T{10};
        \newcommand\X{30*exp(-0.03*\T)};
        \addplot[
            domain=0:50, 
            thick,
        ] {30*exp(-0.03*x)};
        \node[above right] at (axis cs:10,22) {$x(10)=30e^{-0,03*10}\approx22.22$};
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (0,\X) (\T, \X) };
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (\T,0)  (\T, \X)  };
        
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```

> [!Example] (b) 
> When will the salt content be 15lbs?

```tikz 
\usepackage{pgfplots}
\pgfplotsset{compat=1.16}
\begin{document} 
    \begin{tikzpicture}[scale=1.5]     
    \begin{axis}[axis lines=middle,xlabel=$t$,ylabel=$x(t)$,enlargelimits]
        \newcommand\T{-ln(0.5)/0.03};
        \newcommand\X{15};
        \addplot[
            domain=0:50, 
            thick,
        ] {30*exp(-0.03*x)};
        \node[above right] at (axis cs:23.1,15) {$t=-\frac{\ln(0.5)}{0.03}\approx23.1$};
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (0,\X) (\T, \X) };
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (\T,0)  (\T, \X)  };
        
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```

##### Example
#example #non_homogeneous 
> [!Example] Statement
> A tank initially contains 200 gal of brine whose salt concentration is 3 lb/gal. Brine whose salt concentration is 2 lb/gal flows into the tank at the rate of 4 gal/min. The mixture flows out at the same rate. 

This is a [[../Definitions/Linear ODE#Nonhomogeneous|nonhomogeneous equation]]
$x(t)$ is still the amount of salt in total.
$$\dot{x} = -\frac{4}{200}x + 4\cdot2$$
We have a constant forcing over time of 8. This is a direct application of [[Constant input#Linear DE with forcing]]
$$x(t)=x_0e^{kt} + \frac {b} {k} e^{kt} -\frac {b} {k}$$
With $k=-0.02$, $b=8$ and $x(0)=600$. Filling in those values 
$$
\begin{align}
x(t)&= 600e^{-0.02t} + \frac{8}{-0.02}e^{-0.02t} - \frac{8}{-0.02}\\
&= (600 - 400)e^{-0.02t} +200 \\
&= 200e^{-0.02t} +400 \\
\end{align}
$$


```tikz 
\usepackage{pgfplots}
\pgfplotsset{compat=1.13}
\begin{document} 
    \begin{tikzpicture}[scale=1.5]     
    \begin{axis}[
        axis lines=middle,
        xlabel=$t$,
        ylabel=$x(t)$,
        enlargelimits,
    ]
        \addplot[domain=0:250, thick] {exp(-0.02*x)*200 + 400};
        \addplot[dashed] plot coordinates {(0,400) (250,400)};
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```

> [!Example] (a)
> Find the salt content of the brine at the end of 20 minutes.

At the 20 minute mark there is $x(20)=200e^{-0.02\cdot 20} +400\approx 534.06$ pounds of salt
```tikz 
\usepackage{pgfplots}
\pgfplotsset{compat=1.16}
\begin{document} 
    \begin{tikzpicture}[scale=1.5]     
    \begin{axis}[
        axis lines=middle,
        xlabel=$t$,
        ylabel=$x(t)$,
        enlargelimits,
        ymin=450, ymax=600
    ]
        \newcommand\T{20};
        \newcommand\X{200*exp(-0.02*\T)+400};
        \addplot[
            domain=0:60, 
            thick,
        ] {200*exp(-0.02*x)+400};
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (0,\X) (\T, \X) };
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (\T,0)  (\T, \X)  };
        \node[above right] at (axis cs:20,534.06) {$x(20)\approx534.06$};
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```


> [!Example] (b)
> When will the salt concentration be reduced to 2.5lb/gal?

That's $2.5\cdot200 = 500$ pounds of salt in total
$$500 = 200e^{-0.02t} +400 \iff
t=-\frac{\ln(0.5)}{0.02} \approx 34.66
$$

```tikz 
\usepackage{pgfplots}
\pgfplotsset{compat=1.16}
\begin{document} 
    \begin{tikzpicture}[scale=1.5]     
    \begin{axis}[
        axis lines=middle,
        xlabel=$t$,
        ylabel=$x(t)$,
        enlargelimits,
        ymin=400, ymax=600
    ]
        \newcommand\T{-ln(0.5)/0.02};
        \newcommand\X{500};
        \addplot[
            domain=0:50, 
            thick,
        ] {200*exp(-0.02*x)+400};
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (0,\X) (\T, \X) };
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (\T,0)  (\T, \X)  };
        \node[above right] at (axis cs:34.66,500) {$t\approx34.66$};
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```


##### Example
#example #non_homogeneous 
> [!Example] Statement
> A tank initially contains 100 gal of brine whose salt concentration is 0.5 lb/gal. Brine whose salt concentration is 2 lb/gal flows into the tank at the rate of 3 gal/min. The mixture flows out at the rate of 2 gal/min. Find the salt content of the brine and its concentration at the end of 30 minutes.
 
This is similar to a [[#^1b9c9c|previous example]] but trickier because we have a function of $t$ as a coefficient. Will need [[../Solutions/Integrating factor]], first time in this page. 

- Liquid at time $t$ is $(100 + t)$ as 3 gallons go in and 2 go out.
- $x(t)$ is the amount of salt at time $t$
- $x_0=100*0.5=50$ pounds of salt initially
- $\frac{x(t)}{100+t}$ is the concentration at time $t$
- **Out**
    - $2\Delta t$ is the number of gallons going out in $\Delta t$ minutes
    - $\frac{2x(t)}{100+t}\Delta t$ is **approximately** the amout of salt going out during $\Delta t$ minute
- **In**
    - $3\Delta t$ gallons are going in during $\Delta t$ minute
    - $3\cdot2\Delta t$ is exactly the amount of salt going in during $\Delta t$ minute

$$
\begin{align}
\Delta x &= -\frac{2x(t)}{100+t}\Delta t + 6\Delta t \\
\frac{\Delta x}{\Delta t} &= -\frac{2x(t)}{100+t} + 6 \\
\lim_{\Delta t \rightarrow 0} \frac{\Delta x}{\Delta t} &= \frac{\mathrm{d}x}{\mathrm{d}t}=-\frac{2}{100+t}x(t)+6\\
\iff \dot{x}+\frac{2}{100+t}x&=6
\end{align}
$$

Integrating factor
$$
\begin{align}
r(t)&=e^{\int \frac {2} {100+t}}dt\\
&= e^{2\log(100+t)}\\
&= (100+t)^{2}
\end{align}
$$
$$\begin{align}
x(t)&=\frac 1 {(100+t)^{2}}\int6(100+t)^{2}dt\\
&=\frac 1 {(100+t)^{2}}\left(2(100+t)^{3}+C\right)\\
&=\frac C {(100+t)^{2}}+2(100+t)
\end{align}
$$
$$
\begin{align}
x_0=50&=\frac C {(100+0)^{2}}+2(100+0)=\frac C {10000}+200\\
\iff C&=-150\cdot10000=-1500000
\end{align}
$$
$$x(t)=\frac {-1500000} {(100+t)^{2}}+2(100+t)$$
$$x(30)=\frac {-1500000} {(100+30)^{2}}+2(100+30)\approx 171.24$$


```tikz 
\usepackage{pgfplots}
\pgfplotsset{compat=1.16}
\begin{document} 
    \begin{tikzpicture}[scale=1.5]     
    \begin{axis}[
        axis lines=middle,
        xlabel=$t$,
        ylabel=$x(t)$,
        enlargelimits,
    ]
        \newcommand\T{30};
        \newcommand\X{-1500000/((100+\T)^2)+2*(100+\T)};
        \addplot[
            domain=0:100, 
            thick,
        ] {-1500000/((100+x)^2)+2*(100+x)};
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (0,\X) (\T, \X) };
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (\T,0)  (\T, \X)  };
        \node[below right] at (axis cs:30,171.24){$x(30)\approx171.24$};
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```

> [!Error]- What errors did I make?
> - Forgot to add a negative sign for the salt going out
> - Only did the integral of the integrating factor, leaving out the exponential
> - Did not multiply the amount of salt going in per gallon by the number of gallons. So it's as if only 2 lbs of salt were going in every minutes instead of 6. 
> - Did not know I had to use the integrating factor. Had to use wolfram alpha to look at the end result to realize what I needed to do.

##### Example ^22000f
#example #non_homogeneous 
> [!Example] Statement 
> A tank initially contains 200 gal of fresh water. Brine whose salt concentration is 2lbs/gal flows into the tank at the rate of 2 gal/min. The mixture flows out at the same rate. 
> a) Find the salt content of the brine at the end of 100 minutes

Same thing as [[#^e9c5ad|this example]]. 
- At time $t$ there is $x(t)$ lbs of salt
- Concentration is $\frac{x(t)}{200}$ lbs/gal
- **Out**
    - $2\Delta t$ gallons going out over $\Delta t$ minutes
    - **Approximately** $\frac{2\Delta t}{200}x(t)$ lbs going out over $\Delta t$ minutes
  
- **In**
    - $2\Delta t$ gallons going in
    - Exactly $2 \text{ gal/min} \cdot 2\text{ lbs/gal} \cdot \Delta t \text{ minute}=4\text{lbs}$ of salt are going in per $\Delta t$ minute. 

$$\dot{x}=-0.01x+4$$

$$x(t)=-400e^{-0.01t}+400$$
$$x(100)=-400e^{-1}+400\approx 252.85$$
  
> [!Note]- [[../Solutions/Separation of variables]]
> More details. As usual, the constant $C$ absorbs everything.
> $$
> \begin{align}
>     \frac{1}{-0.01x + 4}\frac{dx}{dt} &= 1 \\
>     \int \frac{1}{-0.01x + 4}\frac{dx}{dt}dt &= \int dt \\
>     -100\log\left(-0.01x + 4\right)&= t + C\\
>     \log\left(-0.01x + 4\right)&= -0.01t + C\\
>     -0.01x + 4&= e^{-0.01t + C}\\
>     x&= -100(Ce^{-0.01t} - 4)\\
>     x&= Ce^{-0.01t} + 400\\
>     x&= -400e^{-0.01t}+400
> \end{align}
> $$
> With the initial condition that $x(0) = 0$ we find that $C=-400$.

> [!Note]- [[Constant input#Linear DE with forcing]]
> This is a [[../Definitions/Linear ODE#Nonhomogeneous|nonhomogeneous equation]]
> $$\dot{x} = -0.01x + 4$$
> We have a constant forcing over time of 4. 
> $$x(t)=x_0e^{kt} + \frac {b} {k} e^{kt} -\frac {b} {k}$$
> With $k=-0.01$, $b=4$ and $x(0)=0$. Filling in those values we find the exact same solution
> $$x(t)= -400e^{-0.01t} + 400$$

> [!Error]- What errors did I make?
> - Did not answer the question, just found the solution
> - Still have to go through all the steps, not a second nature to solve these kinds of problems

> [!Example] (b) 
> At what time will the salt concentration reach 1 lb/gal?
> 1 lb/gal means 200 pounds of salt in total

$$
\begin{align}
200&=-400e^{-0.01t}+400 \\
\iff t&=-100\ln{0.5}=69.31
\end{align}$$

> [!Example] (c) 
> Could the salt content of the brine ever reach 400lbs?

No because $-400e^{-0.01t}$ can never be zero. 

```tikz 
\usepackage{pgfplots}
\pgfplotsset{compat=1.13,width=12cm}
\begin{document} 
    \begin{tikzpicture}
    \begin{axis}[axis lines=middle,xlabel=$t$,ylabel=$x(t)$,enlargelimits]
        \addplot[domain=0:400, thick] {-400*exp(-0.01*x) + 400};
        \addplot[dashed] plot coordinates {(0,400) (400,400)};
        \newcommand\T{-100*ln{0.5}};
        \newcommand\X{200};
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (0,\X) (\T, \X) };
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (\T,0)  (\T, \X)  };
        \node[right] at (axis cs:80,200) {$t\approx69.31$};
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```


##### Example
#example #non_homogeneous 
> [!Example] Statement
> A tank initially contains 200 gal of fresh water. It receives brine of an unknown concentration at the rate of 2 gal/min. The mixture flows out at the same rate. At the end of 120 minutes, 280 lbs of salt are in the tank. Find the salt concentration of the entering brine. 

Same as [[#^22000f]] except that the concentration is unknown
[[Constant input#Linear DE with forcing]]
$$\dot{x} = -0.01x + 2s$$
We have a constant forcing over time of $2s$. 
$$x(t)=x_0e^{kt} + \frac {b} {k} e^{kt} -\frac {b} {k}$$
With $k=-0.01$, $b=2s$ and $x(0)=0$. Filling in those values we find the solution
$$x(t)= -200se^{-0.01t} +200s$$
$$
\begin{align}
x(120)&=280=-200se^{-0.01\cdot120}+200s\\
 \iff s&=\frac{280}{-200e^{-1.2}+200}\approx 2.00341
 \end{align}
$$
##### Example
#example #system_of_ODE #homogenous 

> [!Example] Setup
> Two tanks, A and B, each contain 5000 gal of water. To each tank 150 gal of a chemical should be added, but in error the entire 300 gal are poured into the A tank. Pumps are set to work to circulate the liquid through the two tanks at the rate of 100 gal/min.

||A|B|
|-|-|-|
|Initial amount|$x_A(0)=300$|$x_B(0)=0$|
|Amount at time $t$|$x_A(t)$|$x_B(t)$|
|Concentration at time $t$|$\frac{x_A(t)}{5000}$|$\frac{x_b(t)}{5000}$|
|Gallons going out during $\Delta t$ minutes|$-100\Delta t$|$-100\Delta t$|
|$\approx$ Amount going out during $\Delta t$ minutes|$-\frac{100x_A(t)}{5000}\Delta t$|$-\frac{100x_b(t)}{5000}\Delta t$|
|Gallons going in during $\Delta t$ minutes|$100\Delta t$|$100\Delta t$|
|$\approx$ Amount going in during $\Delta t$ minutes|$\frac{100x_B(t)}{5000}\Delta t$|$\frac{100x_A(t)}{5000}\Delta t$

Approximate change in tank A
$$\Delta x_A(t)=-\frac{100x_A(t)}{5000}\Delta t+\frac{100x_B(t)}{5000}\Delta t$$

Limit
$$
\lim_{\Delta t \rightarrow 0}\frac{\Delta x_A(t)}{\Delta t}=\boxed{\dot{x}_A(t)=-\frac{x_A(t)}{50}+\frac{x_B(t)}{50}}
$$


Approximate change in tank B
$$\Delta x_B(t)=-\frac{100x_B(t)}{5000}\Delta t+\frac{100x_A(t)}{5000}\Delta t$$
Limit
$$
\lim_{\Delta t \rightarrow 0}\frac{\Delta x_B(t)}{\Delta t}=\boxed{\dot{x}_B(t)=-\frac{x_B(t)}{50}+\frac{x_A(t)}{50}}
$$


Each states depend on both $x_A$ and $x_B$, so we will solve a system of equations with 
$$\mathbf{x}=\begin{bmatrix}x_A(t) \\ x_B(t)\end{bmatrix},\hspace{1em}
\dot{\mathbf{x}}=
\underbrace{\begin{bmatrix}
-\frac{1}{50} & \frac{1}{50} \\
\frac{1}{50} & -\frac{1}{50}
\end{bmatrix}}_{\mathbf{A}}
\begin{bmatrix}
x_A \\ x_B
\end{bmatrix}
$$

Since the system is coupled, we need to [[../Decoupling a system of ODE|decouple it]]. Eigenvalues of $\mathbf{A}$ are $-\frac{1}{25}$ and 0 and their respective eigenvectors are $\begin{bmatrix}-1&1\end{bmatrix}^T$ and $\begin{bmatrix}1&1\end{bmatrix}^T$

$$
\mathbf{D}=\begin{bmatrix}\frac{-1}{25}&0\\0&0\end{bmatrix},\hspace{1em}\mathbf{T}=\begin{bmatrix}-1&1\\1&1\end{bmatrix},\hspace{1em}\mathbf{T}^{-1}=\begin{bmatrix}-\frac{1}{2}&\frac{1}{2} \\ \frac{1}{2}& \frac{1}{2}\end{bmatrix}
$$

$$
\begin{align}
\mathbf{x}&=
\begin{bmatrix}
-1 & 1 \\ 1 & 1
\end{bmatrix}
e^{
\begin{bmatrix}
-\frac 1 {25} & 0 \\ 0 & 0
\end{bmatrix}t}
\begin{bmatrix}
-\frac{1}{2} & \frac 1 2 \\ \frac 1 2 & \frac 1 2
\end{bmatrix}
\begin{bmatrix} 300 \\ 0 \end{bmatrix} \\
&= \begin{bmatrix}
-1 & 1 \\ 1 & 1
\end{bmatrix}
\begin{bmatrix}
e^{-\frac t {25}} & 0 \\ 0 & 1
\end{bmatrix}
\begin{bmatrix}
-\frac{1}{2} & \frac 1 2 \\ \frac 1 2 & \frac 1 2
\end{bmatrix}
\begin{bmatrix} 300 \\ 0 \end{bmatrix} \\
&=150\begin{bmatrix}
1+e^{-\frac t {25}}  \\
1-e^{-\frac t {25}} 
\end{bmatrix}
\end{align}
$$

```tikz 
\usepackage{pgfplots}
\pgfplotsset{compat=1.16,width=12cm}
\begin{document} 
    \begin{tikzpicture}
    \begin{axis}[axis lines=middle,xlabel=$t$,ylabel=$x(t)$,enlargelimits]
        \addplot[domain=0:100, thick] {150*(1+exp(-x/25))}
        node [above] {Tank A};
        \addplot[domain=0:100, thick] {150*(1-exp(-x/25))}
        node [below] {Tank B};
        \addplot[dashed] plot coordinates {(0,150) (100,150)};
        \newcommand\T{-25*ln(1/3)};
        \newcommand\XA{200};
        \newcommand\XB{300-\XA}
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (0,\XA) (\T, \XA) };
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (0,\XB) (\T, \XB) };
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (\T,0)  (\T, \XA)  };
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```

```tikz
\usepackage{tikz}
\usepackage{pgfplots}
\pgfplotsset{
    compat = newest,
    width=10cm,
}
\begin{document}
\begin{tikzpicture}[scale=1.5] 
    \begin{axis}[
        title={},
        view={0}{90},
        axis background/.style={fill=white},
        domain=0:100,
        colormap/viridis,
        colorbar,
        colorbar style = {
            ylabel = {Vector Length}
        }
    ]
        \def\U{1}
        \def\V{-6*e^(-x/25)}
        \def\LEN{(sqrt((3*\U)^2 + (\V)^2)}
        \addplot[thick,samples=100] {150*(1+exp(-x/25))};
        \addplot3 [
            point meta=(sqrt((\U)^2 + (\V)^2),
            -stealth,samples=15,
            domain y=150:300,
            quiver={
                u={(\U)/\LEN}, v={(\V)/\LEN},
                scale arrows= 8.0
            },
            quiver/colored = {mapped color},
        ]{0};
    
        \def\V{6*e^(-x/25)}
        \def\LEN{(sqrt((3*\U)^2 + (\V)^2)}
        \addplot[thick,samples=100] {150*(1-exp(-x/25))};
        \addplot3 [
            point meta=(sqrt((\U)^2 + (\V)^2),,
            -stealth,samples=15,
            domain y=0:150,
            quiver={
                u={(\U)/\LEN}, v={(\V)/\LEN},
                scale arrows=8.0
            },
            quiver/colored = {mapped color},
        ]{0};
    \end{axis}
\end{tikzpicture}
\end{document}

```

> [!Example] (a)
> How long will it take for the tank A to contain 200 gal of the chemical and tank B to contain 100 galons?
> 
$$
\begin{align}
x_A(t)=200&=150(1+e^{-\frac t {25}})\\
\frac{200}{150}-1&=e^{-\frac t{25}} \\
t&=-25\ln{\frac 1 3} \approx 27.46
\end{align}
$$
Or equivalently
$$
\begin{align}
x_B(t)=100&=150(1-e^{-\frac t {25}})\\
\frac{100}{150}-1&=e^{-\frac t{25}} \\
t&=-25\ln{\frac 1 3} \approx 27.46
\end{align}
$$


> [!Error]- What went wrong?
> Everything, it seems. 
> - Did not realized this was a system of equations. Both $x_A$ and $x_B$ are functions of time. The state of the system depends on two values, so this requires a system of ODE
> - Not too sure how to write a matrix exponential in Wolfram Alpha, but it was not giving what I expected. The final result was giving negative time values. Once I checked the matrix exponential part and realized it was wrong, everything clicked. Good old divide and conquer.
> - My notes on [[../Decoupling a system of ODE|decoupling a system of ODE]] was incomplete and I could not easily find what I was looking for. The solution was scattered in other notes. 


> [!Example] (b)
> Is it theoretically possible for each tank to contain 150 gal?

The solution for tank A is 
$$x_A(t)=150(1+e^{-\frac t {25}})$$
Because $(1+e^{-\frac t{25}})>1 ,\forall t \ge 0$, $x_A(t)=150(1+e^{-\frac t{25}}) > 150,\forall t\ge 0$, thus never reaching the value of 150. 
Similar logic can be applied when looking at the amount in tank B. 

##### Example 
#example #non_homogeneous 
> [!Example] Statement
> The $CO_2$ content of the air in a 5000-cu-ft room is 0.3 percent. Fresh air containing 0.1 percent $CO_2$ is pumped into the room at the rate of 1000 $ft^3/min$.

||A|
|-|-|
|Initial amount|$x(0)=5000\cdot0.003=15$|
|$CO_2$ at time $t$|$x(t)$|
|$CO_2$ concentration at time $t$|$\frac{x(t)}{5000+1000t}$|
|Air going in during $\Delta t$ minutes|$1000\Delta t$|
|$\approx CO_2$ going in during $\Delta t$ minutes|$1000\cdot0.001\Delta t=\Delta t$|

Nothing is going out and the amount going in is constant. It's pretty simple, but let's take the same route as all the example above. 
**Approximate** change in $CO_2$
$$\Delta x = \Delta t$$
Limit
$$
\lim_{\Delta t \rightarrow 0} \frac{\Delta x}{\Delta t} = \dot{x}=1
$$
Using [[../Solutions/Separation of variables|separation of variables]]
$$
\begin{align}
\int \frac{dx}{dt}&=\int dt \\
x(t)=t+C
\end{align}
$$
Initial condition is $x(0)=0+15$
$$\boxed{x(t)=t + 15}$$

```tikz 
\usepackage{pgfplots}
\pgfplotsset{compat=1.16}
\begin{document} 
    \begin{tikzpicture}[scale=1.5]     
    \begin{axis}[
        axis lines=middle,
        xlabel=$t$,
        ylabel=$x(t)$,
        enlargelimits,
    ]
        \addplot[
            domain=0:100, 
            thick,
            samples=200
        ] {(x+15)/(5000+1000*x)};
        \newcommand\T{30};
        \newcommand\X{(\T+15)/(5000+1000*\T)};
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (0,\X) (\T, \X)};
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (\T,0)  (\T, \X)};
    \end{axis} 
    \end{tikzpicture} 
    
\end{document} 
```

---
What is not stated here is that air is also going out. Had to figure it out based on the answer in the book. 

||A|
|-|-|
|Initial amount|$x(0)=5000\cdot0.003=15$|
|$CO_2$ at time $t$|$x(t)$|
|$CO_2$ concentration at time $t$|$\frac{x(t)}{5000}$|
|Air going in during $\Delta t$ minutes|$1000\Delta t$|
|$\approx CO_2$ going in during $\Delta t$ minutes|$1000\cdot0.001\Delta t=\Delta t$|
|Air going out during $\Delta t$ minutes|$1000\Delta t$|
|$\approx CO_2$ going out during $\Delta t$ minutes|$\frac{1000x}{5000}\Delta t=0.2x\Delta t$|

**Approximate** change in $CO_2$
$$\Delta x = -\frac{1000x}{5000}\Delta t+\Delta t$$
Limit
$$
\lim_{\Delta t \rightarrow 0} \frac{\Delta x}{\Delta t} = \dot{x}=-0.2x+1
$$
Using [[../Solutions/Separation of variables|separation of variables]]
$$
\begin{align}
\int \frac{dx}{dt}dt&=\int dt \\
\int \frac 1 {-0.2x+1}dx&=\int dt \\
-5\ln\left(-0.2x+1\right)&= t+C_1\\
\ln\left(-0.2x+1\right)&= -\frac t 5+C_2\\
x(t)&=Ce^{-0.2t}+5
\end{align}
$$

With initial condition $x(0)=15\implies C=10$

$$\boxed{x(t)=10e^{-\frac{t}{5}}+5}$$
```tikz 
\usepackage{pgfplots}
\pgfplotsset{compat=1.16}
\begin{document} 
    \begin{tikzpicture}[scale=1.5]     
    \begin{axis}[
        title=Concentration $\frac{10e^{-\frac{t}{5}}+5}{5000}$,
        axis lines=middle,
        xlabel=$t$,
        ylabel=$x(t)$,
        enlargelimits,
    ]
        \addplot[
            domain=0:40, 
            thick,
            samples=200
        ] {(10*e^(-x/5)+5)/5000};
        \newcommand\T{30};
        \newcommand\X{(10*e^(-\T/5)+5)/5000};
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (0,\X) (\T, \X)};
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (\T,0)  (\T, \X)};
    \end{axis} 
    \end{tikzpicture} 
    
\end{document} 
```

> [!Example] (a)
> Find the percentage of $CO_2$ in the room after 30 minutes.

Concentration at time $t$
$$
\frac{x(30)}{5000}=\frac{10e^{-\frac{30} 5}+5}{5000}\approx0,0010049575
$$

> [!Example] (b)
>  When will the $CO_2$ content be 0.2 percent? 

$$
\begin{align}
0.002\cdot5000&=10e^{-\frac t 5}+5\\
t&=-5\ln{\frac{10-5}{10}}\\
t&\approx3.4657
\end{align}
$$

##### Example
#example #non_homogeneous 
 
> [!Example] Statement
> The $CO_2$ content of the air in a 7200-cu-ft room is 0.2 percent. What volume of fresh air containing 0.05 percent of $CO_2$ must be pumped into the room each minute in order to reduce the $CO_2$ content to 0.1 percent in 15 min?

||v|
|-|-|
|Initial amount of $CO_2$|$x(0)=7200*0.002=14.4$|
|Amount of $CO_2$ at time $t$|$x(t)$|
|Concentration of $CO_2$ at time $t$|$\frac{x(t)}{7200}$|
|Volume going in and out during $\Delta t$ min|$V\Delta t$|
|Amount of $CO_2$ going out during $\Delta t$ min|$-\frac{Vx(t)}{7200}\Delta t$|
Amount of $CO_2$ going in during $\Delta t$ min|$0.0005V\Delta t$|
|Target $CO_2$|$x(15)=7200\cdot0.001=7.2$|

**Approximate** change in $CO_2$
$$
\begin{align}
\Delta x&=-\frac{Vx}{7200}\Delta t + 0.0005V\Delta t\\
\frac{\Delta x}{\Delta t}&=\frac{-Vx}{7200} + 0.0005V
\end{align}$$
Limit
$$
\begin{align}
\lim_{\Delta t \rightarrow 0}\frac{\Delta x}{\Delta t}=\frac{dx}{dt}&=\frac{-Vx}{7200} + 0.0005V=\frac{-Vx+3.6V}{7200}
\end{align}$$

[[../Solutions/Separation of variables]]
$$
\begin{align}
\frac{7200}V\int \frac 1 {-x+3.6}\frac{dx}{dt}dt&=\int dt \\
-\frac{7200}V\ln\left(-x+3.6\right)&=t+C_1\\
\ln\left(-x+3.6\right)&=-\frac{Vt}{7200}+C_2\\
x(t)&=Ce^{-\frac{Vt}{7200}}+3.6
\end{align}
$$
With initial condition $x_0=14.4\implies C=14.4-3.6=10.8$
$$x(t)=10.8e^{-\frac{Vt}{7200}}+3.6$$

$$
\begin{align}
x(15)&=0.001\cdot7200=7.2=10.8e^{-\frac{15V}{7200}}+3.6\\
\iff V&=-\frac{7200}{15}\ln\left(\frac{7.2-3.6}{10.8}\right) \\
&= -480\ln\left(\frac 1 3\right) \\
&\approx 527.33
\end{align}
$$


> [!Error]- What went wrong
> Sign error. Of course. 
