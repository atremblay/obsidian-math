
$$
\frac{dT_B}{dt} = -k(T_B-T_M), \hspace{1em} T_B = Ce^{-kt} + T_M
$$
where 
- $k>0$ is a proportionality constant
- $T_B$ is the tempearture of the body at any time $t$
- $T_M$ is the **constant** temperature of the medium
- $C=T_0-T_m$ 
    -  $T_0$ is the initial temperature of the body

Using [[../Solutions/Separation of variables|separation of variables]]

$$
\begin{align}
\frac{dT_B}{dt} &= -k(T_B-T_M) \\
\frac{1}{(T_B-T_M)}\frac{dT_B}{dt} &= -k \\
\int \frac{1}{(T_B-T_M)}\frac{dT_B}{dt} dt &= \int -k dt\\
\int \frac{1}{(T_B-T_M)}dT_B &= \int -k dt\\
\log (T_B-T_M) &= -kt + C_1\\
T_B &= Ce^{-kt} + T_M\\
\end{align}
$$
And at time $t=0$
$$
\begin{align}
T_0 &= Ce^{0} + T_M\\
\iff C &= T_0 - T_M
\end{align}
$$
##### Example
#example 
> [!Example] Statement
> A body whose temperature is 100º is placed in a medium which is kept at a constant temperature of 20º.  In 10 minutes the temperature of the body falls to 60º.

- $T_0=100º$
- $T_M=20º$

> [!Example] (a)
> Find the temparture $T$ of the body as a function of the time $t$

$$
\begin{align}
T_B &= 80e^{-kt} + 20 \\
60 &= 80e^{-10k} + 20 \\
k &= -\frac{\ln(0.5)}{10} 
\end{align}
$$

$$
T_B = 80e^{\frac{\ln(0.5)}{10}t} + 20
$$


> [!Example] (b)
> Find the temparture of the body after 40 minutes


```tikz 
\usepackage{pgfplots}
\pgfplotsset{compat=1.16}
\begin{document} 
    \begin{tikzpicture}[scale=1.5]    
        \newcommand\T{40};
        \newcommand\K{-ln(0.5)/10}
        \newcommand\X{(80*exp(-\K*\T)+20)}; 
    \begin{axis}[
    title=$80*e^{\frac{40ln(0.5)}{10}}+20$,
        axis lines=middle,
        xlabel=$t$,
        ylabel=$x(t)$,
        enlargelimits,
    ]
        \addplot[
            domain=0:60, 
            thick,
            samples=200
        ] {(80*exp(x*ln(0.5)/10)+20)};

        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (0,\X) (\T, \X)}
            node[above right] {$25$};
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (\T,0)  (\T, \X)};
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```

> [!Example] (c)
> When will the body's temperature be 50º

$$
\begin{align}
50 &= 80e^{\frac{\log(0.5)}{10}t} + 20 \\
\iff t &= \frac{10}{\ln(0.5)}\ln\left(\frac{3}{8}\right) \approx 14.15
\end{align}
$$


---

##### Example
#example 
> [!Example] Statement
> The temparture of a body differs from that of a medium, whose temparature is kept constant, by 40º. In 5 minutes, this difference is 20º.

$$
\begin{align}
T_B &= Ce^{-kt} + T_M \\
& = (T_0-T_M)e^{-kt} + T_M \\
&= 40e^{-kt} + T_M
\end{align}
$$


> [!Example] (a)
> What is the value of $k$?

@$t=5$
$$
\begin{align}
T_B&= 40e^{-5k} + T_M \\
T_B - T_M = 20 &= 40e^{-5k}\\
\iff k=-\frac 1 5 \ln(0.5) \approx 0.1386294361
\end{align}
$$


> [!Example] (b)
> In how many minutes will the difference in temperature be 10º?

$$
\begin{align}
T_B&= 40e^{-kt} + T_M \\
T_B - T_M = 10 &= 40e^{-kt}\\
\iff t&=\frac{5\ln(0.25)}{\ln(0.5)} =10
\end{align}
$$


---

##### Example
#example 
> [!Example] Statement
> A body whose temperature is 20º is placed in a medium which is kept at a constant temperature of 60º. In 5 minutes the body's temperature has risen to 30º.
> 


> [!Example] (a)
> Find the body's temperature after 20 minutes


$T_0 = 20$, $T_M=60$

Initial setup
$$
\begin{align}
T_B &= (20-60)e^{-kt} + 60\\
&= -40e^{-kt} + 60\\
\end{align}
$$
Need to find $k$ with the additionnal information that at $t=5$, $T_5=30$

$$
\begin{align}
T_5=30 &= -40e^{-5k} + 60\\
\iff k &= -\frac 1 5 \ln\frac 3 4 = \ln\sqrt[5]{\frac 4 3} \approx 0.05753641449\\
\end{align}
$$

At $t=20$
$$
\begin{align}
T_{20} &= -40e^{-20\ln\sqrt[5]{\frac 4 3}} + 60\\
&\approx 47.34375\\
\end{align}
$$

> [!Example] (b)
> When will the body's temperature be 40º?

$$
\begin{align}
40 &= -40e^{-kt} + 60\\
\iff t &= -\frac 1 k \ln 0.5 \approx 12.04710419827\\
\end{align}
$$


```tikz 
\usepackage{pgfplots}
\pgfplotsset{compat=1.16}
\begin{document} 
    \begin{tikzpicture}[scale=1.5]    
        \newcommand\T{20};
        \newcommand\K{ln((4/3)^0.2)}
        \newcommand\X{(-40*exp(-\K*\T)+60)}; 
        \newcommand\Xp{40};
        \newcommand\Tp{-ln(0.5)/\K}
    \begin{axis}[
        axis lines=middle,
        xlabel=$t$,
        ylabel=$x(t)$,
        enlargelimits,
    ]
        \addplot[
            domain=0:30, 
            thick,
            samples=200
        ] {(-40*exp(-x*\K)+60)};
        \addplot[
            mark=none,
            orange,
            dashed,
        ] plot coordinates { (0,\X) (\T, \X)};
        \addplot[
            mark=none,
            orange,
            dashed,
        ] plot coordinates { (\T,0)  (\T, \X)}
            node[above]{(a)};
        \addplot[
            mark=none,
            green,
            dashed,
        ] plot coordinates { (0,\Xp) (\Tp, \Xp)};
        \addplot[
            mark=none,
            green,
            dashed,
        ] plot coordinates { (\Tp,0)  (\Tp, \Xp)}
            node[above]{(b)};
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```



---

##### Example
#example 
> [!Example] Statement
> The temperature in a room is 70ºF. A thermometer which has been kept in it is placed outside. In 5 minutes the thermometer reading is 60ºF. Five minutes later, is is 55ºF. Find the outdoor temperature.

$T_M$ unknown, $T_0=70$, $T_5=60$, $T_{10}=55$

$$
\begin{align}
T_B &= (T_0-T_M)e^{-kt} + T_M\\
T_0 = 70 &= (70-T_M) + T_M\\
T_5 = 60 &= (70-T_M)e^{-5k} + T_M\\
T_{10} = 55 &= (70-T_M)e^{-10k} + T_M\\
\end{align}
$$
Two equations, two unknowns

$$
\begin{align}
T_5 = 60 &= 70e^{-5k} + (1-e^{-5k})T_M\\
T_{10} = 55 &= 70e^{-10k} + (1-e^{-10k})T_M\\
\end{align}
$$
$$
\begin{align}
T_M = \frac{60-70e^{-5k}}{1-e^{-5k}} &= \frac{55-70e^{-10k}}{1-e^{-10k}} \\
(60-70e^{-5k})(1-e^{-10k}) &= (1-e^{-5k})(55-70e^{-10k}) \\
60-60e^{-10k}-70e^{-5k} + \cancel{70e^{-5k}e^{-10k}}&= 55-70e^{-10k} -55e^{-5k} + \cancel{70e^{-10k}e^{-5k}} \\
60-60e^{-10k}-70e^{-5k} &= 55-70e^{-10k} -55e^{-5k}  \\
5+10e^{-10k}-15e^{-5k} &= 0  \\
2e^{-5k}-3 &= -\frac 1 {e^{-5k}}  \\
\end{align}
$$

Wrong way! Should isolate $k$ first instead
$$
\begin{align}
T_5 = 60 &= (70-T_M)e^{-5k} + T_M \iff k= -\frac 1 5 \ln\left(\frac{60-T_M}{70-T_M}\right)\\
T_{10} = 55 &= (70-T_M)e^{-10k} + T_M \iff k= -\frac 1 {10} \ln\left(\frac{55-T_M}{70-T_M}\right)\\
\end{align}
$$
$$
\begin{align}
k= -\frac 1 5 \ln\left(\frac{60-T_M}{70-T_M}\right)&= -\frac 1 {10} \ln\left(\frac{55-T_M}{70-T_M}\right)\\
2\ln\left(\frac{60-T_M}{70-T_M}\right)&= \ln\left(\frac{55-T_M}{70-T_M}\right)\\
\ln\left(\frac{(60-T_M)^2}{(70-T_M)^2}\right)&= \ln\left(\frac{55-T_M}{70-T_M}\right)\\
\frac{(60-T_M)^2}{(70-T_M)^2}&= \frac{55-T_M}{70-T_M}\\
(60-T_M)^2&= (55-T_M)(70-T_M)\\
3600-120T_M+\cancel{T_M^2}&= 55*70 - 55T_M - 70T_M + \cancel{T_M^2}\\
\implies T_M = 50
\end{align}
$$

> [!Error]- What did I do wrong?
> Should have realized that I did not need to find the value of $k$. Isolate $k$ first, then we are left with only $T_M$.

---
The following examples require this bit of information

> The **specific heat** of a substance is defined as the ratio of the quantity of heat (e.g. calorie) required to raise a unit weight of the substance 1º to the quantity of heat required to raise the same unit weight of water 1º.
> For example, it takes 1 calorie to change the temperature of 1 gram of water 1ºC. If, therefore, it takes only 0.1 of a calorie to change the temparture of 1 gram of a substance 1ºC, then the specific heat of the substance is 0.1

##### Example
#example 
> [!Example] Statement
> A 50-lbs iron ball is heated to 200ºF and is then immediately plunged into a vessel containing 100lb of water whose tempareture is 40ºF. The specific heat of iron is 0.11.




> [!Example] (a)
> Find the temperature of the body as a function of time.




> [!Example] (b)
> Find the common temperature approached by body and water as $t \rightarrow \infty$ 



---
##### Example
#example 
> [!Example] Statement
> The specific heat of tin is 0.05. A 10-lb body of tin, whose temperature is 100ºF, is plunged into a vessel containing 50lb of water at 10ºF.



> [!Example] (a)
> Find the temperature $T$ of the tin as a function of time



> [!Example] (b)
> Find the common temperature approached by tin and water as $t \rightarrow \infty$


---

##### Example
#example 
> [!Example] Statement
> The temperature of a 100-lbs body whose specific heat is 0.1 is 20ºF. It is pluged into a 40-lb liquid whose specific heat is 0.5 and whose temperature is 50º. 



> [!Example] (a)
> Find the temperature $T$ of the body as a function of time




> [!Example] (b)
> To what temperature will the body eventually cool?






