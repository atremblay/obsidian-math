## Example

^07afa4

#example 
### Statement

A pilot always keeps the nose of his plane pointed toward a city T due west of his starting point. If his speed is o miles per hour and a wind is blowing from the south at the rate of $w$ miles per hour, find the equation of the plane's path. Assume that it starts from a flying field which is at a distance $a$ miles from $T$.

### Solution

```tikz
\usepackage{pgfplots}
\usetikzlibrary{calc}
\pgfplotsset{compat=1.16,width=15cm,height=10cm}
\begin{document} 
    \begin{tikzpicture}
        \newcommand\A{2000};
        \newcommand\W{100};
        \newcommand\V{500};
        \newcommand\K{\W/\V};
        \newcommand\X{1000};
        \newcommand\prop{0.6};
        \newcommand\cprop{0.4}; 
        \begin{axis}[
            axis line style={line width=1.5pt},
            draw={rgb,255:red,209;green,209;blue,209}, 
            text={rgb,255:red,209;green,209;blue,209}, 
            fill={rgb,255:red,209;green,209;blue,209}, 
            axis lines=middle,
            samples=200,
            enlargelimits,
            ticks=none,
            declare function={
                func(\x)=((\A/2)*((\x/\A)^(1-\K)- (\x/\A)^(1+\K));
                derivative(\x)=(
                    (1/2)*( 
                        (1-\K)*( 
                            (\x/\A)^(-\K) 
                        ) - 
                        (1+\K)*(
                            (\x/\A)^(\K)
                        )
                    )
                );
            }
        ]
        \pgfmathparse{\X+0.8};
        \pgfmathsetmacro\slope{derivative(\X)};
        \pgfmathsetmacro\Y{func(\X)};
        \pgfmathsetmacro\angle{atan(\Y/\X)};
        
        \pgfmathsetmacro\vstartx{\X};
        \pgfmathsetmacro\vstarty{\Y};
        \pgfmathsetmacro\vendx{\X*\prop};
        \pgfmathsetmacro\vendy{func(\X)*\prop};

        \pgfmathsetmacro\dstartx{\X};
        \pgfmathsetmacro\dstarty{\Y};
        \pgfmathsetmacro\dendx{\X*\prop};
        \pgfmathsetmacro\dendy{\Y-\X*\slope*\cprop};

        \pgfmathsetmacro\wstartx{\X};
        \pgfmathsetmacro\wstarty{\Y};
        \pgfmathsetmacro\wendx{\X};
        \pgfmathsetmacro\wendy{\Y + (\dendy - \vendy)};
        
        \addplot[
            ultra thick,
            domain=\A:650,
            draw={rgb,255:red,163;green,190;blue,140},
        ] {func(x)}
            node [
                pos=0, 
                below, 
                font=\LARGE,
            ] {$\left(a, 0\right)$}
            node [pos=0.25, above right, font=\Huge] {$\frac{w}{v}=\frac 1 2$};

        \node at (0,0) [font=\LARGE, below right] {$T(0,0)$};
        
        \addplot[] 
            coordinates {(0,0) (\X, {func(\X)})}
            node [above right, font=\LARGE] {$P(x,y)$};
        
        \addplot[]  
            coordinates {(\X,0) (\X, {func(\X)})}
            node [pos=0.4, right, font=\Large] {$y$};

        \addplot[]  
            coordinates {(0,0) (\X, 0)}
            node [pos=0.5, below, font=\Large] {$x$};

        % x component of the plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt, 
        ] 
            coordinates {(\X,{func(\X)*\prop}) (\X*\prop, {func(\X)*\prop})}  
            node [pos=0.5, below] {$v \cos(\theta)$};
        % y component of the plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt
        ] 
            coordinates {(\X,\Y) (\X, {\Y*\prop})}  
            node [rotate=90, below right] {$v \sin(\theta)$};
        % plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\vstartx, \vstarty) (\X*\prop, {func(\X)*\prop})} 
            node [pos=0.5, font=\Large, above left] {$\mathbf{v}$};
        % actual direction of the plane
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\dstartx, \dstarty) (\dendx, \dendy)} 
            node [above left,align=center] {Actual direction \\ of the plane};
        
        \addplot[
            -,
            dashed, 
            line width=1.5pt,
        ] 
            coordinates {
                (\dstartx, \dstarty) 
                (\dstartx + (\dstartx - \dendx), \dstarty+\X*\cprop*\slope)
            } ;
            
        % Direction of the wind
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\wstartx, \wstarty) (\wendx, \wendy)} 
            node [pos=0.5, font=\Large, right] {$\mathbf{w}$};
        
        \addplot [-, thick] coordinates {(\dendx,\dendy) (\wendx, \wendy)} ;

        \addplot [-, thick] 
            coordinates {(\X*\prop,{\Y*\prop}) (\X*\prop, {\Y-\X*\slope*\cprop})};

        \draw[->, line width=1.1pt] (rel axis cs:0.8,0.71) -- (rel axis cs:0.8,0.8) 
        node[left,pos=0.5, font=\Large] {$+$};
        \draw[->, line width=1.1pt] (rel axis cs:0.81,0.7) -- (rel axis cs:0.9,0.7) 
        node[below,pos=0.5, font=\Large] {$+$};

        \coordinate (A) at (axis cs:0,0); 
        \coordinate (B) at (axis cs:\X,\Y); 
        \draw[thick] 
        let 
            \p1 = (B), 
            \n1 = {atan2(\y1,\x1)},
    
        in (A) ++(0:1.5cm) arc[start angle=0, end angle=\n1, radius=1.5cm] 
            node[midway, right, font=\Large] { \( \theta \) };

        \coordinate (C) at (axis cs:\vendx,\vendy); 
        \draw[thick] 
        let 
            \p1 = (B), 
            \n1 = {atan2(\y1,\x1)},
        in (C) ++(0:1cm) arc[start angle=0, end angle=\n1, radius=1cm] 
            node[midway, right, font=\Large] { \( \theta \) };
        
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```

> Schema of the plane trajectory
> Let 
> - $P(x, y)$ be the position of the plane at any time $t$. 
> - The vector representing its speed has magnitude $v$ and is pointed toward $T$. 
> - Call $\theta$ the angle this vector makes with the horizontal line connecting the flying field and $T$. 
> - The wind vector points due north and has a magnitude $w$. 

The diagonal of the parallelogram formed by the vectors $\mathbf{v}$ and $\mathbf{w}$ represents the actual direction and magnitude of the plane's velocity at time $t$. Since the direction is changing instantaneously, it must be tangent to the path of the plane at $P$. It must therefore be the slope $dy / dx$ of the desired curve. Our problem then is to find an equation which expresses $dy / dx$ as a function of $x$ and $y$.

The respective components of the airplane's velocity in the $x$ and $y$ directions are

$$
\frac{d x}{d t}=-v \cos \theta, \quad \frac{d y}{d t}=-v \sin \theta \tag{a}
$$

^6ef307

Hence the effective velocity of the plane in the $y$ direction, taking the wind's velocity into account, is

$$
\frac{d y}{d t}=-v \sin \theta+w \tag{b}
$$

From the schema, we see that

$$
\sin \theta=\frac{y}{\sqrt{x^{2}+y^{2}}}, \quad \cos \theta=\frac{x}{\sqrt{x^{2}+y^{2}}} . \tag{c}
$$

Substituting these values in the first equation of (a) and in (b) we obtain

$$
\frac{d x}{d t}=\frac{-v x}{\sqrt{x^{2}+y^{2}}}, \quad \frac{d y}{d t}=-\frac{v y}{\sqrt{x^{2}+y^{2}}}+w . \tag{d}
$$

Division of the second equation in (d) by the first gives

$$
\frac{d y}{d x}=\frac{-v y+w \sqrt{x^{2}+y^{2}}}{-v x}=\frac{y-\frac{w}{v} \sqrt{x^{2}+y^{2}}}{x} . \tag{e}
$$

For convenience, we let

$$
k=\frac{w}{v}, \tag{f}
$$

so that $k$ represents the ratio of the speeds of wind and plane. Hence (e) now becomes

$$
x \frac{dy}{dx}=y-k \sqrt{x^{2}+y^{2}} \tag{g}
$$

This equation is of the [[../../Definitions/Homogeneous Function|homogeneous type]]. It can therefore be solved by [[../../Solutions/Variable Substitution in DE|Variable Substitution in DE]]. Note that $x,y\ge 0$ since we are working with positive coordinates.
$$
u=\frac y x, \qquad y^\prime=u^\prime x + u
$$
$$
\begin{align}
\underbrace{k \sqrt{x^{2}+y^{2}}-y}_{P(x,y)}+\underbrace{x}_{Q(x,y)} y^\prime=0 \\
kx \sqrt{1+\left(\frac{y}{x}\right)^2}-u x+x\left(u^{\prime} x+u\right)=0 \tag{h}\\
\end{align}
$$

Replacing $\frac y x$ by $u$ in (h)
$$
\begin{align}
k \sqrt{1+u^2}-u+u^{\prime} x+u&=0 \\
u^{\prime} x&=-k \sqrt{1+u^2} \\
x&=-\frac{k \sqrt{1+u^2}}{u^{\prime}} \\
\frac{1}{x}&=-\frac{1}{k \sqrt{1+u^2}} \frac{d u}{d x} \\
-k\int \frac{1}{x} d x&=\int \frac{1}{\sqrt{1+u^2}} \frac{d u}{d x} d x \\
-k\int \frac{1}{x} d x&= \int \frac{1}{\sqrt{1+u^2}} d u \tag{i}\\
\end{align}
$$
Taking the definite integral(i) starting at $x=a$ 
$$
\begin{align}
-k \int_a^x \frac{1}{x} dx&=\int_0^u \frac{1}{\sqrt{1+u^2}} d u \tag{j}\\
\end{align}
$$

Using the [[../../../Calculus/Integration/Cheatsheet#^9a7c4a|integral]] on (j)
$$
\begin{align}
-k[\log (x)-\log (a)]&=\left[\log \left(u+\sqrt{1+u^2}\right)-\log\left(0+\sqrt{1+0^2}\right)\right] \\
-k \log \left(\frac{x}{a}\right)&=\log \left(u+\sqrt{1+u^2}\right) \tag{k} \\
\end{align}
$$

^5d2bb9

Simplifying (k) a bunch

$$
\begin{align}
\left(\frac{x}{a}\right)^{-k}&=u+\sqrt{1+u^2} \\
\left(\left(\frac{x}{a}\right)^{-k}-\frac{y}{x}\right)^2&=1+\left(\frac{y}{x}\right)^2 \\
\cancel{\left(\frac{y}{x}\right)^2}-\frac{2 y}{x}\left(\frac{x}{a}\right)^{-k}+\left(\frac{y}{x}\right)^{-2 k}&=1+\cancel{\left(\frac{y}{x}\right)^2} \\
\frac{x}{2}\left(\frac{x}{a}\right)^{k}\left[\left(\frac{x}{a}\right)^{-2k}-1\right]&=y \\
\frac{a}{a} \frac{x}{2}\left[\left(\frac{x}{a}\right)^{-k}-\left(\frac{x}{a}\right)^{k}\right]&=y \\
\frac{a}{2}\left[\left(\frac{x}{a}\right)^{1-k}-\left(\frac{x}{a}\right)^{1+k}\right]&=y \tag{m}
\end{align}
$$

> [!step]- Alternative step
> We can use the indefinite integral and the initial condition to find (m) again
> $$
> \begin{align}
> -k \int \frac{1}{x} d x&=\int \frac{1}{\sqrt{1+u^2}} du \\
> -k \log (x)+C&=\log \left(u+\sqrt{1+u^2}\right) \\
> C x^{-k}&=u+\sqrt{1+u^2}  \\
> \left(Cx^{\cdot k}-u\right)^2&=1+u^2 \\
> y^2-2 Cu x^{-k}+c^2 x^{-2 k}&=1+u^2 \\
> C^2 x^{2 k}-1=2 C\frac{y}{x} x^{-k}&=2C y x^{-(k+1)} \\
> \frac{\left(C^2 x^{-2 k}-1\right) x^{k+1}}{2C}&=y \\
> \end{align}
> $$
> 
> Using the initial condition that at $x=a, y=0$, we can find the value of $C$
> $$
> \begin{align}
> \frac{\left(C^2 a^{-2 k}-1\right) a^{k+1}}{2C}&=0 \\
> C^2 a^{-2 k+k-1}-a^{k+1}&=0 \\
> C^2&=\frac{a^{k-1}}{a^{-k+1}}=a^{2k} \\
> C&= a^k
> \end{align}
> $$
> Plugging back the value of $C$, we end up with (m)
> $$
> \begin{aligned}
> y& =\frac{a}{2}\left[\left(\frac{x}{a}\right)^{1-k}-\left(\frac{x}{a}\right)^{1+k}\right] \\
> \end{aligned}
> $$

Replace $k$ with its value set in (f), we have finally
$$
y=\frac{a}{2}\left[\left(\frac{x}{a}\right)^{1-(w / v)}-\left(\frac{x}{a}\right)^{1+(w / v)}\right] \tag{n}
$$



as the equation of the path.


Equation (n) enables us to obtain interesting conclusions in regard to the path of the plane.


##### Case 1. 
Wind speed $w=$ plane speed $v$. In this case (n) becomes

$$
y=\frac{a}{2}\left(1-\frac{x^{2}}{a^{2}}\right), \quad x^{2}=-2 a\left(y-\frac{a}{2}\right), \tag{o}
$$

which is the equation of a parabola, Fig. 17.12. Note that the plane will never reach its destination $T$.

```tikz
\usepackage{pgfplots}
\usetikzlibrary{calc}
\pgfplotsset{compat=1.16,width=20cm,height=10cm}
\begin{document} 
    \begin{tikzpicture}
        \newcommand\A{2000};
        \newcommand\W{100};
        \newcommand\V{100};
        \newcommand\K{\W/\V};
        \newcommand\X{1000};
        \newcommand\prop{0.6};
        \newcommand\cprop{0.4}; 
        \begin{axis}[
            axis line style={line width=1.5pt},
            draw={rgb,255:red,209;green,209;blue,209}, 
            text={rgb,255:red,209;green,209;blue,209}, 
            fill={rgb,255:red,209;green,209;blue,209}, 
            axis lines=middle,
            samples=200,
            enlargelimits,
            ticks=none,
            declare function={
                func(\x)=((\A/2)*((\x/\A)^(1-\K)- (\x/\A)^(1+\K));
                derivative(\x)=(
                    (1/2)*( 
                        (1-\K)*( 
                            (\x/\A)^(-\K) 
                        ) - 
                        (1+\K)*(
                            (\x/\A)^(\K)
                        )
                    )
                );
            }
        ]
        \pgfmathparse{\X+0.8};
        \pgfmathsetmacro\slope{derivative(\X)};
        \pgfmathsetmacro\Y{func(\X)};
        \pgfmathsetmacro\angle{atan(\Y/\X)};
        
        \pgfmathsetmacro\vstartx{\X};
        \pgfmathsetmacro\vstarty{\Y};
        \pgfmathsetmacro\vendx{\X*\prop};
        \pgfmathsetmacro\vendy{func(\X)*\prop};

        \pgfmathsetmacro\dstartx{\X};
        \pgfmathsetmacro\dstarty{\Y};
        \pgfmathsetmacro\dendx{\X*\prop};
        \pgfmathsetmacro\dendy{\Y-\X*\slope*\cprop};

        \pgfmathsetmacro\wstartx{\X};
        \pgfmathsetmacro\wstarty{\Y};
        \pgfmathsetmacro\wendx{\X};
        \pgfmathsetmacro\wendy{\Y + (\dendy - \vendy)};
        
        \addplot[
            ultra thick,
            domain=\A:0,
            draw={rgb,255:red,163;green,190;blue,140},
        ] {func(x)}
            node [
                pos=0, 
                below, 
                font=\LARGE,
            ] {$\left(a, 0\right)$}
           ;

        \addplot[
            ultra thick,
            dashed,
            domain=0:-\A,
            draw={rgb,255:red,163;green,190;blue,140},
        ] {func(x)}
            node [
                pos=0, 
                above left, 
                font=\LARGE,
            ] {$\left(0, \frac{a}{2}\right)$}
            node [pos=0.75, above left, font=\Huge] {$\frac{w}{v}=1$};
            
        \node at (0,0) [font=\LARGE, below left] {$T(0,0)$};
        
        \addplot[] 
            coordinates {(0,0) (\X, {func(\X)})}
            node [above right, font=\LARGE] {$P(x,y)$};
        
        \addplot[]  
            coordinates {(\X,0) (\X, {func(\X)})}
            node [pos=0.4, right, font=\Large] {$y$};

        \addplot[]  
            coordinates {(0,0) (\X, 0)}
            node [pos=0.5, below, font=\Large] {$x$};

        % x component of the plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt, 
        ] 
            coordinates {(\X,{func(\X)*\prop}) (\X*\prop, {func(\X)*\prop})}  
            node [pos=0.5, below] {$v \cos(\theta)$};
        % y component of the plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt
        ] 
            coordinates {(\X,\Y) (\X, {\Y*\prop})}  
            node [rotate=90, below right] {$v \sin(\theta)$};
        % plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\vstartx, \vstarty) (\X*\prop, {func(\X)*\prop})} 
            node [pos=0.5, font=\Large, above left] {$\mathbf{v}$};
        % actual direction of the plane
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\dstartx, \dstarty) (\dendx, \dendy)} 
            node [above,align=center] {Actual direction \\ of the plane};
        
        \addplot[
            -,
            dashed, 
            line width=1.5pt,
        ] 
            coordinates {
                (\dstartx, \dstarty) 
                (\dstartx + (\dstartx - \dendx), \dstarty+\X*\cprop*\slope)
            } ;
            
        % Direction of the wind
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\wstartx, \wstarty) (\wendx, \wendy)} 
            node [pos=0.5, font=\Large, right] {$\mathbf{w}$};
        
        \addplot [-, thick] coordinates {(\dendx,\dendy) (\wendx, \wendy)} ;

        \addplot [-, thick] 
            coordinates {(\X*\prop,{\Y*\prop}) (\X*\prop, {\Y-\X*\slope*\cprop})};

        \draw[->, line width=1.1pt] (rel axis cs:0.9,0.81) -- (rel axis cs:0.9,0.9) 
        node[left,pos=0.5, font=\Large] {$+$};
        \draw[->, line width=1.1pt] (rel axis cs:0.91,0.8) -- (rel axis cs:1,0.8) 
        node[below,pos=0.5, font=\Large] {$+$};

        \coordinate (A) at (axis cs:0,0); 
        \coordinate (B) at (axis cs:\X,\Y); 
        \draw[thick] 
        let 
            \p1 = ($(B)-(A)$), 
            \n1 = {atan2(\y1,\x1)},
    
        in (A) ++(0:1.5cm) arc[start angle=0, end angle=\n1, radius=1.5cm] 
            node[midway, right, font=\Large] {$\theta$};

        \coordinate (C) at (axis cs:\vendx,\vendy); 
        \draw[thick] 
        let 
            \p1 =  ($(B)-(A)$), 
            \n1 = {atan2(\y1,\x1)},

        in (C) ++(0:1cm) arc[start angle=0, end angle=\n1, radius=1cm] 
            node[midway, right, font=\Large] { \( \theta \) };
        
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```


##### Case 2. 
Wind speed $w>$ plane speed $v$. In this case $w / v>1$ so that $1-(w / v)<0$. Hence as $x \rightarrow 0,\left(\frac{x}{a}\right)^{1-(w / v)}\left[=\left(\frac{a}{x}\right)^{(w / v)-1}\right] \rightarrow \infty$. We see from (n), therefore, that as $x \rightarrow 0, y \rightarrow \infty$. Again the plane will never reach $T$. A rough graph of its path is shown in Fig. 17.13 with $w=2 v$. 


```tikz
\usepackage{pgfplots}
\usetikzlibrary{calc}
\pgfplotsset{compat=1.16,width=20cm,height=10cm}
\begin{document} 
    \begin{tikzpicture}
        \newcommand\A{2000};
        \newcommand\W{200};
        \newcommand\V{100};
        \newcommand\K{\W/\V};
        \newcommand\X{600};
        \newcommand\prop{0.6};
        \newcommand\cprop{0.4}; 
        \begin{axis}[
            axis line style={line width=1.5pt},
            draw={rgb,255:red,209;green,209;blue,209}, 
            text={rgb,255:red,209;green,209;blue,209}, 
            fill={rgb,255:red,209;green,209;blue,209}, 
            axis lines=middle,
            samples=200,
            enlargelimits,
            ticks=none,
            declare function={
                func(\x)=((\A/2)*((\x/\A)^(1-\K)- (\x/\A)^(1+\K));
                derivative(\x)=(
                    (1/2)*( 
                        (1-\K)*( 
                            (\x/\A)^(-\K) 
                        ) - 
                        (1+\K)*(
                            (\x/\A)^(\K)
                        )
                    )
                );
            }
        ]
        \pgfmathparse{\X+0.8};
        \pgfmathsetmacro\slope{derivative(\X)};
        \pgfmathsetmacro\Y{func(\X)};
        \pgfmathsetmacro\angle{atan(\Y/\X)};
        
        \pgfmathsetmacro\vstartx{\X};
        \pgfmathsetmacro\vstarty{\Y};
        \pgfmathsetmacro\vendx{\X*\prop};
        \pgfmathsetmacro\vendy{func(\X)*\prop};

        \pgfmathsetmacro\dstartx{\X};
        \pgfmathsetmacro\dstarty{\Y};
        \pgfmathsetmacro\dendx{\X*\prop};
        \pgfmathsetmacro\dendy{\Y-\X*\slope*\cprop};

        \pgfmathsetmacro\wstartx{\X};
        \pgfmathsetmacro\wstarty{\Y};
        \pgfmathsetmacro\wendx{\X};
        \pgfmathsetmacro\wendy{\Y + (\dendy - \vendy)};
        
        \addplot[
            ultra thick,
            domain=\A:300,
            draw={rgb,255:red,163;green,190;blue,140},
        ] {func(x)}
            node [
                pos=0, 
                below, 
                font=\LARGE,
            ] {$\left(a, 0\right)$}
            node [pos=0.05, above right, font=\Huge] {$\frac{w}{v}=2$};
           

        \node at (0,0) [font=\LARGE, below right] {$T(0,0)$};
        
        \addplot[] 
            coordinates {(0,0) (\X, {func(\X)})}
            node [above right, font=\LARGE] {$P(x,y)$};
        
        \addplot[]  
            coordinates {(\X,0) (\X, {func(\X)})}
            node [pos=0.4, right, font=\Large] {$y$};

        \addplot[]  
            coordinates {(0,0) (\X, 0)}
            node [pos=0.5, below, font=\Large] {$x$};

        % x component of the plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt, 
        ] 
            coordinates {(\X,{func(\X)*\prop}) (\X*\prop, {func(\X)*\prop})}  
            node [pos=0.5, below] {$v \cos(\theta)$};
        % y component of the plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt
        ] 
            coordinates {(\X,\Y) (\X, {\Y*\prop})}  
            node [rotate=90, below right] {$v \sin(\theta)$};
        % plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\vstartx, \vstarty) (\X*\prop, {func(\X)*\prop})} 
            node [pos=0.5, font=\Large, above left] {$\mathbf{v}$};
        % actual direction of the plane
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\dstartx, \dstarty) (\dendx, \dendy)} 
            node [left,align=center] {Actual direction \\ of the plane};
        
        \addplot[
            -,
            dashed, 
            line width=1.5pt,
        ] 
            coordinates {
                (\dstartx, \dstarty) 
                (\dstartx + (\dstartx - \dendx), \dstarty+\X*\cprop*\slope)
            } ;
            
        % Direction of the wind
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\wstartx, \wstarty) (\wendx, \wendy)} 
            node [pos=0.5, font=\Large, right] {$\mathbf{w}$};
        
        \addplot [-, thick] coordinates {(\dendx,\dendy) (\wendx, \wendy)} ;

        \addplot [-, thick] 
            coordinates {(\X*\prop,{\Y*\prop}) (\X*\prop, {\Y-\X*\slope*\cprop})};

        \draw[->, line width=1.1pt] (rel axis cs:0.9,0.81) -- (rel axis cs:0.9,0.9) 
        node[left,pos=0.5, font=\Large] {$+$};
        \draw[->, line width=1.1pt] (rel axis cs:0.91,0.8) -- (rel axis cs:1,0.8) 
        node[below,pos=0.5, font=\Large] {$+$};

        \coordinate (A) at (axis cs:0,0); 
        \coordinate (B) at (axis cs:\X,\Y); 
        \draw[thick] 
        let 
            \p1 = ($(B)-(A)$), 
            \n1 = {atan2(\y1,\x1)},
    
        in (A) ++(0:1.5cm) arc[start angle=0, end angle=\n1, radius=1.5cm] 
            node[midway, right, font=\Large] {$\theta$};

        \coordinate (C) at (axis cs:\vendx,\vendy); 
        \draw[thick] 
        let 
            \p1 =  ($(B)-(A)$), 
            \n1 = {atan2(\y1,\x1)},

        in (C) ++(0:1cm) arc[start angle=0, end angle=\n1, radius=1cm] 
            node[midway, right, font=\Large] { \( \theta \) };
        
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```


##### Case 3
Wind speed $w<$ plane speed $v$. In this case $w / v<1$, so that $1-(w / v)>0$. We see from (n), therefore that when $x=0$, $y=0$. Hence the plane will reach the town $T$. A rough graph of its path is shown in Fig. 17.14, with $v=2 w$.


```tikz
\usepackage{pgfplots}
\usetikzlibrary{calc}
\pgfplotsset{compat=1.16,width=20cm,height=10cm}
\begin{document} 
    \begin{tikzpicture}
        \newcommand\A{2000};
        \newcommand\W{100};
        \newcommand\V{500};
        \newcommand\K{\W/\V};
        \newcommand\X{1000};
        \newcommand\prop{0.6};
        \newcommand\cprop{0.4}; 
        \begin{axis}[
            axis line style={line width=1.5pt},
            draw={rgb,255:red,209;green,209;blue,209}, 
            text={rgb,255:red,209;green,209;blue,209}, 
            fill={rgb,255:red,209;green,209;blue,209}, 
            axis lines=middle,
            samples=200,
            enlargelimits,
            ticks=none,
            declare function={
                func(\x)=((\A/2)*((\x/\A)^(1-\K)- (\x/\A)^(1+\K));
                derivative(\x)=(
                    (1/2)*( 
                        (1-\K)*( 
                            (\x/\A)^(-\K) 
                        ) - 
                        (1+\K)*(
                            (\x/\A)^(\K)
                        )
                    )
                );
            }
        ]
        \pgfmathparse{\X+0.8};
        \pgfmathsetmacro\slope{derivative(\X)};
        \pgfmathsetmacro\Y{func(\X)};
        \pgfmathsetmacro\angle{atan(\Y/\X)};
        
        \pgfmathsetmacro\vstartx{\X};
        \pgfmathsetmacro\vstarty{\Y};
        \pgfmathsetmacro\vendx{\X*\prop};
        \pgfmathsetmacro\vendy{func(\X)*\prop};

        \pgfmathsetmacro\dstartx{\X};
        \pgfmathsetmacro\dstarty{\Y};
        \pgfmathsetmacro\dendx{\X*\prop};
        \pgfmathsetmacro\dendy{\Y-\X*\slope*\cprop};

        \pgfmathsetmacro\wstartx{\X};
        \pgfmathsetmacro\wstarty{\Y};
        \pgfmathsetmacro\wendx{\X};
        \pgfmathsetmacro\wendy{\Y + (\dendy - \vendy)};
        
        \addplot[
            ultra thick,
            domain=\A:0,
            draw={rgb,255:red,163;green,190;blue,140},
        ] {func(x)}
            node [
                pos=0, 
                below, 
                font=\LARGE,
            ] {$\left(a, 0\right)$}
            node [pos=0.05, above right, font=\Huge] {$\frac{w}{v}=\frac1 5$};
           

        \node at (0,0) [font=\LARGE, below right] {$T(0,0)$};
        
        \addplot[] 
            coordinates {(0,0) (\X, {func(\X)})}
            node [above right, font=\LARGE] {$P(x,y)$};
        
        \addplot[]  
            coordinates {(\X,0) (\X, {func(\X)})}
            node [pos=0.4, right, font=\Large] {$y$};

        \addplot[]  
            coordinates {(0,0) (\X, 0)}
            node [pos=0.5, below, font=\Large] {$x$};

        % x component of the plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt, 
        ] 
            coordinates {(\X,{func(\X)*\prop}) (\X*\prop, {func(\X)*\prop})}  
            node [pos=0.5, below] {$v \cos(\theta)$};
        % y component of the plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt
        ] 
            coordinates {(\X,\Y) (\X, {\Y*\prop})}  
            node [rotate=90, below right] {$v \sin(\theta)$};
        % plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\vstartx, \vstarty) (\X*\prop, {func(\X)*\prop})} 
            node [pos=0.5, font=\Large, above left] {$\mathbf{v}$};
        % actual direction of the plane
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\dstartx, \dstarty) (\dendx, \dendy)} 
            node [left,align=center] {Actual direction \\ of the plane};
        
        \addplot[
            -,
            dashed, 
            line width=1.5pt,
        ] 
            coordinates {
                (\dstartx, \dstarty) 
                (\dstartx + (\dstartx - \dendx), \dstarty+\X*\cprop*\slope)
            } ;
            
        % Direction of the wind
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\wstartx, \wstarty) (\wendx, \wendy)} 
            node [pos=0.5, font=\Large, right] {$\mathbf{w}$};
        
        \addplot [-, thick] coordinates {(\dendx,\dendy) (\wendx, \wendy)} ;

        \addplot [-, thick] 
            coordinates {(\X*\prop,{\Y*\prop}) (\X*\prop, {\Y-\X*\slope*\cprop})};

        \draw[->, line width=1.1pt] (rel axis cs:0.9,0.81) -- (rel axis cs:0.9,0.9) 
        node[left,pos=0.5, font=\Large] {$+$};
        \draw[->, line width=1.1pt] (rel axis cs:0.91,0.8) -- (rel axis cs:1,0.8) 
        node[below,pos=0.5, font=\Large] {$+$};

        \coordinate (A) at (axis cs:0,0); 
        \coordinate (B) at (axis cs:\X,\Y); 
        \draw[thick] 
        let 
            \p1 = ($(B)-(A)$), 
            \n1 = {atan2(\y1,\x1)},
    
        in (A) ++(0:1.5cm) arc[start angle=0, end angle=\n1, radius=1.5cm] 
            node[midway, right, font=\Large] {$\theta$};

        \coordinate (C) at (axis cs:\vendx,\vendy); 
        \draw[thick] 
        let 
            \p1 =  ($(B)-(A)$), 
            \n1 = {atan2(\y1,\x1)},

        in (C) ++(0:1cm) arc[start angle=0, end angle=\n1, radius=1cm] 
            node[midway, right, font=\Large] { \( \theta \) };
        
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```


#### Flip the script
Going from $a$ to 0 or from 0 to $a$ has important consequences. Solving either way will give the same answer, but one is much easier than the other.

Going from left to right instead of right to left like [[#^6ef307|here]] means that the speed in the $x$ direction is now positive instead of negative
$$
\dot{x}=v \cos \theta, \quad \dot{y}=-v \sin \theta + w
$$
and dividing the two still yields $y^\prime$

$$\frac{\dot{y}}{\dot{x}}=\frac{dy}{dx}=\frac{-v \sin \theta + w}{v \cos \theta} = \frac{-\sin \theta + k}{v \cos \theta}, \quad k=\frac w v \tag{a}$$
The problem comes next when we want to replace $\sin \theta$ and $\cos \theta$. Looking at the following plot we see that

$$\sin \theta = \frac{y}{\sqrt{p^2 + y^2}}, \quad \cos \theta = \frac{p}{\sqrt{p^2 + y^2}}$$
where $p=a-x$. Replacing in (a)

$$
\begin{align}
\frac{dy}{dx}= \frac{-\sin \theta + k}{v \cos \theta} &= \frac{-y + k\sqrt{p^2+y^2}}{p} \\
&= \frac{-y + k\sqrt{(a-x)^2+y^2}}{(a-x)} \tag{b}\\
\end{align}
$$


```tikz
\usepackage{pgfplots}
\usetikzlibrary{calc}
\pgfplotsset{compat=1.16,width=20cm,height=10cm}
\begin{document} 
    \begin{tikzpicture}
        \newcommand\A{2000};
        \newcommand\W{100};
        \newcommand\V{500};
        \newcommand\K{\W/\V};
        \newcommand\X{1000};
        
        \newcommand\prop{0.6};
        \pgfmathsetmacro\cprop{1-\prop}; 
        \begin{axis}[
            axis line style={line width=1.5pt},
            draw={rgb,255:red,209;green,209;blue,209}, 
            text={rgb,255:red,209;green,209;blue,209}, 
            fill={rgb,255:red,209;green,209;blue,209}, 
            axis lines=middle,
            samples=200,
            enlargelimits,
            ticks=none,
            declare function={
                func(\x)=((\A/2)*(((-\x+\A)/\A)^(1-\K)- ((-\x+\A)/\A)^(1+\K));
                derivative(\x)=(
                    (1/2)*( 
                        (1-\K)*( 
                            ((-\x+\A)/\A)^(-\K) 
                        ) - 
                        (1+\K)*(
                            ((-\x+\A)/\A)^(\K)
                        )
                    )
                );
            }
        ]
        
        \pgfmathsetmacro\P{\A-\X};
        \pgfmathsetmacro\slope{derivative(\X)};
        \pgfmathsetmacro\Y{func(\X)};
        \pgfmathsetmacro\angle{atan(\Y/\X)};
        
        \pgfmathsetmacro\vstartx{\X};
        \pgfmathsetmacro\vstarty{\Y};
        \pgfmathsetmacro\vendx{\X+\P*\prop};
        \pgfmathsetmacro\vendy{\Y*\cprop};

        \pgfmathsetmacro\dstartx{\X};
        \pgfmathsetmacro\dstarty{\Y};
        \pgfmathsetmacro\dendx{\X+\P*\prop};
        \pgfmathsetmacro\dendy{\Y-\P*\slope*\cprop};

        \pgfmathsetmacro\wstartx{\X};
        \pgfmathsetmacro\wstarty{\Y};
        \pgfmathsetmacro\wendx{\X};
        \pgfmathsetmacro\wendy{\Y + (\dendy - \vendy)};
        
        \addplot[
            ultra thick,
            domain=\A:0,
            draw={rgb,255:red,163;green,190;blue,140},
        ] {func(x)}
            node [
                pos=0, 
                below, 
                font=\LARGE,
            ] {$\left(a, 0\right)$};
           

        \node at (0,0) [font=\LARGE, below right] {$T(0,0)$};
        
        \addplot[] 
            coordinates {(\A,0) (\X, \Y)}
            node [above left, font=\LARGE] {$P(x,y)$};
        
        \addplot[]  
            coordinates {(\X,0) (\X, \Y)}
            node [pos=0.2, left, font=\Large] {$y$};

        \addplot[]  
            coordinates {(0,0) (\X, 0)}
            node [pos=0.5, below, font=\Large] {$x$};
        \addplot[]  
            coordinates {(\X,0) (\A, 0)}
            node [pos=0.5, below, font=\Large] {$p$};

        % x component of the plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt, 
        ] 
            coordinates {(\X,{\Y*\cprop}) (\vendx, {\Y*\cprop})}  
            node [pos=0.5, below] {$v \cos(\theta)$};
        % y component of the plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt
        ] 
            coordinates {(\X,\Y) (\X, {\Y*\cprop})}  
            node [pos=0.5, rotate=90, above] {$v \sin(\theta)$};
        % plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\vstartx, \vstarty) (\vendx, \vendy)} 
            node [pos=0.5, font=\Large, above right] {$\mathbf{v}$};
            
        % Direction of the wind
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\wstartx, \wstarty) (\wendx, \wendy)} 
            node [pos=0.5, font=\Large, right] {$\mathbf{w}$};
        
        %\addplot [-, thick] coordinates {(\dendx,\dendy) (\wendx, \wendy)} ;

        %\addplot [-, thick] coordinates {(\X*\prop,{\Y*\prop}) (\X*\prop, {\Y-\X*\slope*\cprop})};

        \draw[->, line width=1.1pt] (rel axis cs:0.9,0.81) -- (rel axis cs:0.9,0.9) 
        node[left,pos=0.5, font=\Large] {$+$};
        \draw[->, line width=1.1pt] (rel axis cs:0.91,0.8) -- (rel axis cs:1,0.8) 
        node[below,pos=0.5, font=\Large] {$+$};

        \coordinate (A) at (axis cs:\A,0); 
        \coordinate (B) at (axis cs:\X,\Y); 
        \draw[thick] 
        let 
            \p1 = ($(A)-(B)$), 
            \n1 = {atan2(\y1,\x1)},
    
        in (A) ++(0:-1.5cm) arc[start angle=180, end angle={180+\n1}, radius=1.5cm] 
            node[midway, left, font=\Large] {$\theta$};

        \coordinate (C) at (axis cs:\vendx,\vendy); 
        \draw[thick] 
        let 
            \p1 =  ($(A)-(B)$), 
            \n1 = {atan2(\y1,\x1)},

        in (C) ++(0:-1.3cm) arc[start angle=180, end angle={180+\n1}, radius=1.3cm] 
            node[midway, left, font=\Large] { \( \theta \) };
        
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```

Solving from (b) puts us on the fast track to ugly-town. We can technically use [[../../Solutions/Variable Substitution in DE|Variable Substitution in DE]] on 

$$
\begin{align}
(a-x)y^\prime= -y + k\sqrt{(a-x)^2+y^2}\\
\end{align}
$$

with $Q(x,y) = (a-x)$ and $P(x,y)=-y + k\sqrt{(a-x)^2+y^2}$. We can use the substitution $u=\frac y x$ and $y^\prime = u^\prime x + u$


$$
\begin{align}
(a-x)y^\prime + y - k\sqrt{(a-x)^2+y^2} &= 0\\
(a-x)(u^\prime x + u) + ux - k\sqrt{(a-x)^2+(ux)^2} &= 0\\
a(u^\prime x + u) - x(u^\prime x + u) + ux - k\sqrt{(a-x)^2+(ux)^2} &= 0\\
axu^\prime  + au - x^2u^\prime  - \cancel{ux} + \cancel{ux} - k\sqrt{a^2-2ax + x^2+(ux)^2} &= 0\\
axu^\prime  + au - x^2u^\prime  - k\sqrt{a^2-2ax + x^2+(ux)^2} &= 0\\
\end{align}
$$

Doing a separation of variable ($u^\prime$ on one side and $x$ on the other) is madness. 


> [!important] 
> Chose a setting that will make your life easier. Think about the direction of the movement, the impact of the origin, etc.




## With Polar Coordinates

### Solution 
The components of the wind velocity $w$ in the radial and transverse directions are respectively [for the derivation of these formulas, see Lesson 34, (34.2)].

$$
\frac{d r}{d t}=w \sin \theta, \quad r \frac{d \theta}{d t}=w \cos \theta \tag{a}
$$

```tikz
\usepackage{pgfplots}
\usetikzlibrary{calc}
\pgfplotsset{compat=1.16,width=20cm,height=10cm}
\begin{document} 
    \begin{tikzpicture}
        \newcommand\A{2500};
        \newcommand\W{100};
        \newcommand\V{500};
        \newcommand\K{\W/\V};
        \newcommand\X{1200};
        \newcommand\windangle{30};
        \newcommand\prop{0.6};
        \pgfmathsetmacro\cprop{1 - \prop}; 
        \begin{axis}[
            axis line style={line width=1.5pt},
            draw={rgb,255:red,209;green,209;blue,209}, 
            text={rgb,255:red,209;green,209;blue,209}, 
            fill={rgb,255:red,209;green,209;blue,209}, 
            axis lines=middle,
            samples=200,
            enlargelimits,
            ticks=none,
            declare function={
                func(\x)=((\A/2)*((\x/\A)^(1-\K)- (\x/\A)^(1+\K));
                derivative(\x)=(
                    (1/2)*( 
                        (1-\K)*( 
                            (\x/\A)^(-\K) 
                        ) - 
                        (1+\K)*(
                            (\x/\A)^(\K)
                        )
                    )
                );
            }
        ]
        \pgfmathparse{\X+0.8};
        \pgfmathsetmacro\slope{derivative(\X)};
        \pgfmathsetmacro\Y{func(\X)};
        \pgfmathsetmacro\angle{atan(\Y/\X)};
        
        \pgfmathsetmacro\vstartx{\X};
        \pgfmathsetmacro\vstarty{\Y};
        \pgfmathsetmacro\vendx{\X*\prop};
        \pgfmathsetmacro\vendy{func(\X)*\prop};

        \pgfmathsetmacro\dstartx{\X};
        \pgfmathsetmacro\dstarty{\Y};
        \pgfmathsetmacro\dendx{\X*\prop};
        \pgfmathsetmacro\dendy{\Y-\X*\slope*\cprop};

        \pgfmathsetmacro\wstartx{\X};
        \pgfmathsetmacro\wstarty{\Y};
        \pgfmathsetmacro\wrendx{\X*1.3};
        \pgfmathsetmacro\wrendy{func(\X)*1.3};

        % Calculate the aspect ratio 
        \pgfmathsetmacro{\aspectRatio}{10/20}; % height/width 


        \pgfmathsetmacro\wtstartx{\wrendx};
        \pgfmathsetmacro\wtstarty{\wrendy};
        \pgfmathsetmacro\MMM{-1/((\wrendy - \wstarty)/((\wrendx-\wstartx)*\aspectRatio))};
        \pgfmathsetmacro\AAA{(\wrendx-\wstartx)*0.3};
        \pgfmathsetmacro\wtendx{\wrendx - \AAA};
        \pgfmathsetmacro\wtendy{\wrendy - \MMM * \AAA * 0.13};
        
        \pgfmathsetmacro\wyendx{\X};
        \pgfmathsetmacro\wyendy{\Y + (\dendy - \vendy)};
        \pgfmathsetmacro\wx{\dendy - \vendy}; 
        \pgfmathsetmacro\wxendx{\X*(1+\cprop*0.8) };
        \pgfmathsetmacro\wxendy{\Y};
        
        \addplot[
            ultra thick,
            domain=\A:\X,
            draw={rgb,255:red,163;green,190;blue,140},
        ] {func(x)}
            node [
                pos=0, 
                below, 
                font=\LARGE,
            ] {$\left(a, 0\right)$};

        \node at (0,0) [font=\LARGE, below right] {$T(0,0)$};
        
        \addplot[] 
            coordinates {(0,0) (\X, {func(\X)})};
        
        \addplot[]  
            coordinates {(\X,0) (\X, {func(\X)})}
            node [pos=0.4, right, font=\Large] {$y$};

        \addplot[]  
            coordinates {(0,0) (\X, 0)}
            node [pos=0.5, below, font=\Large] {$x$};

        % plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\vstartx, \vstarty) (\X*\prop, {func(\X)*\prop})} 
            node [pos=0.5, font=\Large, above left] {$\mathbf{v}$};
 
            
        % Direction of the wind
        \addplot[
            -, 
            line width=1pt,
        ] 
            coordinates {(\wstartx, \wstarty) (\wyendx, \wyendy)};


        \addplot[
            ->, 
            -latex, 
            line width=1pt,
        ] 
            coordinates {(\wstartx, \wstarty) (\wrendx, \wrendy)}
            node [pos=0.7, font=\large, right] {$\mathbf{w}\cos(\psi)$};

        \addplot[
            ->, 
            -latex, 
            line width=1pt,
        ] 
            coordinates {(\wtstartx, \wtstarty) (\wtendx, \wtendy)}
            node [pos=0.5, font=\large, right] {$\mathbf{w}\sin(\psi)$};
        
        \addplot[
            ->, 
            -latex, 
            line width=1pt,
        ] 
            coordinates {(\wstartx, \wstarty) (\wtendx, \wtendy)} 
            node [pos=0.8, above left, font=\Large] {$\mathbf{w}$}
            node [above right, font=\Large] {Wind Direction};
        
        \addplot[
            -, 
            line width=1pt,
        ] 
            coordinates {(\X, \Y) (\wxendx, \wxendy)};
            
        \draw[->, line width=1.1pt] (rel axis cs:0.9,0.41) -- (rel axis cs:0.9,0.5) 
        node[left,pos=0.5, font=\Large] {$+$};
        \draw[->, line width=1.1pt] (rel axis cs:0.91,0.4) -- (rel axis cs:1,0.4) 
        node[below,pos=0.5, font=\Large] {$+$};

        \coordinate (A) at (axis cs:0,0); 
        \coordinate (B) at (axis cs:\X,\Y); 
        \draw[thick] 
        let 
            \p1 = (B), 
            \n1 = {atan2(\y1,\x1)},
    
        in (A) ++(0:1.5cm) arc[start angle=0, end angle=\n1, radius=1.5cm] 
            node[midway, right, font=\Large] { \( \theta \) };
    
        \coordinate (D) at (axis cs:\X,\Y); 
        \coordinate (E) at (axis cs:\wtendx,\wtendy); 
        \draw[thick] 
        let 
            \p1 = ($(E)-(D)$), 
            \n1 = {atan2(\y1,\x1)},
        in (D) ++(90:1cm) arc[start angle=90, end angle=\n1, radius=1cm] 
            node[midway, above, font=\Large] {$\alpha$};

        \coordinate (F) at (axis cs:\wrendx,\wrendy); 
        \draw[thick] 
        let 
            \p1 = ($(F)-(D)$), 
            \p2 = ($(E)-(D)$), 
            \n1 = {atan2(\y1,\x1)},
            \n2 = {atan2(\y2,\x2)},
        in (D) ++(\n1:1.3cm) arc[start angle=\n1, end angle=\n2, radius=1.3cm] 
            node[midway, above right, font=\Large] {$\psi$};

        \coordinate (G) at (axis cs:\wxendx,\Y); 
        \draw[thick] 
        let 
            \p1 = ($(F)-(D)$), 
            \p2 = ($(G)-(D)$), 
            \n1 = {atan2(\y1,\x1)},
            \n2 = {atan2(\y2,\x2)},
        in (D) ++(\n1:1cm) arc[start angle=\n1, end angle=\n2, radius=1cm] 
            node[midway,  right, font=\Large] {$\theta$};

    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```



> [!blank-container]
> ![[Pasted image 20230822152639.png]]

Figure 17.21

Hence the effective velocity of the plane in the radial direction, taking into account the wind's speed $w$ and the plane's speed $v$, is

$$
\frac{d r}{d t}=-v+w \sin \theta \tag{b}
$$

Dividing (b) by the second equation in (a), we obtain

$$
\frac{d r}{r d \theta}=\frac{-v+w \sin \theta}{w \cos \theta}=-\frac{v}{w} \sec \theta+\tan \theta \tag{c}
$$

Let

$$
k=-\frac{v}{w} .
$$

Then (c) becomes

$$
\frac{d r}{r}=(k \sec \theta+\tan \theta) d \theta \tag{d}
$$

Its solution, by integration, is

$$
\begin{align}
\frac{\frac{dr}{dt}}{r\frac{d\theta}{dt}}=\frac{d r}{r d \theta}&=\frac{-v+\omega \sin \theta}{\omega \cos \theta}\\
&=k \sec \theta+\tan \theta \\
\int \frac{1}{r} \frac{d r}{d \theta} d \theta&=\int k \sec \theta+\tan \theta d \theta \\
\log r+C_1&=k \ln |\sec \theta+\tan \theta|-\ln |\cos \theta| +C_2 \tag{e}\\
\end{align}
$$
Since we are dealing with values of $\theta$ that are between 0 and $\pi$, we can drop the absolute values. Taking the $\exp$ on both sides gives

$$
\begin{align}
\log r+C_3 &=k \ln (\sec \theta+\tan \theta)-\ln (\cos \theta) \\
C r&=\frac{(\sec \theta+\tan \theta)^{k}}{\cos \theta} \\
& =(\sec \theta+\tan \theta)^{k} \sec \theta \tag{f} \\
\end{align}
$$

At $t=0, r=a, \theta=0$. Substituting these values in (f), we have

$$
C a=1, \quad C=1 / a \tag{g}
$$

The equation of the path is, therefore, after replacing $k$ by its value in (f),

$$
r=a(\sec \theta+\tan \theta)^{-v / w} \sec \theta \tag{h}
$$

or

$$
r \cos \theta=a(\sec \theta+\tan \theta)^{-v / w} \tag{i}
$$

## Wind direction at an angle

^35ce11

### Statement

A pilot always keeps the nose of his plane pointed toward a city T due west of his starting point. If his speed is $v$ miles per hour and a wind is blowing with a velocity $w$ in a direction which makes an angle $\alpha$ with the vertical, find the equation of the plane's path. Assume that it starts from a flying field which is at a distance $a$ miles from $T$.

See [[#^07afa4|this example]] for a special case of this where $\alpha=0$

```tikz
\usepackage{pgfplots}
\usetikzlibrary{calc}
\pgfplotsset{compat=1.16,width=20cm,height=10cm}

\begin{document} 

\begin{tikzpicture}
    \newcommand\A{1200};
    \newcommand\W{100};
    \newcommand\V{500};
    \newcommand\K{\W/\V};
    \newcommand\X{1000};

    \begin{axis}[
        axis line style={line width=1.5pt},
        draw={rgb,255:red,209;green,209;blue,209}, 
        text={rgb,255:red,209;green,209;blue,209}, 
        fill={rgb,255:red,209;green,209;blue,209}, 
        axis lines=middle,
        samples=200,
        enlargelimits,
        ticks=none,
        declare function={
            func(\x)=((\A/2)*((\x/\A)^(1-\K) - (\x/\A)^(1+\K)));
        },
        
    ]
    \pgfmathsetmacro\Y{func(\X)};
    
    \node at (0,0) [font=\LARGE, below right] {$T(0,0)$};
    
    \addplot[] 
        coordinates {(\A, 0)} 
        node [font=\Large, below] {$(a,0)$};
    
    \addplot[-stealth, line width=3pt] 
        coordinates {(0,0) (\X, {func(\X)})}
        node [above right, font=\LARGE] {Wind direction}
        node [pos=0.5, above left, font=\Large]{$\mathbf{w}$};
    \draw[-stealth, line width=3pt]
        (\X,0) -- (\X, {func(\X)})
        node [pos=0.5, rotate=90, below, font=\Large] {$w\cos(\alpha)$};
    \draw[-stealth, line width=3pt]
        (0,0) -- (\X, 0)
        node [pos=0.5, below, font=\Large] {$w\sin(\alpha)$};
        
    %\draw[->, line width=1.1pt] (rel axis cs:0.9,0.81) -- (rel axis cs:0.9,0.9) 
    %    node[left,pos=0.5, font=\Large] {$+$};
    %\draw[->, line width=1.1pt] (rel axis cs:0.91,0.8) -- (rel axis cs:1,0.8) 
    %    node[below,pos=0.5, font=\Large] {$+$};
    \coordinate (A) at (axis cs:0,0); 
    \coordinate (B) at (axis cs:\X,\Y); 
    \draw[thick] 
    let 
        \p1 = ($(B)-(A)$), 
        \n1 = {atan2(\y1,\x1)},
    in (A) ++(90:1.5cm) arc[start angle=90, end angle=\n1, radius=1.5cm] 
        node[midway, above right, font=\Large] {$\alpha$};

    \draw[thick] 
    let 
        \p1 = ($(B)-(A)$), 
        \n1 = {180+atan2(\y1,\x1)},
    in (B) ++(\n1:1.5cm) arc[start angle=\n1, end angle=270, radius=1.5cm] 
        node[midway, above right, font=\Large] {$\alpha$};
    \end{axis} 
\end{tikzpicture} 

\end{document}
```


### Solution

We shall give two methods by which this problem may be solved. 

#### Method 1
Choose the axes so that the direction of the wind becomes the $y$ axis (Fig. 17.32). The initial conditions at $t=0$, therefore, become

$$
x=a \cos \alpha, \quad y=a \sin \alpha .
$$

```tikz
\usepackage{pgfplots}
\usetikzlibrary{calc}
\pgfplotsset{compat=1.16,width=20cm,height=10cm}
\begin{document} 
    \begin{tikzpicture}
        \pgfmathsetmacro\A{2000};
        \pgfmathsetmacro\W{100};
        \pgfmathsetmacro\V{500};
        \pgfmathsetmacro\K{\W/\V};
        \newcommand\X{800};
        \newcommand\prop{0.6};
        \newcommand\cprop{0.4}; 
        \pgfmathsetmacro\windangle{5};
        \pgfmathsetmacro\MM{ (tan(\windangle) + sec(\windangle))*(((\A * cos(\windangle))^\K)) };
        
        \begin{axis}[
            axis line style={line width=1.5pt},
            draw={rgb,255:red,209;green,209;blue,209}, 
            text={rgb,255:red,209;green,209;blue,209}, 
            fill={rgb,255:red,209;green,209;blue,209}, 
            axis lines=middle,
            samples=200,
            enlargelimits,
            enlarge x limits={abs=2cm},
            ticks=none,
            xmax={\A*1.5},
            declare function={
                func(\x)=(((\MM^2)*(\x^(1-\K))) - (\x^(1+\K)))/(2*\MM);
                derivative(\x)=(((1-\K)*(\MM^2)*(\x^(-\K))) - (1+\K)*(\x^(\K)))/(2*\MM);
            }
        ]
        \pgfmathparse{\X+0.8};
        \pgfmathsetmacro\slope{derivative(\X)};
        \pgfmathsetmacro\Y{func(\X)};
        \pgfmathsetmacro\angle{atan(\Y/\X)};
        
        \pgfmathsetmacro\vstartx{\X};
        \pgfmathsetmacro\vstarty{\Y};
        \pgfmathsetmacro\vendx{\X*\prop};
        \pgfmathsetmacro\vendy{func(\X)*\prop};

        \pgfmathsetmacro\dstartx{\X};
        \pgfmathsetmacro\dstarty{\Y};
        \pgfmathsetmacro\dendx{\X*\prop};
        \pgfmathsetmacro\dendy{\Y-\X*\slope*\cprop};

        \pgfmathsetmacro\wstartx{\X};
        \pgfmathsetmacro\wstarty{\Y};
        \pgfmathsetmacro\wendx{\X};
        \pgfmathsetmacro\wendy{\Y + (\dendy - \vendy)};
        
        \addplot[
            ultra thick,
            domain=\A:650,
            draw={rgb,255:red,163;green,190;blue,140},
        ] {func(x)}
            node[
                pos=0, 
                pin={[font=\large, pin edge={thick}, anchor=west, pin distance=0.8cm]270:{Initial plane position}}
            ]{}
            %node [pos=0.25, above right, font=\Huge] {$\frac{w}{v}=\frac 1 2$}
            node [mark=*, pos=0, above right, font=\Large] {$P_0(a\cos(\alpha), a\sin(\alpha)$};

        \node at (0,0) [font=\LARGE, below left] {$T(0,0)$};
        
        \addplot[] 
            coordinates {(0,0) (\X, {func(\X)})}
            node [above right, font=\LARGE] {$P(x,y)$};
        
        \addplot[]  
            coordinates {(\X,0) (\X, {func(\X)})}
            node [pos=0.2, right, font=\Large] {$y$};

        \addplot[]  
            coordinates {(0,0) (\X, 0)}
            node [pos=0.5, below, font=\Large] {$x$};

        % x component of the plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt, 
        ] 
            coordinates {(\X,{func(\X)*\prop}) (\X*\prop, {func(\X)*\prop})}  
            node [pos=0.5, below] {$v \cos(\theta)$};
        % y component of the plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt
        ] 
            coordinates {(\X,\Y) (\X, {\Y*\prop})}  
            node [rotate=90, below right] {$v \sin(\theta)$};
        % plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\vstartx, \vstarty) (\X*\prop, {func(\X)*\prop})} 
            node [pos=0.5, font=\Large, above left] {$\mathbf{v}$};
        % actual direction of the plane
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\dstartx, \dstarty) (\dendx, \dendy)} 
            node[
                pos=0.5, 
                pin={
                [pin edge={thick}, 
                anchor=north, 
                pin distance=2.5cm
                ]90:{Actual velocity of the plane}}
            ]{};
        
        \addplot[
            -,
            dashed, 
            line width=1.5pt,
        ] 
            coordinates {
                (\dstartx, \dstarty) 
                (\dstartx + (\dstartx - \dendx), \dstarty+\X*\cprop*\slope)
            } ;
            
        % Direction of the wind
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\wstartx, \wstarty) (\wendx, \wendy)} 
            node [pos=0.5, font=\Large, right] {$\mathbf{w}$};
        
        \addplot [-, thick] coordinates {(\dendx,\dendy) (\wendx, \wendy)} ;

        \addplot [-, thick] 
            coordinates {(\X*\prop,{\Y*\prop}) (\X*\prop, {\Y-\X*\slope*\cprop})};

        \draw[->, line width=1.1pt] (rel axis cs:0.8,0.71) -- (rel axis cs:0.8,0.8) 
        node[left,pos=0.5, font=\Large] {$+$};
        \draw[->, line width=1.1pt] (rel axis cs:0.81,0.7) -- (rel axis cs:0.9,0.7) 
        node[below,pos=0.5, font=\Large] {$+$};

        \coordinate (A) at (axis cs:0,0); 
        \coordinate (B) at (axis cs:\X,\Y); 
        \draw[thick] 
        let 
            \p1 = (B), 
            \n1 = {atan2(\y1,\x1)},
    
        in (A) ++(0:1.5cm) arc[start angle=0, end angle=\n1, radius=1.5cm] 
            node[midway, right, font=\Large] { \( \theta \) };

        \coordinate (C) at (axis cs:\vendx,\vendy); 
        \draw[thick] 
        let 
            \p1 = (B), 
            \n1 = {atan2(\y1,\x1)},
        in (C) ++(0:1cm) arc[start angle=0, end angle=\n1, radius=1cm] 
            node[midway, right, font=\Large] { \( \theta \) };

        \addplot[thick] coordinates {(0, 0) (\A, {func(\A)})}
            node[pos=0.75,above]{$a$};
        \addplot[] 
            coordinates {(\A,0) (\A, {func(\A)})}
            node [midway, right] {$a\sin(\alpha)$};
        \node at ({\A*2/3}, 0)[below] {$a\cos(\alpha)$};
        \coordinate (D) at (axis cs:\A,{func(\A)}); 
        \draw[thick] 
        let 
            \p1 = ($(D)-(A)$), 
            \n1 = {atan2(\y1,\x1)},
        in (A) ++(0:2cm) arc[start angle=0, end angle=\n1, radius=2cm] 
            node[midway, right, font=\Large] { \( \alpha \) };
            
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```


Figure 17.32

The differential equations of motion, however, are exactly the same as those in Example 17.1, namely

$$
\frac{d x}{d t}=-v \cos \theta, \quad \frac{d y}{d t}=-v \sin \theta+w .
$$

Proceeding just as we did in [[#^07afa4|this example]], we obtain the same thing as [[#^5d2bb9]],

$$
\log \left(u+\sqrt{1+u^{2}}\right)=-k \log x+\log c, \quad k=w / v .
$$

But now at $t=0, x=a \cos \alpha, u=y / x=(a \sin \alpha) /(a \cos \alpha)=\tan \alpha$. Substituting these values in (c), and solving for $c$, we obtain

$$
C=(\tan \alpha+\sec \alpha)(a \cos \alpha)^{k}
$$

Hence (c) becomes, after replacing $u$ by its value $y / x$,

(e) $\log \left(\frac{y}{x}+\sqrt{\frac{x^{2}+y^{2}}{x^{2}}}\right)+\log x^{k}=\log \left[(\tan \alpha+\sec \alpha)(a \cos \alpha)^{k}\right]$. 

Simplification of (e) gives

$$
x^{k-1}\left(y+\sqrt{x^{2}+y^{2}}\right)=(\tan \alpha+\sec \alpha)(a \cos \alpha)^{k}, \quad k=w / v .
$$

Replacing $k$ by its value $w / v$, we obtain finally as the equation of the plane's path

$$
\text { (g) } \quad x^{(w / v)-1}\left(y+\sqrt{x^{2}+y^{2}}\right)=(\tan \alpha+\sec \alpha)(a \cos \alpha)^{w / v} \text {. }
$$

We leave it to you as an exercise to draw rough graphs of its path as we did in Comment 17.11. Remember $\tan \alpha, \sec \alpha, \cos \alpha$ and $a$ are constants.


#### Method 2

```tikz
\usepackage{pgfplots}
\usetikzlibrary{calc}
\pgfplotsset{compat=1.16,width=15cm,height=10cm}
\begin{document} 
    \begin{tikzpicture}
        \newcommand\A{2500};
        \newcommand\W{100};
        \newcommand\V{500};
        \newcommand\K{\W/\V};
        \newcommand\X{1200};
        \newcommand\windangle{30};
        \newcommand\prop{0.6};
        \pgfmathsetmacro\cprop{1 - \prop}; 
        \begin{axis}[
            axis line style={line width=1.5pt},
            draw={rgb,255:red,209;green,209;blue,209}, 
            text={rgb,255:red,209;green,209;blue,209}, 
            fill={rgb,255:red,209;green,209;blue,209}, 
            axis lines=middle,
            samples=200,
            enlargelimits,
            ticks=none,
            declare function={
                func(\x)=((\A/2)*((\x/\A)^(1-\K)- (\x/\A)^(1+\K));
                derivative(\x)=(
                    (1/2)*( 
                        (1-\K)*( 
                            (\x/\A)^(-\K) 
                        ) - 
                        (1+\K)*(
                            (\x/\A)^(\K)
                        )
                    )
                );
            }
        ]
        \pgfmathparse{\X+0.8};
        \pgfmathsetmacro\slope{derivative(\X)};
        \pgfmathsetmacro\Y{func(\X)};
        \pgfmathsetmacro\angle{atan(\Y/\X)};
        
        \pgfmathsetmacro\vstartx{\X};
        \pgfmathsetmacro\vstarty{\Y};
        \pgfmathsetmacro\vendx{\X*\prop};
        \pgfmathsetmacro\vendy{func(\X)*\prop};

        \pgfmathsetmacro\dstartx{\X};
        \pgfmathsetmacro\dstarty{\Y};
        \pgfmathsetmacro\dendx{\X*\prop};
        \pgfmathsetmacro\dendy{\Y-\X*\slope*\cprop};

        \pgfmathsetmacro\wstartx{\X};
        \pgfmathsetmacro\wstarty{\Y};
        \pgfmathsetmacro\wyendx{\X};
        \pgfmathsetmacro\wyendy{\Y + (\dendy - \vendy)};
        \pgfmathsetmacro\wx{\dendy - \vendy}; 
        \pgfmathsetmacro\wxendx{\X*(1+\cprop*0.8) };
        \pgfmathsetmacro\wxendy{\Y};
        
        \addplot[
            ultra thick,
            domain=\A:\X,
            draw={rgb,255:red,163;green,190;blue,140},
        ] {func(x)}
            node [
                pos=0, 
                below, 
                font=\LARGE,
            ] {$\left(a, 0\right)$};

        \node at (0,0) [font=\LARGE, below right] {$T(0,0)$};
        
        \addplot[] 
            coordinates {(0,0) (\X, {func(\X)})}
            node [above left, font=\LARGE] {$P(x,y)$};
        
        \addplot[]  
            coordinates {(\X,0) (\X, {func(\X)})}
            node [pos=0.4, right, font=\Large] {$y$};

        \addplot[]  
            coordinates {(0,0) (\X, 0)}
            node [pos=0.5, below, font=\Large] {$x$};

        % x component of the plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt, 
        ] 
            coordinates {(\X,{func(\X)*\prop}) (\X*\prop, {func(\X)*\prop})}  
            node [pos=0.5, below] {$v \cos(\theta)$};
        % y component of the plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt
        ] 
            coordinates {(\X,\Y) (\X, {\Y*\prop})}  
            node [rotate=90, below right] {$v \sin(\theta)$};
        % plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\vstartx, \vstarty) (\X*\prop, {func(\X)*\prop})} 
            node [pos=0.5, font=\Large, above left] {$\mathbf{v}$};
 
            
        % Direction of the wind
        \addplot[
            -, 
            line width=1pt,
        ] 
            coordinates {(\wstartx, \wstarty) (\wyendx, \wyendy)};

        \addplot[
            ->, 
            -latex, 
            line width=1pt,
        ] 
            coordinates {(\wxendx, \wxendy) (\wxendx, \wyendy)}
            node [pos=0.5, font=\large, right] {$\mathbf{w}_y=w\cos(\alpha)$};
        \addplot[
            ->, 
            -latex, 
            line width=1pt,
        ] 
            coordinates {(\X, \Y) (\wxendx, \wxendy)}
            node[
                pos=0.5, 
                pin={[font=\large, pin edge={ultra thick}, anchor=west, pin distance=1.5cm]330:{$\mathbf{w}_x=w\sin(\alpha)$}}
            ]{};
            %node [font=\large, below right] {$\mathbf{w}_x=w\sin(\alpha)$};
            
        \addplot[
            ->, 
            -latex, 
            line width=1pt,
        ] 
            coordinates {(\wstartx, \wstarty) (\wxendx, \wyendy)} 
            node [pos=0.8, above left, font=\Large] {$\mathbf{w}$}
            node [above right, font=\Large] {Wind Direction};
        

        \draw[->, line width=1.1pt] (rel axis cs:0.9,0.41) -- (rel axis cs:0.9,0.5) 
        node[left,pos=0.5, font=\Large] {$+$};
        \draw[->, line width=1.1pt] (rel axis cs:0.91,0.4) -- (rel axis cs:1,0.4) 
        node[below,pos=0.5, font=\Large] {$+$};

        \coordinate (A) at (axis cs:0,0); 
        \coordinate (B) at (axis cs:\X,\Y); 
        \draw[thick] 
        let 
            \p1 = (B), 
            \n1 = {atan2(\y1,\x1)},
    
        in (A) ++(0:1.5cm) arc[start angle=0, end angle=\n1, radius=1.5cm] 
            node[midway, right, font=\Large] { \( \theta \) };

        \coordinate (C) at (axis cs:\vendx,\vendy); 
        \draw[thick] 
        let 
            \p1 = (B), 
            \n1 = {atan2(\y1,\x1)},
        in (C) ++(0:1cm) arc[start angle=0, end angle=\n1, radius=1cm] 
            node[midway, right, font=\Large] { \( \theta \) };
    
        \coordinate (D) at (axis cs:\X,\Y); 
        \coordinate (E) at (axis cs:\wxendx,\wyendy); 
        \draw[thick] 
        let 
            \p1 = ($(E)-(D)$), 
            \n1 = {atan2(\y1,\x1)},
        in (D) ++(90:1cm) arc[start angle=90, end angle=\n1, radius=1cm] 
            node[midway, above, font=\Large] {$\alpha$};

        \draw[thick] 
        let 
            \p1 = ($(E)-(D)$), 
            \n1 = {180+atan2(\y1,\x1)},
        in (E) ++(\n1:1cm) arc[start angle=\n1, end angle=270, radius=1cm] 
            node[midway, below, font=\Large] {$\alpha$};
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```


The speed in the $x$ and $y$ directions are given by 
$$\dot{y}=-v \sin \theta+w \cos \alpha ,\quad \dot{x}=-v \cos \theta+w \sin \alpha \tag{a}$$

Dividing the first equation in (a) by the second one, we get $\frac{dy}{dx}$
$$
\begin{align}
\frac{\dot{y}}{\dot{x}}=\frac{d y}{d x}&=\frac{-v \sin \theta+w\cos \alpha}{-v \cos \theta+w \sin \alpha} \\
& =\frac{\sin \theta-k \cos \alpha}{\cos \theta-k \sin \alpha},\quad k=\frac{w}{v} \tag{b}
\end{align}
$$

Replace $\displaystyle \sin \theta=\frac{y}{\sqrt{x^2+y^2}}, \cos \theta=\frac{x}{\sqrt{x^2+y^2}}$ in (b)

$$
\begin{align}
y^{\prime}&=\frac{y-k \sqrt{x^2+y^2} \cos \alpha}{x-k \sqrt{x^2+y^2} \sin \alpha} \\
0 &= \underbrace{\left(x-k \sqrt{x^2+y^2} \sin \alpha\right)}_{Q(x, y)} y^{\prime}-\underbrace{y+k \sqrt{x^2+y^2} \cos \alpha}_{P(x, y)} \tag{c}\\
\end{align}
$$
where $Q(x,y)$ and $P(x,y)$ are [[../../Definitions/Homogeneous Function|homogeneous functions]]. Remember $v, w, \cos \alpha$, and $v \sin \alpha$ are constants. Hence it can be solved by [[../../Solutions/Variable Substitution in DE|Variable Substitution in DE]] . The initial conditions are $x=a, y=0$. $x$ is always positive, so no need to worry about absolute values. 

Using [[../../Solutions/Variable Substitution in DE#1. Substitution $u(t) = frac{x(t)}{t}$|this substitution]] $\displaystyle u=\frac{y}{x}, y^{\prime}=u^{\prime} x+u$ in (c)
$$
\begin{align} \\
 \left(x-k \sqrt{x^2+(u x)^2} \sin \alpha\right)(u^\prime x+u)-u x+k \sqrt{x^2+(ux)^2} \cos \alpha &= 0\\
 \cancel{x}\left(1-k \sqrt{1+u^2} \sin \alpha\right)\left(u^{\prime} x+u\right)-u\cancel{x}+\cancel{x}k \sqrt{1+u^2} \cos \alpha&=0 \\
u^{\prime} x-u^{\prime} xk \sqrt{1+u^2} \sin \alpha+\cancel{u}-uk \sqrt{1+u^2} \sin \alpha-\cancel{u}+ k \sqrt{1+u^2} \cos \alpha &= 0\\
u^{\prime}x\left(1-k \sqrt{1+u^2} \sin \alpha\right)+k \sqrt{1+u^2}(\cos \alpha-u \sin \alpha)&=0 \\
u^{\prime}x\left(1-k \sqrt{1+u^2} \sin \alpha\right)&=-k \sqrt{1+u^2}(\cos \alpha-u \sin \alpha) \\

\frac{1-k \sqrt{1+u^2} \sin \alpha}{k\sqrt{1+u^2}(\cos \alpha-u \sin \alpha)} u^{\prime}&=\frac{-1}{x} \\
\frac{1}{k \sqrt{1+u^2}(\cos \alpha-u \sin \alpha)} u^{\prime}-\frac{\cancel{k \sqrt{1+u^2}} \sin \alpha}{\cancel{k \sqrt{1+u^2}}(\cos \alpha-u \sin \alpha)}u^\prime &= \frac{-1}{x} \\
\int \frac{1}{k \sqrt{1+u^2}(\cos \alpha-u \sin \alpha)}\frac{du}{dx}dx-\int \frac{ \sin \alpha}{(\cos \alpha-u \sin \alpha)}\frac{du}{dx}dx &= \int \frac{-1}{x} dx \\
\frac 1 k \textcolor{orange}{\int \frac{1}{\sqrt{1+u^2}(\cos \alpha-u \sin \alpha)}du}\textcolor{violet}{-\int \frac{ \sin \alpha}{(\cos \alpha-u \sin \alpha)}du}&= \int \frac{-1}{x} dx \tag{d}
\end{align}
$$

The first integral is a bit of a pain, so I used Wolfram Alpha. The indefinite form is
$$
\textcolor{orange}{\int \frac{1}{\sqrt{1+u^2}(\cos \alpha-u \sin \alpha)} d u=\operatorname{arctanh}\left(\frac{\sin \alpha+u \cos \alpha}{\sqrt{u^2+1}}\right)+C}
$$

We'll use the initial conditions to avoid gymnastics to find $C$

$$
\begin{align}
\int_0^u \frac{1}{\sqrt{1+u^2}(\cos \alpha-u \sin \alpha)}  d u &=\operatorname{arctanh}\left(\frac{\sin \alpha+u \cos \alpha}{\sqrt{u^2+1}}\right) \Bigg \vert_0^u \\
&=\textcolor{lime}{\operatorname{arctanh}\left(\frac{\sin \alpha+u \cos \alpha}{\sqrt{u^2+1}}\right)} - \textcolor{cyan}{\operatorname{arctanh}\left(\sin (\alpha)\right)} \tag{e}
\end{align}
$$

Second one is much easier using [[../../../Calculus/Integration/Substitution rule|Substitution rule]]. Let’s declare $z=\cos \alpha-u \sin \alpha$ and $dz=-\sin \alpha du$

$$
\begin{align}
\textcolor{violet}{\int_0^u \frac{- \sin \alpha}{(\cos \alpha-u \sin \alpha)}du} &= \int_0^u \frac 1 z dz \\
&=\ln{z}+C \\
&= \ln\left(\cos \alpha-u \sin \alpha\right) \Big \vert_0^u\\
&=\ln\left(\cos \alpha-u \sin \alpha\right) - \ln\left(\cos \alpha\right)\\
&=\textcolor{violet}{\ln\left(\frac{\cos \alpha-u \sin \alpha}{\cos \alpha}\right)} \tag{f}
\end{align}
$$

Using [[../../../Trigonometry/Hyperbolic Identity#^5d2c7c|this identity]]  for $\operatorname{arctanh}$ in (e) for the first part

$$
\begin{align}
\textcolor{lime}{\operatorname{arctanh}\left(\frac{\sin \alpha+u \cos \alpha}{\sqrt{u^2+1}}\right)} &= \frac{1}{2} \ln \left(\frac{1+\frac{(\sin \alpha+u \cos \alpha)}{\sqrt{u^2+1}}}{1-\frac{(\sin \alpha+u \cos \alpha)}{\sqrt{u^2+1}}}\right) \\
&=\textcolor{lime}{\frac{1}{2} \ln \left(\frac{\sqrt{u^2+1}+\sin \alpha+u \cos \alpha}{\sqrt{u^2+1}-\sin \alpha-u \cos \alpha}\right)} \tag{g} \\ 
\end{align}
$$

and the second part
$$
\begin{align}
\textcolor{cyan}{\operatorname{arctanh}\left(\sin \alpha\right)} &= \textcolor{cyan}{\frac{1}{2} \ln \left(\frac{1+\sin \alpha}{1-\sin \alpha}\right)} \tag{h}
\end{align}
$$

> [!blank-container|right-large]
> ![[plane_with_wind_at_angle.svg]]
> > Four different wind angles $\alpha$. From top to bottom, 0, 30, 45 and 60 degrees from the vertical. $v=500$, $w=100$


Put (g), (h) and (f) together
$$
\begin{align}
-\ln\left(\frac x a\right) &= \frac 1 k \left(\textcolor{lime}{\frac{1}{2} \ln \left(\frac{\sqrt{u^2+1}+\sin \alpha+u \cos \alpha}{\sqrt{u^2+1}-\sin \alpha-u \cos \alpha}\right)} - \textcolor{cyan}{\frac{1}{2} \ln \left(\frac{1+\sin \alpha}{1-\sin \alpha}\right)} \right) + \textcolor{violet}{ \ln\left(\frac{\cos \alpha-u \sin \alpha}{\cos \alpha}\right) }\\
-\ln\left(\frac x a\right) -   \ln\left(\frac{\cos \alpha-u \sin \alpha}{\cos \alpha}\right)  &= \frac 1 {2k}  \ln \left(\frac{(1-\sin \alpha)\left(\sqrt{u^2+1}+\sin \alpha+u \cos \alpha\right)}{(1+\sin \alpha)\left(\sqrt{u^2+1}-\sin \alpha-u \cos \alpha\right)}\right)   \\
\left(\frac {x\left(\cos \alpha-u \sin \alpha\right)} {a\cos \alpha}\right)^{-2k}&= \frac{(1-\sin \alpha)\left(\sqrt{u^2+1}+\sin \alpha+u \cos \alpha\right)}{(1+\sin \alpha)\left(\sqrt{u^2+1}-\sin \alpha-u \cos \alpha\right)} \\
\end{align}
$$



This is an implicit equation. I can’t simplify more than that. So let’s just plot a few values of $\alpha$. 

It simplifies quite a bit if $\alpha=0$. Ends up just like [[#^5d2bb9]]
$$
\begin{align}
\left(\frac {x} {a}\right)^{-2k}&= \frac{\sqrt{u^2+1}+u }{\sqrt{u^2+1}-u} \\
&= \frac{\sqrt{u^2+1}+u }{\sqrt{u^2+1}-u} \frac{\sqrt{u^2+1}+u }{\sqrt{u^2+1}+u} \\
&= \frac{\left(\sqrt{u^2+1}+u\right)^2}{u^2+1-u^2}  \\
\left(\frac {x} {a}\right)^{-2k}&= \left(\sqrt{u^2+1}+u\right)^2 \\
\left(\frac {x} {a}\right)^{-k}&= \sqrt{u^2+1}+u \\
\end{align}
$$


## Wind direction at an angle in Polar Coordinates

It's much easier to calculate the angle of the wind if we take it from the x-axis instead of the y-axis. We would arrive at the same result, but it's a bit uglier and longer.


```tikz
\usepackage{pgfplots}
\usetikzlibrary{calc}
\pgfplotsset{compat=1.16,width=20cm,height=10cm}
\begin{document} 
    \begin{tikzpicture}
        \newcommand\A{2500};
        \newcommand\W{100};
        \newcommand\V{500};
        \newcommand\K{\W/\V};
        \newcommand\X{1200};
        \newcommand\windangle{30};
        \newcommand\prop{0.6};
        \pgfmathsetmacro\cprop{1 - \prop}; 
        \begin{axis}[
            axis line style={line width=1.5pt},
            draw={rgb,255:red,209;green,209;blue,209}, 
            text={rgb,255:red,209;green,209;blue,209}, 
            fill={rgb,255:red,209;green,209;blue,209}, 
            axis lines=middle,
            samples=200,
            enlargelimits,
            ticks=none,
            declare function={
                func(\x)=((\A/2)*((\x/\A)^(1-\K)- (\x/\A)^(1+\K));
                derivative(\x)=(
                    (1/2)*( 
                        (1-\K)*( 
                            (\x/\A)^(-\K) 
                        ) - 
                        (1+\K)*(
                            (\x/\A)^(\K)
                        )
                    )
                );
            }
        ]
        \pgfmathparse{\X+0.8};
        \pgfmathsetmacro\slope{derivative(\X)};
        \pgfmathsetmacro\Y{func(\X)};
        \pgfmathsetmacro\angle{atan(\Y/\X)};
        
        \pgfmathsetmacro\vstartx{\X};
        \pgfmathsetmacro\vstarty{\Y};
        \pgfmathsetmacro\vendx{\X*\prop};
        \pgfmathsetmacro\vendy{func(\X)*\prop};

        \pgfmathsetmacro\dstartx{\X};
        \pgfmathsetmacro\dstarty{\Y};
        \pgfmathsetmacro\dendx{\X*\prop};
        \pgfmathsetmacro\dendy{\Y-\X*\slope*\cprop};

        \pgfmathsetmacro\wstartx{\X};
        \pgfmathsetmacro\wstarty{\Y};
        \pgfmathsetmacro\wrendx{\X*1.3};
        \pgfmathsetmacro\wrendy{func(\X)*1.3};

        % Calculate the aspect ratio 
        \pgfmathsetmacro{\aspectRatio}{10/20}; % height/width 


        \pgfmathsetmacro\wtstartx{\wrendx};
        \pgfmathsetmacro\wtstarty{\wrendy};
        \pgfmathsetmacro\MMM{-1/((\wrendy - \wstarty)/((\wrendx-\wstartx)*\aspectRatio))};
        \pgfmathsetmacro\AAA{(\wrendx-\wstartx)*0.3};
        \pgfmathsetmacro\wtendx{\wrendx - \AAA};
        \pgfmathsetmacro\wtendy{\wrendy - \MMM * \AAA * 0.13};
        
        \pgfmathsetmacro\wyendx{\X};
        \pgfmathsetmacro\wyendy{\Y + (\dendy - \vendy)};
        \pgfmathsetmacro\wx{\dendy - \vendy}; 
        \pgfmathsetmacro\wxendx{\X*(1+\cprop*0.8) };
        \pgfmathsetmacro\wxendy{\Y};
        
        \addplot[
            ultra thick,
            domain=\A:\X,
            draw={rgb,255:red,163;green,190;blue,140},
        ] {func(x)}
            node [
                pos=0, 
                below, 
                font=\LARGE,
            ] {$\left(a, 0\right)$};

        \node at (0,0) [font=\LARGE, below right] {$T(0,0)$};
        
        \addplot[] 
            coordinates {(0,0) (\X, {func(\X)})};
        
        \addplot[]  
            coordinates {(\X,0) (\X, {func(\X)})}
            node [pos=0.4, right, font=\Large] {$y$};

        \addplot[]  
            coordinates {(0,0) (\X, 0)}
            node [pos=0.5, below, font=\Large] {$x$};

        % plane direction
        \addplot[
            ->, 
            -latex, 
            line width=1.5pt,
        ] 
            coordinates {(\vstartx, \vstarty) (\X*\prop, {func(\X)*\prop})} 
            node [pos=0.5, font=\Large, above left] {$\mathbf{v}$};
 
            
        % Direction of the wind
        \addplot[
            -, 
            line width=1pt,
        ] 
            coordinates {(\wstartx, \wstarty) (\wyendx, \wyendy)};


        \addplot[
            ->, 
            -latex, 
            line width=1pt,
        ] 
            coordinates {(\wstartx, \wstarty) (\wrendx, \wrendy)}
            node [pos=0.7, font=\large, right] {$\mathbf{w}\cos(\alpha-\theta)$};

        \addplot[
            ->, 
            -latex, 
            line width=1pt,
        ] 
            coordinates {(\wtstartx, \wtstarty) (\wtendx, \wtendy)}
            node [pos=0.5, font=\large, right] {$\mathbf{w}\sin(\alpha-\theta)$};
        
        \addplot[
            ->, 
            -latex, 
            line width=1pt,
        ] 
            coordinates {(\wstartx, \wstarty) (\wtendx, \wtendy)} 
            node [pos=0.8, above left, font=\Large] {$\mathbf{w}$}
            node [above right, font=\Large] {Wind Direction};
        
        \addplot[
            -, 
            line width=1pt,
        ] 
            coordinates {(\X, \Y) (\wxendx, \wxendy)};
            
        \draw[->, line width=1.1pt] (rel axis cs:0.9,0.41) -- (rel axis cs:0.9,0.5) 
        node[left,pos=0.5, font=\Large] {$+$};
        \draw[->, line width=1.1pt] (rel axis cs:0.91,0.4) -- (rel axis cs:1,0.4) 
        node[below,pos=0.5, font=\Large] {$+$};

        \coordinate (A) at (axis cs:0,0); 
        \coordinate (B) at (axis cs:\X,\Y); 
        \draw[thick] 
        let 
            \p1 = (B), 
            \n1 = {atan2(\y1,\x1)},
    
        in (A) ++(0:1.5cm) arc[start angle=0, end angle=\n1, radius=1.5cm] 
            node[midway, right, font=\Large] { \( \theta \) };
    
        \coordinate (D) at (axis cs:\X,\Y); 
        \coordinate (E) at (axis cs:\wtendx,\wtendy); 
        \coordinate (F) at (axis cs:\wrendx,\wrendy); 
        \draw[thick] 
        let 
            \p1 = ($(F)-(D)$), 
            \p2 = ($(E)-(D)$), 
            \n1 = {atan2(\y1,\x1)},
            \n2 = {atan2(\y2,\x2)},
        in (D) ++(0:0.8cm) arc[start angle=0, end angle=\n2, radius=0.8cm] 
            node[pos=0.8, above right, font=\Large] {$\alpha$};

        \coordinate (G) at (axis cs:\wxendx,\Y); 
        \draw[thick] 
        let 
            \p1 = ($(F)-(D)$), 
            \p2 = ($(G)-(D)$), 
            \n1 = {atan2(\y1,\x1)},
            \n2 = {atan2(\y2,\x2)},
        in (D) ++(\n1:1cm) arc[start angle=\n1, end angle=\n2, radius=1cm] 
            node[midway,  right, font=\Large] {$\theta$};

    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```





$$
\frac{dr}{dt}=-v+w \cos(\alpha-\theta),\qquad r\frac{d \psi}{d t}=w \sin(\alpha-\theta) \\
$$

$$
\begin{align}
\frac{d r}{r d \theta}&=\frac{-v+w \cos(\alpha-\theta)}{w \sin(\alpha-\theta)}=\frac{k+\cos (\alpha-\theta)}{\sin (\alpha-\theta)} \\
\int \frac{1}{r} d r&=\int \frac{k}{\sin(\alpha-\theta)} d \theta+\int \cot (\alpha-\theta) d \theta \\
\ln r+C&=k \left[\ln \left(\cos\left(\frac{\alpha-\theta}{2}\right)\right)-\ln\left(\sin \left(\frac{\alpha-\theta}{2}\right)\right)\right]  - \ln\left(\sin\left(\alpha-\theta\right)\right)\\
&= k\ln \left(\frac{\cos\left(\frac{\alpha-\theta}{2}\right)}{\sin \left(\frac{\alpha-\theta}{2}\right)}\right) - \ln\left(\sin\left(\alpha-\theta\right)\right)\\
&= \ln \frac{
    \left(\frac{
        \cos\left(\frac{\alpha-\theta}{2}\right)
        }{
        \sin \left(\frac{\alpha-\theta}{2}\right)
        }
    \right)^k
    }{
    \sin\left(\alpha-\theta\right)
    }
\\

Cr&= \frac{
    \left(\frac{
        \cos\left(\frac{\alpha-\theta}{2}\right)
        }{
        \sin \left(\frac{\alpha-\theta}{2}\right)
        }
    \right)^k
    }{
    \sin\left(\alpha-\theta\right)
    }
\\
\end{align}
$$


When $t=0\rightarrow r=a, \theta = 0 \rightarrow \psi=\frac{\pi}{2}-\alpha$
$$
\begin{align}
C&=\frac{1}{a} \frac{
    \left(\frac{
        \cos\left(\frac{\alpha-0}{2}\right)
        }{
        \sin \left(\frac{\alpha-0}{2}\right)
        }
    \right)^k
    }{
    \sin\left(\alpha-0\right)
    } \\
&=\frac{1}{a} \frac{
    \left(\frac{
        \cos\left(\frac{\alpha}{2}\right)
        }{
        \sin \left(\frac{\alpha}{2}\right)
        }
    \right)^k
    }{
    \sin\left(\alpha\right)
    } \\
\end{align}
$$


$$
\begin{align}
r &= \frac{
\displaystyle\frac{
    \left(\displaystyle\frac{
        \cos\left(\frac{\alpha-\theta}{2}\right)
        }{
        \sin \left(\frac{\alpha-\theta}{2}\right)
        }
    \right)^k
    }{
    \sin\left(\alpha-\theta\right)
    }}
    {\displaystyle\frac{1}{a} \frac{
    \left(\displaystyle\frac{
        \cos\left(\frac{\alpha}{2}\right)
        }{
        \sin \left(\frac{\alpha}{2}\right)
        }
    \right)^k
    }{
    \sin\left(\alpha\right)
    }} \\
&= \displaystyle\frac{
    \left(\displaystyle\frac{
        \cos\left(\frac{\alpha-\theta}{2}\right)
        }{
        \sin \left(\frac{\alpha-\theta}{2}\right)
        }
    \right)^k  a\sin\left(\alpha\right)
    }{
    \left(\displaystyle\frac{
        \cos\left(\frac{\alpha}{2}\right)
        }{
        \sin \left(\frac{\alpha}{2}\right)
        }
    \right)^k \sin\left(\alpha-\theta\right)
    } \\
&= a\left(\displaystyle\frac{
        \displaystyle \cos\left(\frac{\alpha-\theta}{2}\right) \sin \left(\frac{\alpha}{2}\right)
        }{
        \displaystyle \sin \left(\frac{\alpha-\theta}{2}\right)\cos\left(\frac{\alpha}{2}\right)
        }
    \right)^k 
    
    \frac{ \sin\left(\alpha\right)}{
   \sin\left(\alpha-\theta\right)
    }
\end{align}
$$

Final answer,
- $\alpha$ is the angle of the wind with the x-axis (constant)
- $\theta$ is the angle of the plane to the x-axis (also the angle the nose of the plane makes with the x-axis)
- $a$ is the initial distance of the plane when it leaves (at $t=0$) (constant)
- $v$ is the plane velocity (constant)
- $w$ is the wind speed (constant)
$r$, a function of $\theta$, gives the distance of the plane to its destination. 

$$
\boxed{r\left( \theta\right)= a\left(\displaystyle\frac{
        \displaystyle \cos\left(\frac{\alpha-\theta}{2}\right) \sin \left(\frac{\alpha}{2}\right)
        }{
        \displaystyle \sin \left(\frac{\alpha-\theta}{2}\right)\cos\left(\frac{\alpha}{2}\right)
        }
    \right)^{\textstyle -\frac{v}{w}}
    
    \frac{ \sin\left(\alpha\right)}{
   \sin\left(\alpha-\theta\right)
    }}
$$

##### When the wind is fully vertical
In this case, the angle of the wind is calculated from the x-axis. So a vertical wind is at $\displaystyle \alpha = \frac{\pi}{2}$. This is to see if we get the same answer as above 

$$
\begin{align}
r = &\left(\frac{\cos \left(\frac{\alpha-\theta}{2}\right) \sin \frac{\alpha}{2}}{\sin \left(\frac{\pi-\theta}{2}\right) \cos \left(\frac{\alpha}{2}\right)}\right)^k \frac{\sin \alpha}{\sin (\alpha-\theta)} \\
=&\left(\frac{\cos \left(\frac{\pi}{4}-\frac{\theta}{2}\right) \cancel{\sin \frac{\pi}{4}}}{\sin \left(\frac{\pi}{4}-\frac{\theta}{2}\right) \cancel{\cos \frac{\pi}{4}}}\right)^k \frac{\cancelto{1}{\sin \frac{\pi}{2}}}{\sin \left(\frac{\pi}{2}-\theta\right)} \\
\end{align}
$$
Because $\displaystyle \sin\frac{\pi}{4} = \cos\frac \pi 4$  we can simplify this a little bit everywhere.
$$
\begin{align}
r=&\left(\frac{\cancel{\cos \frac{\pi}{4}} \cos \frac{\theta}{2}+\cancel{\sin \frac{\pi}{4}} \sin \frac{\theta}{2}}{\cancel{\sin \frac{\pi}{4}} \cos \frac{\theta}{2}-\cancel{\cos \frac{\pi}{4}} \sin \frac{\theta}{2}}\right)^k \frac{1}{\cos \theta} \\
=& \left(\frac{\cos \frac{\theta}{2}+\sin \frac{\theta}{2}}{\cos \frac{\theta}{2}-\sin \frac{\theta}{2}}\right)^k \frac{1}{\cos \theta} \\
\end{align}
$$
After using the [[../../../Calculus/Integration/Trigonometry|Half Angle]] property of both $\sin$ and $\cos$, we can simplify the $\sqrt{2}$ in the numerator and denominator
$$
\begin{align}
r=& \left(\frac{\sqrt{\frac{1+\cos \theta}{2}}+\sqrt{\frac{1-\cos \theta}{2}}}{\sqrt{\frac{1+\cos \theta}{2}}-\sqrt{\frac{1-\cos \theta}{2}}}\right)^{k} \frac{1}{\cos \theta} \\
=& \left(\frac{\sqrt{1+\cos \theta}+\sqrt{1-\cos \theta}}{\sqrt{1+\cos \theta}-\sqrt{1-\cos \theta}}\right)^{k} \frac{1}{\cos \theta} \\
\end{align}
$$

Then, using the difference of squares, the denominator becomes more manageable. 
$$
\begin{align}
r=& \left(\frac{\sqrt{1+\cos \theta}+\sqrt{1-\cos \theta}}{\sqrt{1+\cos \theta}-\sqrt{1-\cos \theta}}
\times \frac{\sqrt{1+\cos \theta}+\sqrt{1-\cos \theta}}{\sqrt{1+\cos \theta}+\sqrt{1-\cos \theta}} \right)^{k} \frac{1}{\cos \theta} \\
=& \left(\frac{1+\cancel{\cos \theta}+1-\cancel{\cos \theta} + 2(\sqrt{1+\cos \theta}\sqrt{1-\cos \theta})}{\cancel{1}+\cos \theta-\cancel{1}+\cos \theta} \right)^{k} \frac{1}{\cos \theta} \\
=& \left(\frac{2+ 2\left(\sqrt{1-\cos^2 \theta -\cancel{\cos \theta}+\cancel{\cos \theta}}\right)}{2\cos\theta} \right)^{k} \frac{1}{\cos \theta} \\
=& \left(\frac{1+ \sqrt{1-\cos^2 \theta}}{\cos\theta} \right)^{k} \frac{1}{\cos \theta} \\
=& \left(\frac{1+ \sin \theta}{\cos\theta} \right)^{k} \frac{1}{\cos \theta} \\
=& \left(\sec \theta + \tan \theta  \right)^{k} \frac{1}{\cos \theta} \\
\end{align}
$$