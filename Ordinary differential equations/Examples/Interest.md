Unless specified, all the examples below use continuously compounded interest rate. 

##### Example
#example 
> [!Example] Statement
> How long will it take for 1.00$ to double itself if it is coumpounded continuously at 4% per annum.

$$
\begin{align}
P(t)=2&=1.00e^{0.04t}\\
\iff t&=25\ln(2)\approx 17.33
\end{align}
$$
---
##### Example
#example 
> [!Example] Statement
> At what interest rate will 1.00$ double itself in 12 years?

$$
\begin{align}
P(12)=2=1.00e^{12r}\\
\iff r=\frac{1}{12}\ln(2)\approx 5.77%
\end{align}
$$
---
##### Example
#example 
> [!Example] Statement
> How much will 1000.00$ be worth at 4.5% interest after 10 years?


$$P(10)=1000e^{0.045\cdot10}\approx 1568.31$$

---
##### Example
#example 
> [!Example] Statement
> In a will, a man left a few million dollars, to be divided among several trusts. The will provided that the money was to be deposited in savings institutions and held for 500 years before being distributed to the designated legatees. The will was contested by the government on the grounds that the monetary wealth of the nation would be concentrated on these trusts. I'd the money earned an average of 4%, approximately how much only 1 000 000$ amount at the end of 500 years?

$$P(500)=1000000e^{0.04\cdot500}\approx 4.85\cdot10^{14}$$

---
##### Example
#example 
> [!Example] Statement
> How much money would you need to deposit in a bank at 5% interest in order to be able to withdraw 3600$/year for 20 years if you wish the entire principal to be consumed at the end of this time



> [!Example] (a)
> if the money is also being withdrawn continuously from the date of deposit as, for example, withdrawing 3600$/365 each day. 

$$
\begin{align}
\Delta x &= 0.05x \Delta t - 3600\Delta t \\
\frac{\Delta x}{\Delta t}&=0.05x-3600 \\
\frac{dx}{dt} = \lim_{\Delta t \rightarrow 0}\frac{\Delta x}{\Delta t} &= 0.05x-3600
\end{align}
$$
Using [[Separation of variables]],
$$
x(t)=Ce^{0.05t}+72000
$$
The only data point we have is that the capital is depleted after 20 years, so we use that to find $C$
$$
\begin{align}
x(20)=0&=Ce^{0.05 \cdot 20} + 72000 \\
\iff C &= -\frac{72000}{e} \\
\implies x(t)&=-\frac{72000}{e}e^{0.05t} + 72000 \\
&= 72000(1-e^{0.05t-1})
\end{align}
$$
From this we can get the initial capital at $t=0$
$$
x(0)=72000(1-e^{-1}) \approx 45512.48$
$$

```tikz 
\usepackage{pgfplots}
\pgfplotsset{compat=1.13,width=15cm}
\begin{document} 
    \begin{tikzpicture}
    \begin{axis}[axis lines=middle,xlabel=$t$,ylabel=$x(t)$,enlargelimits]
        \addplot[domain=0:20, thick] {-72000/exp(1)*exp(0.05*x) + 72000};
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```



> [!Example] (b)
> If the money is being withdrawn at the rate of 300$/month beginning with the first month after the deposit.

Interest rate is continuous. So if $\dot{x}=0.05x$, then $x(t) = Ae^{0.05t}$. One month is $t=\frac{1}{12}$, then the capital is  $Ae^{\frac{0.05}{12}}$. At the end of the month, 300 is withdrawn. For simplicity of notation, $r=e^{\frac{0.05}{12}}$

$$
\begin{align}
&\underbrace{
    \left(
        \left(
            \underbrace{
                \left(
                    \underbrace{
                        \left(
                            \underbrace{Ar-300}_{\text{month 1}}
                        \right)r-300
                    }_{\text{month 2}}
                \right)r-300
            }_{\text{month 3}}
        \right)\ldots
    \right)r-300
}_{\text{month t}} \\
=&\underbrace{
    \left(
        \left(
            \underbrace{
                \left(
                    \underbrace{Ar^2-300r-300}_{\text{month 2}}
                \right)r-300
            }_{\text{month 3}}
        \right)\ldots
    \right)r-300
}_{\text{month t}} \\
=&\underbrace{
    \left(
        \left(
            \underbrace{Ar^3-300r^2-300r-300}_{\text{month 3}}
        \right)\ldots
    \right)r-300
}_{\text{month t}} \\
=&Ar^{t} - 300\sum_{k=0}^{t-1}r^k \\
=&Ar^{t} - 300\frac{1-r^{t}}{1-r}
\end{align}
$$

We then use the fact that the capital is depleted at the end when $t=240$
$$
\begin{align}
Ar^{240} - 300\frac{1-r^{240}}{1-r} &= 0 \\
\iff Ar^{240} &= 300\frac{1-r^{240}}{1-r} \\
A &= 300\frac{1-r^{240}}{r^{240}(1-r)} \\
A &= 300\frac{1-r^{240}}{r^{240}-r^{241}} \\
A &= 300\frac{1-r^{240}}{r^{240}-r^{241}} \\
A &= 300\frac{1-e^{240\frac{0.05}{12}}}{e^{240\frac{0.05}{12}}-e^{241\frac{0.05}{12}}}  \\
A &= 300\frac{1-e^{20 \cdot 0.05}}{e^{20 \cdot 0.05}-e^{20 \cdot 0.05 + \frac{0.05}{12}}} \\ 
A &= 300\frac{1-e}{e-e^{\frac{12.05}{12}}} \\ 
\end{align}
$$

The initial amount required, considering that the interest rate is continuous and the withdrawal rate is 300 per month, is $$\boxed{A = 300\frac{1-e}{e-e^{\frac{12.05}{12}}}} = 45417,93\$$$

The capital at time $t$ is

$$
\begin{align}
x(t) &= 300\frac{1-e}{e-e^{\frac{12.05}{12}}} e^{\frac{0.05}{12}t} - 300 \frac{1-e^{\frac{0.05}{12}t}}{e^{\frac{0.05}{12}t}-e^{\frac{0.05}{12}(t+1)}} \\
&= 300\frac{e^{\frac{0.05}{12}t}-e^{\frac{0.05t+12}{12}}}{e-e^{\frac{12.05}{12}}}  - 300 \frac{1-e^{\frac{0.05}{12}t}}{e^{\frac{0.05}{12}t}-e^{\frac{0.05}{12}(t+1)}}
\end{align}
$$
```tikz 
\usepackage{pgfplots}
\pgfplotsset{compat=1.13,width=15cm}
\begin{document} 
    \begin{tikzpicture}
    \begin{axis}[
        axis lines=middle,
        xlabel=$t$,
        ylabel=$x(t)$,
        enlargelimits,
        samples=200
    ]
        \newcommand\R{exp(0.05/12)};
        \newcommand\A{300*(1-exp(1))/(exp(1)-exp(12.05/12))};
        \addplot[domain=0:240, thick] {
            \A* \R^x 
            -300*(
                1-\R^x
            )/(
                1-\R
            )
        };
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```


> [!Error]- What went wrong
> Question was not super clear. The interested rate is continuous here. Found out after analyzing the answer from the book. I more or less answered the next question where I recalculated the interest rate on a monthly basis and withdrew every month. You end up with a geometric sum, not with exponential anywhere



> [!Example] (c)
> Try to solve this problem assuming the more realistic situation of interest begin credited quarterly and 900$ withdrawn quarterly, beginning with the first quarter after the deposit. This no longer involves a differential equation. 


---
##### Example
#example 
> [!Example] Statement
> You plan to retire in 30 years. At the end of that time, you wish to have $45,500.00, the approximate amount needed in order to withdraw 300.00 monthly for 20 years after retirement. 



> [!Example] (a)
> What amount must you deposit monthly at 5%?



> [!Example] (b)
> What amount must you deposit semiannually if interest is credited semiannually at 5% instead of continuously? This problem no longer involves differential equations. 


