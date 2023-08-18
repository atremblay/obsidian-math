
> [!Summary]-
> Problems which involve joint proportionality factors are known as **second order processes**.
> 
> If one unit of $C$ is formed by combining $m$ units of $A$ and $n$ units of $B$, then the differential equation becomes 
> $$ \frac{dx}{dt} = k(s_1-mx)(s_2-nx),$$
> Where $s_1$ and $s_2$ are the respective number of units of $A$ and $B$ present initially and $x$ is the number of units of $C$ present in time $t$.



> [!Definition]+ Jointly Proportional
> First order process used $\frac{dx}{dt}=kx$; the rate of change was proportional to the quantity.
> Jointly proportional means that its proportional to two things and not just one $\frac{dx}{dt}=kf(x)g(x)$. Why are $f(x)$ and $g(x)$ multiplied together? No idea.

A new substance $C$ is sometimes formed from two given substances $A$ and $B$ by taking something away from each; the growth of the new substance being jointly proportional to the amount *remaining* of each of the original substances. Let $s_1$ and $s_2$ be the respective amounts of $A$ and $B$ present initially and let $x$ represent the number of units of the new substance $C$ formed in time $t$. If, for example, one unit of $C$ is formed by combining 2 units of substance $A$ with three units of substance $B$, then when $x$ units of the new substance are present at time $t$

<div style="text-align: center;">
      <span class="math display">(s_1 - 2x)</span> is the amount of <span class="math display">A</span> remaining at time <span class="math display">t</span>.<br>
      <span class="math display">(s_2 - 3x)</span> is the amount of <span class="math display">B</span> remaining at time <span class="math display">t</span>.
</div>

By the first sentence above, therefore, the differential equation which represents the rate of change of $C$ at any time $t$ is given by 

$$\frac{dx}{dt} = k(s_1-2x)(s_2-3x),$$
Where $k$ is a proportionality constant. In general, if one unit of $C$ is formed by combining $m$ units of $A$ and $n$ units of $B$, then the differential equation becomes
$$\frac{dx}{dt} = k(s_1-mx)(s_2-nx), \tag{1}$$
Where $s_1$ and $s_2$ are the respective number of units of $A$ and $B$ present initially and $x$ is the number of units of $C$ present in time $t$.

## Dissolution

> [!definition]+ Saturated Solution
> Solution that is fully saturated, cannot contain anymore of whatever we are dissolving.


A substance may also be dissolved in a solution, its rate of dissolutions being jointly proportional to:
1. The amount of the substance which is still undissolved.
2. The difference between the concentration of the substance in a saturated solution and the actual concentration of the substance in the solution. 
> For example, if 10 gallons of water can hold a maximum of 30 pounds of salt, it is said to be saturated when it holds this amount of salt. The concentration of salt in a saturated solution is then 3 pounds per gallon. When therefore the solution contains only 15 pounds of salt, the actual concentration of the salt in the solution is 1.5 pounds per gallon or 50 percent of saturation.

Let $x$ represent the amount of the substance *undissolved* at any time $t$, $x_0$ the initial amount of the substance,and $v$ the volume of the solution.

Then at any time $t$,

$(x_0-x)$ is the amount of the substance *dissolved* in the solution,
$\displaystyle\frac{x_0-x}{v}$ is the concentration of the substance in the solution.

If $c$ represents the concentration of the substance in a saturated solution, then the differential equation which expresses mathematically conditions 1 and 2 above is 

$$
\frac{dx}{dt} = kx\left(c-\frac{x_0-x}{v}\right) \tag{1.1}
$$

^9fedca

##### Example
#example #seperation_of_variable #partial_fraction
> [!Example] Statement
> A new substance $C$ is to be formed by removing two units from each of two substances whose initials quantites are 10 and 8 units respectively. Assume that the rate at which the new substance is formed is jointly proportional to the amount remaining of each of the original substances. If $x$ is the number of units of $C$ formed at any time $t$ and $x=1$ unit when $t=5$ minutes, find $x$ when $t=10$ minutes.

Using (1)
$$
\frac{\mathrm{d}x}{\mathrm{d}t} = k(10-2x)(8-2x) = 4k(5-x)(4-x)
$$

[[Solutions/Separation of variables|Separation of variables]]

$$
\begin{align}
\int \frac{\mathrm{d}x}{\mathrm{d}t} \mathrm{d}t &= \int 4k(5-x)(4-x) \mathrm{d}t \\
\int \frac{1}{(5-x)(4-x)}\mathrm{d}x &= 4k \int \mathrm{d}t \\
\end{align}
$$
This calls for [[../Partial Fraction Expansion|Partial Fraction Expansion]]

> [!solution]- Partial fraction
> $$
> \begin{align}
> \frac{1}{(5-x)(4-x)} &= \frac{A}{5-x} + \frac{B}{4-x} \\
> &= \frac{A(4-x) + B(5-x)}{(5-x)(4-x)}\\ 
>  \frac{1}{\cancel{(5-x)(4-x)}} &= \frac{A(4-x) + B(5-x)}{\cancel{(5-x)(4-x)})}\\
>  1 &= x(-A-B) + (4A + 5B)\\
> \end{align}
> $$
> Match the coefficients of like powers
> $$
> \begin{align}
> -A-B &= 0 \\
> 4A + 5B &= 1
> \end{align}
> $$
> Yields $A=-1$ and $B=1$
> $$\frac{1}{(5-x)(4-x)} = \frac{1}{4-x} - \frac{1}{5-x}$$

$$
\begin{align}
\int \frac{1}{(5-x)(4-x)}\mathrm{d}x &= 4k \int \mathrm{d}t \\
\int \left(\frac{1}{4-x} - \frac{1}{5-x}\right)\mathrm{d}x &= 4k \int \mathrm{d}t \\
\end{align}
$$
We need to use the conditions provided to find the value of $k$, namely that $x=1$ when $t=5$
$$
\begin{align}
\int_0^1 \left(\frac{1}{4-x} - \frac{1}{5-x}\right)\mathrm{d}x &= 4k \int_0^5 \mathrm{d}t \\
-\log(4-x) + \log(5-x)\bigg\lvert_0^1 &= 4kt \bigg\lvert_0^5 \\
\log\left(\frac{5-x}{4-x}\right)_0^1 &= 20k  \\
\log\left(\frac{4}{3}\right) - \log\left(\frac{5}{4}\right) &= 20k  \\
\log\left(\frac{4}{5}\frac{4}{3}\right) &= 20k  \\
\frac 1 {20} \log\left(\frac{16}{15}\right) &= k  \\
\end{align}
$$
Now that we found the value of $k$, we can find the answer to the problem; what is $\textcolor{orange}{x}$ when $\textcolor{cyan}{t=10}$

> Slight abuse of notation here, the $x$ in the integral is not the same as the upper limit $\textcolor{orange}{x}$
$$
\begin{align}
\int_0^{\textcolor{orange}{x}} \left(\frac{1}{4-x} - \frac{1}{5-x}\right)\mathrm{d}x &= 4k \int_0^{\textcolor{cyan}{10}} \mathrm{d}t \\
-\log(4-x) + \log(5-x)\bigg\lvert_0^x &= 4kt \bigg\lvert_0^{10} \\
\log\left(\frac{5-x}{4-x}\right)_0^x &= 40k  \\
\log\left(\frac{5-x}{4-x}\right) - \log\left(\frac{5}{4}\right) &= 40k  \\
\log\left(\frac{4}{5}\left(\frac{5-x}{4-x}\right)\right) &= 40k  \\
\frac 1 {40} \log\left(\frac{4}{5}\left(\frac{5-x}{4-x}\right)\right) &= \frac 1 {20} \log\left(\frac{16}{15}\right) \qquad \text{replace } k  \\
\log\left(\frac{4}{5}\left(\frac{5-x}{4-x}\right)\right) &= 2  \log\left(\frac{16}{15}\right) \\
\log\left(\frac{4}{5}\left(\frac{5-x}{4-x}\right)\right) &=  \log\left(\left(\frac{16}{15}\right)^2\right)  \\
\frac{4}{5}\left(\frac{5-x}{4-x}\right) &=  \left(\frac{16}{15}\right)^2  \\
\end{align}
$$

Further simplification yields $19x=31 \iff x=\frac{19}{31}$. So 1.63 units of the substance $x$ are formed in 10 minutes.


##### Example
#example #substitution_rule
> [!Example] Statement
> Six grams of sulfure are placed in a solution of 100cc of benzol which when saturated will hold 10 grams of sulfur. If 3 grams of sulfur are in the solution in 50 minutes, how may grams will be in the solution in 250 minutes?

Dissolving requires ([[#^9fedca|1.1]]).
- Volume $v=100cc$
- The saturated solution will be $\displaystyle \frac{10g}{100cc}=0.1$ ($c$ in [[#^9fedca|1.1]])
- $x_0=6$ grams of sulfur initially
- $x$ is the amount of undissolved, so $(x_0 - x)$ is the dissolved amount

Applying [[#^9fedca|(1.1)]]

$$
\begin{align}
\frac{dx}{dt} = kx\left(c-\frac{x_0-x}{v}\right) &= kx\left(0.1 - \frac{6 - x}{100}\right) \\
&= 0.1kx - \frac{6kx - kx^2}{100} \\
&= \frac{10kx - 6kx + kx^2}{100} \\
&= \frac{4kx + kx^2}{100} \\
\dot{x} \frac{100}{4x+x^2}&= k \\
\dot{x} \frac{100}{x(4+x)}&= k
\end{align}
$$
> [!step]- This calls for [[../Partial Fraction Expansion|Partial Fraction Expansion]]
> 
> $$
> \begin{align}
> \frac{1}{x(x+4)} &= \frac{A}{x} + \frac{B}{x+4} \\
> &= \frac{A(x+4) + Bx}{x(x+4)} \\
> \frac{1}{\cancel{x(x+4)}} &= \frac{(A+B)x + 4A}{\cancel{x(x+4)}} \\
> 1 &= (A+B)x + 4A \\
> \end{align}
> $$
> Match like-powers
> 
> $$
> \begin{rcases}
>     \begin{align}
>     A+B&=0 \\
>     4A &= 1
>     \end{align}
> \end{rcases} \implies A=\frac 1 4, B=-\frac 1 4
> $$
> 
> $$
> \begin{align}
> \frac{1}{x(x+4)} &= \frac{1}{4x} - \frac{1}{4(4+x)} \\
> &= \frac{\cancel{16}+\bcancel{4x} - \bcancel{4x}}{\cancel{16}x(4+x)} \\
> &= \frac{1}{x(4+x)} \\
> \end{align}
> $$

$$
\begin{align}
\dot{x} \frac{100}{x(4+x)}&= k \\
100 \int \frac{dx}{dt}\left(\frac{1}{4x} - \frac{1}{4(4+x)}\right) &= k\int dt \\ 
100 \left(\int \frac{1}{4x}\frac{dx}{dt} dt - \int \frac{1}{4(4+x)}\frac{dx}{dt} dt\right) &= k\int dt \\ \tag{2}
100 \left(\int \frac{1}{4x} dx - \int \frac{1}{4(4+x)} dx\right) &= k\int dt \\ 
100 \left(\frac 1 4 \log(4x)  - \frac 1 4 \log\left(4(4+x)\right) \right) &= kt + C\\ 
25 \log\left(\frac{\cancel{4}x}{\cancel{4}(4+x)}\right) &= kt + C_1\\
\log\left(\frac{\cancel{4}x}{\cancel{4}(4+x)}\right) &= \frac{kt}{25} + C_2\\
\frac{x}{(4+x)} &= e^{\frac{kt}{25} + C_2}\\
\frac{x}{(4+x)} &= Ce^{kt/25}\\
\end{align}
$$
###### Initial conditions
With the initial condition that at $t=0$, $x_0=6$ we can find the value of $C$

$$
\frac{6}{4+6} = Ce^{0k/25} \implies C = 0.6
$$


###### Additional conditions
Then, with the given condition that 3 grams of sulfur are in the solution in 50 minutes ($x=3, (x_0-x)=3$ at $t=50$), we can find the value of $k$

$$
\begin{align}
\frac{3}{4+3} &= 0.6 e^{50k/25} \\
\iff k &= \frac 1 2 \log\left(\frac 3 7 \frac 1 {0.6} \right) \\
&= \frac 1 2 \log\left(\frac{30}{42}\right) = \frac 1 2 \log\left(\frac{5}{7}\right) \\
&= \log\left(\sqrt{\frac{5}{7}}\right) \\
\end{align}
$$

> [!step]- Alternative step - Definite Integral
> Working off of (2), we can do a definite integral with the condition (instead of an antiderivative)
> $$
> \begin{align}
> 100 \left(\int_6^3 \frac{1}{4x}\frac{dx}{dt} dt - \int_6^3 \frac{1}{4(4+x)}\frac{dx}{dt} dt\right) &= k\int_0^{50} dt \\
> \left. \left( 25\log\left(\frac{x}{4+x}\right) + C_1 \right)\right\lvert_6^3  &= \left(kt + C_2\right)\bigg\lvert_0^{50}\\
> \left( 25\log\left(\frac{3}{4+3}\right) + \cancel{C_1} \right) - \left( 25\log\left(\frac{6}{4+6}\right) + \cancel{C_1} \right) &= (50k + \bcancel{C_2}) - (0k + \bcancel{C_2})\\
>  25\log\left(\frac{3}{7}\frac{10}{6}\right) &= 50k\\
>  \frac 1 2 \log\left(\frac{5}{7}\right) &= k\\
>  k &= \log\left(\sqrt{\frac{5}{7}}\right) \\
> \end{align}
> $$


###### Solution
So the formula for the amount of undissolved sulfure is
$$
\begin{align}
\frac{x(t)}{x(t) +4} &= 0.6e^{\log\left(\sqrt{\frac{5}{7}}\right)t/25} \\
&= 0.6\left(e^{\log\left(\sqrt{\frac{5}{7}}\right)}\right)^{t/25} \\
&= 0.6\left(\sqrt{\frac{5}{7}}\right)^{t/25}
\end{align}
$$
> [!step]- Alternative step - Definite Integral
> Again, working off of (2), we can do a definite integral 
> $$
> \begin{align}
> 100 \left(\int_6^x \frac{1}{4x}\frac{dx}{dt} dt - \int_6^x \frac{1}{4(4+x)}\frac{dx}{dt} dt\right) &= k\int_0^{t} dt \\
> \left. \left( 25\log\left(\frac{x}{4+x}\right) + C_1 \right)\right\lvert_6^x  &= \left(kt + C_2\right)\bigg\lvert_0^{t}\\
> \left( 25\log\left(\frac{x}{4+x}\right) + \cancel{C_1} \right) - \left( 25\log\left(\frac{6}{4+6}\right) + \cancel{C_1} \right) &= (kt + \bcancel{C_2}) - (0k + \bcancel{C_2})\\
>  25\log\left(\frac{x}{4+x}\frac{10}{6}\right) &= kt\\
>  \log\left(\frac{5x}{3(x+4)}\right) &= \frac{kt}{25}\\
>  \frac{5x}{3(x+4)} &= e^{\frac{kt}{25}} \\
>  \frac{5x}{3(x+4)} &= e^{\log\left(\sqrt{\frac{5}{7}}\right)^{t/25}} \\
>  \frac{x}{x+4} &= 0.6\left(\sqrt{\frac{5}{7}}\right)^{t/25} \\
> \end{align}
> $$
> 


> It would have been possible to fully isolate $x(t)$, but it would give something really ugly and harder to look at. So we keep this formulation instead.

At $t=250$
$$
\begin{align}
\frac{x(250)}{x(250) +4} &= 0.6\left(\sqrt{\frac{5}{7}}\right)^{250/25} \\
\frac{x(250)}{x(250) +4} &= 0.6\left(\frac{5}{7}\right)^{5} \\
x(250) &= \frac{4*0.6\left(\frac{5}{7}\right)^{5}}{1-0.6\left(\frac{5}{7}\right)^{5}}  \approx 0.54816 \\
\end{align}
$$
This is the amount of undissolved sulfure, so 
$$\boxed{6-0.5481 \approx 5.4518}$$
grams of sulfure are dissolved.


```tikz
\usepackage{pgfplots}
\pgfplotsset{compat=1.16,width=10cm}
\begin{document} 
    \begin{tikzpicture}
        \begin{axis}[
            title={Undissolved Amount $x(t)$},
            axis lines=middle,
            xlabel=$t$,
            ylabel=$x(t)$,
            samples=200,
            enlargelimits,
            domain=0:400,
            draw={rgb,255:red,209;green,209;blue,209},
            text={rgb,255:red,209;green,209;blue,209},
        ]
        \newcommand\C{0.6*(5/7)^(1/50)};
        \addplot[
            ultra thick,
        ] {(4^\C^x)/(1-\C^x)};
        
        \newcommand\T{250};
        \newcommand\X{((4^\C^\T)/(1-\C^\T))}; 
        \addplot[
            mark=none,
            dashed,
        ] plot coordinates { (0,\X) (\T, \X)}
            node[above right] {$250$};
        \addplot[
            mark=none,
            dashed,
        ] plot coordinates { (\T,0)  (\T, \X)};
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```


##### Example
#example #substitution_rule
> [!Example] Statement
> In (1) take $s_1=10$, $s_2=10$, $m=1$, $n=1$. If 5 units of $C$ are formed in 10 min, determine the number of $C$ units formed in 50 min.

$$
\begin{align}
\dot{x} &= k(10-x)^2 \\
\int_0^5 \frac{1}{(10-x)^2} \frac{dx}{dt} dt &= k \int_0^{10} dt \tag{3}\\
-\frac{1}{(10-x)}\bigg\lvert_0^5  &= kt\bigg \lvert_0^{10} \\
-\left[\frac{1}{(10-5)} - \frac{1}{(10-0)}\right]  &= 10k \\
\iff k  &= -1 \\
\end{align}
$$

Changing the conditions in (3) and using $k=-1$


$$
\begin{align}
\int_0^x \frac{1}{(10-x)^2} \frac{dx}{dt} dt &= - \int_0^{50} dt \tag{3}\\
-\frac{1}{(10-x)}\bigg\lvert_0^x  &= -t\bigg \lvert_0^{50} \\
-\left[\frac{1}{(10-x)} - \frac{1}{(10-0)}\right]  &= -50 \\
\frac{x}{10-x} = 50 \\
\iff x(t) = 8.\overline{3}
\end{align}
$$


##### Example
#example 
> [!Example] Statement
> In (1) take $s_1=10$, $s_2=8$, $m=1$, $n=1$. If 5 unites of $C$ are formed in 5 min, determine the number of $C$ units formed in 10 min.

##### Example
#example 
> [!Example] Statement
> In (1) take $m=1$, $n=1$
> 



> [!Example] (a)
> Sovel for $x$ as a function of time when $s_1 \neq s_2$ and when $s_1 = s_2$



> [!Example] (b)
> Show that as $t \rightarrow \infty$, $x \rightarrow s_1$ if $s_2 \ge s_1$ and $x \rightarrow s_2$ if $s_1 \le s_2$


##### Example
#example 
> [!Example] Statement
> In a certain chemical reaction, substance A, initially weighing 12 lb, is converted into substance B. The rate at which B is formed is proportional to the amount of A *remaining*. At the end of 2.5 min, 4 lb of B have been formed.
> 
> Work this problem in two ways:
> 1. Letting $x$ represent amount of A remaining at time $t$ 
> 2. Letting $x$ represent amount of B formed at time $t$.
> 
> Chemical reactions of this type are called **first order processes**.



> [!Example] (a)
> How much of the B substance will be present after 6 min? 



> [!Example] (b)
> How much time will be required to convert 60 percent of A? 


##### Example
#example 
> [!Example] Statement
> A new substance C is to be formed from two given substances A and B by combining one unit of A with two units of B. Initially A weighs 20lb and B weighs 40lb. 




> [!Example] (a)
> If 12lb of C are formed in 1/8 hr, express express $x$ as a functuion of time in hours, where $x$ is the number of units of $C$ formed in time $t$. 



> [!Example] (b)
> What is the maximum possible value of $x$?


##### Example
#example 
> [!Example] Statement 
> A saturated solution of salt water will hold approximately 3lb of salt per gallon. A block of salt weighing 60lb is placed into a vessel containing 100 gal of water. In 5 minutes, 20lm of salt are dissolved.



> [!Example] (a)
> How much salt will be dissolved in 1hr?



> [!Example] (b)
> When will 45lb of salt be dissolved?


##### Example
#example 
> [!Example] Statement
> Five grams of a chemical $A$ are placed ina. solution of 100cc of a liquid $B$ which, when saturated, will hold 10g of $A$. If 2g of $A$ are in the solution in 1hr, how many grams of $A$ will be in the solution in 2hr?


##### Example
#example 
> [!Example] Statement
> Fifteen grams of a chemical $A$ are placed into 50cc of water, which when saturated will hold 25g of $A$. If 5g of $A$ are dissolved in 2hr, how many grams of $A$ will be dissolved in 5hr?




##### Example
#example 
> [!Example] Statement
> A substance containing 10 lb of moisture is placed in a sealed room, whose volume is 2000 cu ft and which when saturated can hold 0.015 lb of moisture per cubic foot. Initially the relative humidity of the air is 30 percent. If the substance loses 4 lb of moisture in 1 hr, how much time is required for the substance to lose 80 percent of its moisture content? Assume the substance loses moisture at a rate that is proportional to its moisture content and to the difference between the moisture content of saturated air and the moisture content of the air.

- There will be a maximum of $2000 \times 0.015=30$ lbs of moisture when fully saturated
- Starting humidity is 30%. 30% of fully saturated (that's what relative humidity means), $2000 \times 0.015 \times 0.3 = 9$ lbs already in the air
- $x_0=10$ is the starting moisture of the substance
- $x$ is the remaining moisture left in the substance
- Current amount moisture need to account for the starting humidity: $\underbrace{x_0 - x}_{\text{From substance}} + 9$

Giving
$$
\begin{align}
\dot{x} = kx\left(\frac{30}{2000} - \frac{10-x+9}{2000}\right)
\end{align}
$$

The rest is just following through the motion.

$$
\begin{align}
\dot{x}  &= kx\left(\frac{11+x}{2000}\right) \\
\dot{x} \frac{1}{x(11+x)} &= \frac{k}{2000} \\
\dot{x} \left(\frac{-1}{11(11+x)} + \frac{1}{11x}\right) &= \frac{k}{2000}
\end{align}
$$

> [!step]- [[../Partial Fraction Expansion|Partial Fraction Expansion]]
> $$
> \begin{align}
> \frac{A}{11+x} + \frac{B}{x} &= \frac{1}{(11+x)x} \\
> \frac{Ax+B(11+x)}{\cancel{(11+x)x}} &= \frac{1}{\cancel{(11+x)x}} \\
> (A+B)x+11B &= 1 \\
> \end{align}
> $$
> 
> Match like-powers
> 
> $$
> \begin{align}
> A + B &= 0 \\
> 11B &= 1
> \end{align}
> $$
> 
> Giving $A = -\frac{1}{11}$ and $B=\frac{1}{11}$
> 
> Partial fraction expansion gives
> $$\frac{-1}{11(11+x)} + \frac{1}{11x}$$

$$
\begin{align}
\int \left(\frac{-1}{11(11+x)} + \frac{1}{11x}\right) \frac{dx}{dt} dt  = \int \frac{k}{2000} dt   \\
\int \left(\frac{-1}{11(11+x)} + \frac{1}{11x}\right) dx  = \frac{k}{2000} \int dt   \\
\frac{1}{11}\left(\int \frac{-1}{(11+x)}dx + \int \frac{1}{x}dx\right)   = \frac{k}{2000} \int dt  \tag{I} \\
\end{align}
$$

From this we can plug the know conditions (bounds for the integral)

$$
\begin{align}
\frac{1}{11}\left(\int_{10}^6 \frac{-1}{(11+x)}dx + \int_{10}^6 \frac{1}{x}dx\right)   & = \frac{k}{2000} \int_0^1 dt   \\
\frac{1}{11}\left[\ln(x) - \ln(11+x) \right]_{10}^6 &= \frac{k}{2000} \\
\frac{1}{11}\left[\ln\left(\frac{x}{(11+x)}\right)\right]_{10}^6 &= \frac{k}{2000} \\
\frac{1}{11}\left[\ln\left(\frac{6}{(11+6)}\right) - \ln\left(\frac{10}{(11+10)}\right)\right] &= \frac{k}{2000} \\
\frac{1}{11} \ln\left(\frac{6}{17}\frac{21}{10}\right) &= \frac{k}{2000} \\
\iff k &= \frac{2000}{11} \ln\left(\frac{126}{170}\right) \\ 
&= \frac{2000}{11} \ln\left(\frac{63}{85}\right)
\end{align}
$$

Now that we have the value for $k$, reuse ($I$) to have the final time. 80% of the substance's moisture is gone, so 2lbs is left.

$$
\begin{align}
\frac{1}{11}\left(\int_{10}^2 \frac{1}{x}dx - \int_{10}^2 \frac{1}{(11+x)}dx\right) &= \frac{k}{2000} \int_0^t dt \\
\frac{1}{11}\left[\ln\left(\frac{x}{11+x}\right)\right]_{10}^2 &= \frac{kt}{2000}\bigg\lvert_0^t \\
\frac{1}{11}\left[\ln\left(\frac{2}{11+2}\right) - \ln\left(\frac{10}{11+10}\right)\right] &= \frac{kt}{2000} \\
\frac{1}{11}\ln\left(\frac{2}{13}\frac{21}{10}\right) &= \frac{k}{2000} \\
 \bcancel{\frac{1}{11}}\ln\left(\frac{42}{130}\right) &= \frac{\cancel{2000}}{\bcancel{11}}\frac{ \ln\left(\frac{63}{85}\right)t}{\cancel{2000}} \\
 \iff t &= \frac{\ln\left(\frac{42}{130}\right)}{ \ln\left(\frac{63}{85}\right)} \approx 3.7722954115
\end{align}
$$


> [!error]- What went wrong
> - Made a mistake when using (1.1), forgot the $x$ in $kx$
> - Signs, again...
