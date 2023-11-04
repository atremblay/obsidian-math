

## Example

### Statement
A man swims across a river 100 ft wide, always heading for a tree directly across from his starting point. He can swim at the rate of 3ft/sec.

Find the equation of his path if the current is carrying him downstream at the rate of 1ft/sec; 3ft/sec; 4ft/sec.

#### Solution


```tikz
\usepackage{pgfplots}
\pgfplotsset{compat=1.16,width=20cm,height=10cm}
% Define Nord theme colors
\definecolor{NordOrange}{HTML}{D08770}
\definecolor{NordGreen}{HTML}{A3BE8C}
\definecolor{NordPurple}{HTML}{B48EAD}

\begin{document} 
    \begin{tikzpicture}
        \newcommand\A{100};
        \begin{axis}[
            axis line style={line width=1.5pt},
            draw={rgb,255:red,209;green,209;blue,209}, 
            text={rgb,255:red,209;green,209;blue,209}, 
            fill={rgb,255:red,209;green,209;blue,209}, 
            axis lines=middle,
            samples=200,
            xmax=100,
            enlargelimits,
            declare function={
                func(\x,\myV,\myW) = ((\A/2)*((\x/\A)^(1-\myW/\myV)-(\x/\A)^(1+\myW/\myV)));
                paramy(\x) = (\x);
            }
        ]
        \addplot[
            ultra thick,
            domain=\A:0,
            draw={rgb,255:red,163;green,190;blue,140},
        ] ({func(x, 3, 1)},{paramy(x)});

        \addplot[
            ultra thick,
            domain=\A:0,
            draw=NordOrange,
        ] ({func(x, 3, 3)},{paramy(x)});

        \addplot[
            ultra thick,
            domain=\A:0,
            draw=NordPurple,
        ] ({func(x, 3, 4)},{paramy(x)});
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```
> Green: $w=1ft/sec$, Orange: $w=3ft/sec$, Purple: $w=4ft/sec$


$$
\begin{align}
y^{\prime}&=\frac{-y}{-x+k \sqrt{x^2+y^2}}\\
\left(-x+k \sqrt{x^2+(k x)^2}\right) y^{\prime}+y&=0 \\
-x\left(u^{\prime} x+u\right)+k\left(u^{\prime} x+u\right) \sqrt{x^2+u^2 x^2}+u x &= 0 \\
-u^{\prime} x^2-x u+u^{\prime} x k|x| \sqrt{1+u^2}+u k|x| \sqrt{1+u^2}+u x&=0 \\
-u^{\prime}x+u^\prime xk \sqrt{1+u^2}+u k \sqrt{1+u^2}&=0 \\
u^{\prime} x\left(k \sqrt{1+u^2}-1\right)&=-u k \sqrt{1+u^2} \\
\frac{1}{x}&=\frac{k \sqrt{1+u^2}-1}{-u k \sqrt{1+u^2}} u^{\prime} \\
&=\left(-\frac{1}{u}+\frac{1}{u k \sqrt{1-u^2}}\right) u^{\prime} \\
\ln x+C&=-\ln u+\ln \left(\sqrt[2k]{\frac{\sqrt{1+u^2}-1}{\sqrt{1+u^2}+1}}\right) \\
&=\ln \left(\frac{1}{u}\sqrt[2k]{\frac{\sqrt{1+u^2}-1}{\sqrt{1+u^2}+1}}\right)\\
Cxu&=\sqrt[2k]{\frac{\sqrt{1+u^2}-1}{\sqrt{1+u^2}+1}}\\
\end{align}
$$

This becomes an absolute mess.

$$
\frac{d x}{d y}=\frac{-v \cos \theta+\omega}{-v \sin \theta}
$$

$$\cos \theta=\frac{x}{\sqrt{x^2+y^2}}, \qquad \sin \theta=\frac{y}{\sqrt{x^2+y^2}}$$

$$
\begin{align}
\frac{d x}{d y} & =\frac{x / \sqrt{x^2+y^2}-k}{y / \sqrt{x^2+y^2}} \\
& =\frac{x-k \sqrt{x^2+y^2}}{y}
\end{align}
$$

Because of the choice of coordinates, the easiest variable substitution is [[../../Solutions/Variable Substitution in DE#3. Substitution $u(t) = frac{t(x)}{x}$|this one]]. It's the other way around from the [[#Example|previous example]].
> absolute values are sometimes used for the sake of completeness, but not required here since both $x$ and $y$ are $\ge 0$
$$u=\frac{x}{y}, \qquad x^{\prime}=u^{\prime} y+u$$

$$x^{\prime} \underbrace{y}_{Q(x,y)}-\underbrace{x+k \sqrt{x^2+y^2}}_{P(x,y)}=0$$


$$
\begin{align}
\left(u^{\prime} y+u\right) y-u y+k\sqrt{(uy)^2+y^2}&=0 \\
\left(u^{\prime} y+u\right) \cancel{y}-u \cancel{y}+k\cancel{|y|} \sqrt{u^2+1}&=0 \\
u^{\prime} y+u-u+k \sqrt{u^2+1}&=0 \\
u^\prime y&=-k \sqrt{u^2+1} \\
\frac{u^{\prime}}{k \sqrt{u^2+1}} & =\frac{1}{y} \\
\end{align}
$$

$$
\begin{aligned}
\frac{u^{\prime}}{k \sqrt{u^2+1}} & =\frac{1}{y} \\
-\frac{1}{k} \int_0^u \frac{1}{\sqrt{u^2+1}} d u & =\int_T^0 \frac{1}{y} d y \\
-\frac{1}{k}\left[\ln \left(\sqrt{u^2+1}+1\right)\right]_0^u & =[\ln y]_T^y \\
-\frac{1}{k}\left[\ln \left(\sqrt{u^2+1} \times u\right)-\ln \left(\sqrt{u^2+1}-0\right)\right] & =\ln y-\ln T \\
\frac{-1}{k} \ln \left(\sqrt{u^2+1}+u\right) & =\ln \frac{y}{T}
\end{aligned}
$$


$$
\begin{aligned}
\sqrt{u^2+1}+u & =\left(\frac{y}{T}\right)^{-k} \\
u^2+1 & =\left[\left(\frac{y}{T}\right)^{-k}-u\right]^2 \\
u^2 & =\left(\frac{y}{T}\right)^{-2 k}-2 u\left(\frac{y}{T}\right)^{-k}+u^2-1 \\
0 & =\left(\frac{y}{T}\right)^{-2 k}-2 \frac{x}{y}\left(\frac{y}{T}\right)^{-k}-1 \\
x & =\frac{\left(\left(\frac{y}{T}\right)^{-2 k}-1\right) y}{2\left(\frac{y}{T}\right)^{-k}} \\
x &=\frac{1}{2}\left(\left(\frac{y}{T}\right)^{-2k}  \left(\frac{y}{T}\right)^k y-\left(\frac{y}{T}\right)^k y\right) \\
& =\frac{1}{2}\left(\frac{y^{-2k+1+k}}{T^{-2k+k}}-\frac{y^{k+1}}{T^{k}}\right) \\
& =\frac{1}{2}\left(\frac{y^{1-k}}{T^{-k}}-\frac{y^{k+1}}{T^{k}}\right) \cdot \frac{T}{T} \\
& =\frac{T}{2}\left(\left(\frac{y}{T}\right)^{1-k}-\left(\frac{y}{T}\right)^{1+k}\right)
\end{aligned}
$$

##### Where does the boy land?

If we want to know where on the opposite bank the boy will land, we have to look at the limit as he gets closer and closer $\lim_{y \rightarrow 0}$. 

Given that $v=3ft/sec$ and $T=100$

###### Safe and sound
if $w=1ft/sec$ then $k=\frac 1 3$.

$$
\begin{align}
x& =\frac{T}{2}\left(\left(\frac{y}{T}\right)^{1-k}-\left(\frac{y}{T}\right)^{1+k}\right) \\
&=\frac{100}{2}\left(\left(\frac{y}{100}\right)^{1-\frac 1 3}-\left(\frac{y}{100}\right)^{1+\frac 1 3}\right) \\
&=50\left(\left(\frac{y}{100}\right)^{\frac 2 3}-\left(\frac{y}{100}\right)^{\frac 4 3}\right) \\
\end{align}
$$

Taking the limit,  the boy will reach the other bank ($y=0$) at
$$
\begin{align}
\lim_{y \to 0} &= 50\left(\left(\frac{y}{100}\right)^{\frac 2 3}-\left(\frac{y}{100}\right)^{\frac 4 3}\right) \\
&= 50\left(\lim_{y \to 0} \left(\frac{y}{100}\right)^{\frac 2 3}-\lim_{y \to 0} \left(\frac{y}{100}\right)^{\frac 4 3}\right) \\
&= 50(0-0) \\
&= 0
\end{align}
$$

###### Barely, missed the target
If $w=3ft/sec$ the he will reach the other bank at $x=50$
$$
\begin{align}
x& =\frac{T}{2}\left(\left(\frac{y}{T}\right)^{1-k}-\left(\frac{y}{T}\right)^{1+k}\right) \\
&=\frac{100}{2}\left(\left(\frac{y}{100}\right)^{1-\frac 3 3}-\left(\frac{y}{100}\right)^{1+\frac 3 3}\right) \\
&=50\left(\left(\frac{y}{100}\right)^{0}-\left(\frac{y}{100}\right)^{2}\right) \\
\end{align}
$$

$$
\begin{align}
\lim_{y \to 0} &= 50\left(\left(\frac{y}{100}\right)^{0}-\left(\frac{y}{100}\right)^{2}\right) \\
 &= 50\left(\lim_{y \to 0}\left(\frac{y}{100}\right)^{0}-\lim_{y \to 0}\left(\frac{y}{100}\right)^{2}\right) \\
&= 50\left(1-0\right) \\
&= 50 
\end{align}
$$

###### Bye-bye
If $w=4ft/sec$ then it gets more interesting.

$$
\begin{align}
x& =\frac{T}{2}\left(\left(\frac{y}{T}\right)^{1-k}-\left(\frac{y}{T}\right)^{1+k}\right) \\
&=\frac{100}{2}\left(\left(\frac{y}{100}\right)^{1-\frac 4 3}-\left(\frac{y}{100}\right)^{1+\frac 4 3}\right) \\
&=\frac{100}{2}\left(\left(\frac{y}{100}\right)^{-\frac 1 3}-\left(\frac{y}{100}\right)^{\frac 7 3}\right) \\
\end{align}
$$

The boy will never reach the other side.
$$
\begin{align}
\lim_{y \to 0} &=\frac{100}{2}\left(\left(\frac{y}{100}\right)^{-\frac 1 3}-\left(\frac{y}{100}\right)^{\frac 7 3}\right) \\
&=\frac{100}{2}\left(\lim_{y \to 0}\left(\frac{y}{100}\right)^{-\frac 1 3}-\lim_{y \to 0}\left(\frac{y}{100}\right)^{\frac 7 3}\right) \\
&=50\left(\left(\frac 1 {100}\right)^{-\frac 1 3} \lim_{y \to 0}\left(y\right)^{-\frac 1 3}-\left(\frac 1 {100}\right)^{\frac 7 3}\lim_{y \to 0}\left(y\right)^{\frac 7 3}\right) \\
&=50\left(\left(\frac 1 {100}\right)^{-\frac 1 3} \lim_{y \to 0}\left(\frac 1 y\right)^{\frac 1 3}-\left(\frac 1 {100}\right)^{\frac 7 3}\lim_{y \to 0}\left(y\right)^{\frac 7 3}\right) \\
&=50\left(\left(\frac 1 {100}\right)^{-\frac 1 3}\infty-0\right) \\
&=\infty \\
\end{align}
$$


## Example

### Statement
Solve previous problem by using polar coordinates. 

