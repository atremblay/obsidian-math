# Flying Plane Subjected to Wind (Cartesian)

## Vertical Wind
^07afa4

A pilot always keeps the nose of his plane pointed toward a city T due west of his starting point. If his speed is $v$ miles per hour and a wind is blowing from the south at the rate of $w$ miles per hour, find the equation of the plane's path. Assume that it starts from a flying field which is at a distance $a$ miles from $T$.

### Solution in Cartesian Coordinates
#example 

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

This equation is of the [[../../../Definitions/Homogeneous Function|homogeneous type]]. It can therefore be solved by [[../../../Solutions/Variable Substitution in DE|Variable Substitution in DE]]. Note that $x,y\ge 0$ since we are working with positive coordinates.
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

Using the [[../../../../Calculus/Integration/Cheatsheet#^9a7c4a|integral]] on (j)
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


##### Flip the script
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

Solving from (b) puts us on the fast track to ugly-town. We can technically use [[../../../Solutions/Variable Substitution in DE|Variable Substitution in DE]] on 

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






