
 

> [!Definition] Pursuit Curve
> The path traced by a body which always moves in the direction of a fixed point or of another moving object is called a pursuit curve.

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
k \sqrt{1+u^2}-u+u^{\prime} x+u=0 \\
u^{\prime} x=-k \sqrt{1+u^2} \\
x=-\frac{k \sqrt{1+u^2}}{u^{\prime}} \\
\frac{1}{x}=-\frac{1}{k \sqrt{1+u^2}} \frac{d u}{d x} \\
-k\int \frac{1}{x} d x=\int \frac{1}{\sqrt{1+u^2}} \frac{d u}{d x} d x \\
-k\int \frac{1}{x} d x= \int \frac{1}{\sqrt{1+u^2}} d u \tag{i}\\
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
-k[\log (x)-\log (a)]&=\left[\log \left(u+\sqrt{1+u^2}\right)-\log\left(0+\sqrt{1+c^2}\right)\right] \tag{k} \\
\end{align}
$$
Simplifying (k) a bunch

$$
\begin{align}
-k \log \left(\frac{x}{a}\right)&=\log \left(u+\sqrt{1+u^2}\right) \\
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



## Example 
### Statement
Solve the problem of Example 17.1 by using polar coordinates.

### Solution 
The components of the wind velocity $w$ in the radial and transverse directions are respectively [for the derivation of these formulas, see Lesson 34, (34.2)].

$$
\frac{d r}{d t}=w \sin \theta, \quad r \frac{d \theta}{d t}=w \cos \theta .
$$
![[Pasted image 20230822152639.png]]
Figure 17.21

Hence the effective velocity of the plane in the radial direction, taking into account the wind's speed $w$ and the plane's speed $v$, is

$$
\frac{d r}{d t}=-v+w \sin \theta
$$

Dividing (b) by the second equation in (a), we obtain

$$
\frac{d r}{r d \theta}=\frac{-v+w \sin \theta}{w \cos \theta}=-\frac{v}{w} \sec \theta+\tan \theta .
$$

Let

$$
k=-\frac{v}{w} .
$$

Then (c) becomes

$$
\frac{d r}{r}=(k \sec \theta+\tan \theta) d \theta
$$

Its solution, by integration, is

$$
\log r+\log c=k \log (\sec \theta+\tan \theta)-\log \cos \theta,
$$

which can be written as

$$
c r=(\sec \theta+\tan \theta)^{k} \sec \theta .
$$

At $t=0, r=a, \theta=0$. Substituting these values in (g), we have

$$
c a=1, \quad c=1 / a .
$$

The equation of the path is, therefore, after replacing $k$ by its value in (d),

$$
r=a(\sec \theta+\tan \theta)^{-v / w} \sec \theta
$$

or

$$
r \cos \theta=a(\sec \theta+\tan \theta)^{-v / w} .
$$

## Example

### Statement

A pilot always keeps the nose of his plane pointed toward a city T due west of his starting point. If his speed is o miles per hour and a wind is blowing with a velocity $w$ in a direction which makes an angle $\alpha$ with the vertical, find the equation of the plane's path. Assume that it starts from a flying field which is at a distance $a$ miles from $T$.

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
![[Pasted image 20230821142443.png|800]]

Figure 17.32

The differential equations of motion, however, are exactly the same as those in Example 17.1, namely

$$
\frac{d x}{d t}=-v \cos \theta, \quad \frac{d y}{d t}=-v \sin \theta+w .
$$

Proceeding just as we did in Example 17.1, we obtain,

$$
\log \left(u+\sqrt{1+u^{2}}\right)=-k \log x+\log c, \quad k=w / v .
$$

But now at $t=0, x=a \cos \alpha, u=y / x=(a \sin \alpha) /(a \cos \alpha)=\tan \alpha$. Substituting these values in (c), and solving for $c$, we obtain

$$
c=(\tan \alpha+\sec \alpha)(a \cos \alpha)^{k}
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
Figure 17.33

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
            node [font=\large, below right] {$\mathbf{w}_x=w\sin(\alpha)$};
            
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


Method 2 (Fig. 17.33). From Fig. 17.33, we see that the resultant of the effective velocities of wind and plane in the $x$ and $y$ directions are respectively

$$
\frac{d x}{d t}=-v \cos \theta+w \sin \alpha, \quad \frac{d y}{d t}=-v \sin \theta+w \cos \alpha .
$$

Dividing the second equation in (h) by the first we obtain

$$
\frac{d y}{d x}=\frac{-v \sin \theta+w \cos \alpha}{-v \cos \theta+w \sin \alpha}
$$

In (i) replace $\sin \theta$ by its value $y / \sqrt{x^{2}+y^{2}}$ and $\cos \theta$ by $x / \sqrt{x^{2}+y^{2}}$. It then simplifies to

$$
\text { (j) }\left(\frac{-v y}{\sqrt{x^{2}+y^{2}}}+w \cos \alpha\right) d x=\left(-\frac{v x}{\sqrt{x^{2}+y^{2}}}+w \sin \alpha\right) d y \text {, }
$$

which is a homogeneous equation. Remember $v, w \cos \alpha$, and $v \sin \alpha$ are constants. Hence it can be solved by the method of Lesson 7 . The initial conditions are $x=a, y=0$. We leave it to you as an exercise to complete.

## 1. EXERCISE 17A

1. A man swims across a river 100 it wide, always heading for a tree directly across from his starting point. He can swim at the rate of $3 \mathrm{ft} / \mathrm{sec}$.

(a) Find the equation of his path if the current is carrying him downstream at the rate of $1 \mathrm{ft} / \mathrm{sec} ; 3 \mathrm{ft} / \mathrm{sec} ; 4 \mathrm{ft} / \mathrm{sec}$.

(b) Draw the graph of each equation. Solve independently and check your answers with (r) of Example 17.1.

2. In problem 1 show that the man will reach the tree on the opposite bank if the current is $1 \mathrm{ft} / \mathrm{sec}$, that he will reach a point on the opposite shore $50 \mathrm{ft}$ from the tree if the current is $3 \mathrm{ft} / \mathrm{sec}$, that he will never reach the opposite shore, if the current is $4 \mathrm{ft} / \mathrm{sec}$.
3. Solve problem 1 by using polar coordinates. Solve independently and check your answers with (j) of Example 17.2.
4. An insect steps on the edge of a turntable of radius $a$ that is rotating at a constant angular velocity $\alpha$. It moves straight toward the center of the table at a constant velocity $v_{0}$. Find the equation of its path in polar coordinates, relative to axes fixed in space. (Hint. If $\theta$ is the angle through which the turntable has rotated in time $t$ and $r$ is the distance of the insect from the center at that moment, then $d \theta / d t=\alpha, r(d \theta / d t)=r \alpha$.) Draw a graph of the equation if $a=10 \mathrm{ft}, \alpha=\pi / 4$ radians/sec, $v_{0}=1 \mathrm{ft} / \mathrm{sec}$. How many revolutions will the table have made by the time the insect reaches the center?
5. Assume in problem 4 that the insect always moves in a direction parallel to the diameter drawn through the point where he steps on the table.

(a) Find the equation of its path relative to axes fixed in space.

(b) What kind of curve is it? Hint. Change the equation to rectangular coordinates if the polar form is unfamiliar to you.

6. A boy stands at A, Fig. 17.34. By means of a string $l \mathrm{ft}$ long, he holds a boat, which is in the water at $B$. He starts walking in a direction perpendicular to $A B$, always keeping the string taut. Find the equation of the boat's path. Ignore the height of the boy above the horizontal and assume that the string is always tangent to the path. The resulting curve is known as a tractrix. Hint. The slope of the tractrix is: $\tan \theta=d y / d x$.

![](https://cdn.mathpix.com/cropped/2023_07_30_ca1c1f7b3544dd2eae87g-190.jpg?height=345&width=470&top_left_y=1061&top_left_x=357)

Figure 17.34

7. A pilot always keeps the nose of his plane pointed toward a city which is 400 $\mathrm{mi}$ due west of him. A wind is blowing from the south at the rate of $20 \mathrm{mi} / \mathrm{hr}$. His speed is $300 \mathrm{mi} / \mathrm{hr}$. Find the equation of his path. Solve independently, by using both rectangular and polar coordinates. Check the accuracy of your answers with (r) of Example 17.1 and (j) of Example 17.2.
8. A pilot always keeps the nose of his plane pointed toward a city which is a $\mathrm{mi}$ due north of him. His speed is $v \mathrm{mi} / \mathrm{hr}$ and a wind is blowing from the west at $w \mathrm{mi} / \mathrm{hr}$. Find the equation of his path.
9. Solve problem 8 if the city is $a$ mi due south of the pilot.
10. A pilot always keeps the nose of his plane pointed toward a city which is $300 \mathrm{mi}$ due west of him. A $25-\mathrm{mi} / \mathrm{hr}$ wind is blowing in a direction whose slope is $\frac{4}{3}$. His speed is $200 \mathrm{mi} / \mathrm{hr}$. Find the equation of his path. Solve independently. Use both methods outlined in this lesson. Check the accuracy of your differential equations with (b) and (i) of Example 17.3.
11. Solve problem 10 if the wind is blowing in a direction whose slope is -4 . Use method 2 of Example 17.3. Solve independently and then check the accuracy of your differential equation with (j) of this example.
12. Assume in problem 4 that the insect moves straight toward a light which is fixed in space directly above the end of the diameter drawn through the point where it steps on the table. Find the differential equation of its path in polar coordinates.

## 2. ANSWERS $17 \mathrm{~A}$

1. $y=50\left[\left(\frac{x}{100}\right)^{2 / 3}-\left(\frac{x}{100}\right)^{4 / 3}\right] ; x^{2}=-200(y-50)$;

$y=50\left[\left(\frac{x}{100}\right)^{-1 / 3}-\left(\frac{x}{100}\right)^{7 / 3}\right]$

3. $r=100(1-\sin \theta) /(1+\sin \theta)^{2} ; r=100 /(1+\sin \theta)$;

$r \cos \theta=100(\sec \theta+\tan \theta)^{-3 / 4}$.

4. $r=a-\frac{v_{0}}{\alpha} \theta$ (origin at center). $1 \frac{1}{4}$ revolutions.
5. (a) $2 v_{0}(r \sin \theta)=\alpha\left(a^{2}-r^{2}\right)$. (b) Circle: center $\left(0,-v_{0} / \alpha\right)$, radius $\sqrt{a^{2}+v_{0}^{2} / \alpha^{2}}$.
6. $x=l \log \frac{l+\sqrt{l^{2}-y^{2}}}{y}-\sqrt{l^{2}-y^{2}}$.
7. $y=200\left[\left(\frac{x}{400}\right)^{14 / 15}-\left(\frac{x}{400}\right)^{16 / 15}\right] ; r \cos \theta=400(\sec \theta+\tan \theta)^{-15}$.
8. $x=\frac{a}{2}\left[\left(\frac{y}{a}\right)^{1-(w / v)}-\left(\frac{y}{a}\right)^{1+(w / v)}\right]$ with positive directions to the east and to the south.
9. Same as 8 with positive directions to the east and to the north.
10. First method: solution is $y+\sqrt{x^{2}+y^{2}}=2(240)^{1 / 8} x^{7 / 8}$.

Second method: Differential equation is

$$
\left(200 y-20 \sqrt{x^{2}+y^{2}}\right) d x-\left(200 x-15 \sqrt{x^{2}+y^{2}}\right) d y=0 .
$$

11. Differential equation is

$$
\left(200 y-20 \sqrt{x^{2}+y^{2}}\right) d x-\left(200 x+15 \sqrt{x^{2}+y^{2}}\right) d y=0 .
$$

12. $\frac{d r}{d \theta}=-\frac{v_{0} r \cos (\theta-\psi)}{r \alpha+v_{0} \sin (\theta-\psi)}$, where $\psi$ is the angle made by the original diameter and a line drawn from its end (i.e., where the light is) to the insect's position.

LESSON 17B. Relative Pursuit Curve. If the origin of a coordinate system is not fixed on the ground but is attached to a pursued object, then the path described by a pursuer, moving always in the direction of the object he is pursuing, is called a relative pursuit curve. It is, in effect, the path traced by the pursuing body as seen by an observer in the pursued object.

Example 17.4. A fighter plane whose speed is $V_{F}$ is chasing a bomber plane whose speed is $V_{B}$. The nose of the fighter plane is always pointed toward the bomber which is flying in a direction making an angle $\beta$ with the horizontal. Find the path traced by the fighter as plotted by an observer in the bomber.

Solution (Fig. 17.41). Call $(0,0)$ the origin of a coordinate system fixed on the ground, and $(\overline{0}, \overline{0})$ the origin of a coordinate system which is moving with the bomber plane. The position of the fighter plane at any

![](https://cdn.mathpix.com/cropped/2023_07_30_ca1c1f7b3544dd2eae87g-192.jpg?height=568&width=1053&top_left_y=612&top_left_x=73)

Figure 17.41

time $t$ is therefore given by two sets of coordinates, one $(x, y)$ with respect to the fixed ground origin $(0,0)$ and the other, $(x, y)$ with respect to the moving origin $(0,0)$.

As measured from an observer on the ground, the effective velocities of the fighter plane in the $x$ and $y$ directions are (see Fig. 17.41)

$$
\begin{aligned}
& \frac{d x}{d t}=V_{F} \cos (\pi-\theta)=-V_{F} \cos \theta, \\
& \frac{d y}{d t}=-V_{F} \sin (\pi-\theta)=-V_{F} \sin \theta .
\end{aligned}
$$

If two planes are approaching each other along a straight line, then to an observer in one plane, it will seem as if he were standing still and the other plane were coming toward him with a speed equal to the sum of the speeds of the two planes. If the two planes are moving away from each other along a straight line, then it will seem to an observer in one plane as if he were standing still and the other plane were going away from him with a speed equal to the sum of the speeds of the two planes. If, however, the planes are going in the same direction along a straight line, then to an observer in a pursued plane it will seem, if his speed is slower than the other, as if he were standing still and the other plane were coming toward him with a speed equal to the difference of the speeds of the two planes. If his speed is greater than the other, then it will seem to him as if he were standing still and the other plane were going away from him with a speed equal to the difference of the speeds of the two planes.

Here the horizontal component of fighter and bomber velocities are in the same direction. Hence (see Fig. 17.41), to an observer in the bomber (we assume the fighter's component is greater than the bomber's), it is as if he were standing still, and the fighter plane were coming toward him in the positive $x$ direction with a velocity equal to the difference of their two $x$ components of velocity, i.e., the rate of change of $x$ to an observer in the bomber is

$$
\frac{d x}{d t}=-V_{F} \cos \theta-V_{B} \cos \beta
$$

The vertical components of fighter and bomber velocities are in opposite directions and pointed toward each other. Hence, the velocity of the fighter relative to an observer in the bomber, is in the negative $y$ direction and is the sum of their two vertical velocities, i.e., the rate of change of $\eta$ is

$$
\frac{d y}{d t}=-\left(V_{F} \sin \theta+V_{B} \sin \beta\right)
$$

Dividing (c) by (b), we obtain

$$
\frac{d y}{d x}=\frac{V_{F} \sin \theta+V_{B} \sin \beta}{V_{F} \cos \theta+V_{B} \cos \beta},
$$

which has the same form as (i) of Example 17.3. Remember $V_{F}, V_{B} \sin \beta$, and $V_{B} \cos \beta$ are constants. Hence it can be solved by the method suggested there. We leave it to you as an exercise to complete. The initial conditions will depend on the distance and direction of the fighter plane from the bomber plane at $t=0$, i.e., at the moment when the pursuit began.

Example 17.5. Solve the problem of Example 17.4 by using polar coordinates.

Solution (Fig. 17.51). To an observer in the bomber, when he moves to the right it appears to him as if he were standing still and the fighter plane were moving to the left. Hence, to an observer in the bomber, when he moves with a velocity $\mathbf{V}_{B}$, represented by the lower vector in Fig. 17.51, it appears to him as if he were standing still and the fighter is

![](https://cdn.mathpix.com/cropped/2023_07_30_ca1c1f7b3544dd2eae87g-194.jpg?height=436&width=991&top_left_y=288&top_left_x=104)

Figure 17.51

moving with a velocity $\mathbf{V}_{B}$ represented by the upper vector in this figure. The radial and transverse components of the upper $\mathbf{V}_{B}$ are

$$
\frac{d r}{d t}=-V_{B} \cos (\theta-\beta), \quad r \frac{d \theta}{d t}=V_{B} \sin (\theta-\beta) .
$$

As seen from an observer in the bomber, therefore, the effective velocity of the fighter in the radial direction is (remember it is the sum of the two velocities in the same negative $r$ direction)

$$
\frac{d r}{d t}=-V_{F}-V_{B} \cos (\theta-\beta)
$$

and in the transverse direction is

$$
r \frac{d \theta}{d t}=V_{B} \sin (\theta-\beta)
$$

Dividing (b) by (c), we obtain

$$
\frac{d r}{r d \theta}=-\frac{\cos (\theta-\beta)}{\sin (\theta-\beta)}-\frac{V_{F}}{V_{B}} \csc (\theta-\beta) .
$$

Its solution is

(e) $\log r+\log c$

$$
=-\log \sin (\theta-\beta)-\frac{V_{F}}{V_{B}} \log [\csc (\theta-\beta)-\cot (\theta-\beta)],
$$

which simplifies to

$$
\begin{aligned}
c r & =\frac{[\csc (\theta-\beta)-\cot (\theta-\beta)]^{-V_{P} / V_{B}}}{\sin (\theta-\beta)} \\
& =\frac{1}{\sin (\theta-\beta)}\left[\frac{1-\cos (\theta-\beta)}{\sin (\theta-\beta)}\right]^{-V_{F} / V_{B}} \\
& =\frac{1}{[\sin (\theta-\beta)]^{1-\left(V_{F} / V_{B}\right)}[1-\cos (\theta-\beta)]^{V_{F} / V_{B}}} \cdot
\end{aligned}
$$

Nore. The initial conditions are $t=0, r=r_{0}, \theta=\theta_{0}$.

Comment 17.52. As viewed by an observer in the bomber, it is as if he were standing still, and the path of the fighter as given in (f) (which is the resultant of both planes' motions) were due entirely to the fighter plane's movements.

Comment 17.521. By means of (f), we can draw a rough graph of the fighter plane's path, as seen from the observer in the bomber.

Case 1. If $V_{B}=V_{F}$, i.e., if the ground speed of both planes are the same, then $(f)$ reduces to

$$
c r=\frac{1}{1-\cos (\theta-\beta)},
$$

which is the equation of a parabola [see (34.68) and comment following].

A rough graph of the path of the fighter plane as viewed from the bomber is given for this case in Fig. 17.53. It is evident that the fighter will never reach the bomber.

![](https://cdn.mathpix.com/cropped/2023_07_30_ca1c1f7b3544dd2eae87g-195.jpg?height=668&width=949&top_left_y=1109&top_left_x=129)

Case 2. If $V_{F}>V_{B}$, then $1-\left(V_{F} / V_{B}\right)<0$, and (f) can be written as

$$
c r=\frac{[\sin (\theta-\beta)]^{\left(V_{R} / V_{B}\right)-1}}{[1-\cos (\theta-\beta)]^{V_{F} / V_{B}}}
$$

A rough graph of the fighter plane as viewed from the bomber is given for this case in Fig. 17.54 (with $V_{F}=2 V_{B}$ ).

![](https://cdn.mathpix.com/cropped/2023_07_30_ca1c1f7b3544dd2eae87g-196.jpg?height=699&width=902&top_left_y=468&top_left_x=160)

Figure 17.54

## 3. EXERCISE 17B

1. An airplane $A$ is flying with a speed of $200 \mathrm{mi} / \mathrm{hr}$ in a direction whose slope is 3. A plane $B, 50$ miles due north of him, starts in pursuit. Plane $B$, whose speed is $300 \mathrm{mi} / \mathrm{hr}$, always keeps the nose of his plane pointed toward $A$. Find the equation of $B$ 's path as observed from $A$. Use rectangular and polar coordinates. Hint. In polar coordinates at $t=0, \theta=\pi / 2, \tan \beta=\frac{3}{4}, r=50$.
2. Solve problem 1 , in polar coordinates only, if $A$ is fying in a southeasterly direction and plane $B$, at the instant he starts in pursuit, is 50 miles northeast of $A$. Hint. At $t=0, \theta=\pi / 4, \beta=-\pi / 4, r=50$. Make a figure corresponding to 17.51 . Be very careful of signs.
3. Solve problem 1 , in polar coordinates only, if $A$ is flying in a northeasterly direction and plane $B$, at the instant he starts in pursuit, is 50 miles southeast of $A$. Hint. At $t=0, \theta=-\pi / 4, \beta=\pi / 4, r=50$. Make a figure corresponding to Fig. 17.51. Be very careful of signs.

## 4. ANSWERS 17B

1. Differential equation in rectangular coordinates is

$$
\frac{d \bar{y}}{d \bar{x}}=\frac{300 \bar{y}+120 \sqrt{\bar{x}^{2}+\bar{y}^{2}}}{300 \bar{x}+160 \sqrt{\bar{x}^{2}+\bar{y}^{2}}} \text { with } t=0, \bar{x}=0, \bar{y}=50 \text {. }
$$

Solution in polar coordinate is:

$$
\begin{gathered}
r=\frac{50 \sqrt{2}(4 \sin \theta-3 \cos \theta)^{1 / 2}}{(5-4 \cos \theta-3 \sin \theta)^{3 / 2}} . \\
\text { 2. } r=\frac{50 \sqrt{2}(\sin \theta+\cos \theta)^{1 / 2}}{(\sqrt{2}-\cos \theta+\sin \theta)^{3 / 2}} . \quad \text { 3. } r=\frac{50 \sqrt{2}(\cos \theta-\sin \theta)^{1 / 2}}{(\sqrt{2}-\cos \theta-\sin \theta)^{3 / 2}} .
\end{gathered}
$$

## 5. LESSON $17 \mathrm{M}$.

## 6. MISCELLANEOUS TYPES OF PROBLEMS LEADING TO EQUATIONS OF THE FIRST ORDER

A. Flow of Water Through an Orifice. When water flows from a tank through a small hole in its bottom, it has been proved that the rate of flow of water is proportional to the area of the hole and the square root of the height of the water in the tank. Hence

$$
\frac{d V}{d t}=-k a \sqrt{h}, \quad d V=-k a \sqrt{h} d t
$$

where

$V$ is the number of cubic feet of water in the tank at time $t \mathrm{sec}$, $k$ is a positive proportionality constant, $a$ is the area of the hole in square feet, $h$ is the height in feet of the water above the hole at time $t$.

The minus sign is necessary because the volume of water is decreasing. If $A$ is the cross-sectional area of the water surface at time $t$, then $d V=A d h$. Substituting this value of $d V$ in the second equation of (17.6) and taking $k=4.8$, a figure determined experimentally as valid under certain conditions, we obtain

$$
A d h=-4.8 a \sqrt{h} d t, \quad A h^{-1 / 2} d h=-4.8 \pi r^{2} d t,
$$

where

$A$ is the cross-sectional area in square feet of the water surface at time $t$ sec,

$h$ is the height in feet of the water level above the hole or orifice at time $t$,

$r$ is the radius in feet of the orifice. With the aid of (17.61), solve the following problems.

1. A tank, whose cross section measures $3 \mathrm{ft} \times 4 \mathrm{ft}$, is filled with water to a height of $9 \mathrm{ft}$. It has a hole at the bottom of radius $1 \mathrm{in}$. (a) When will the tank be empty? (b) When will the water be $5 \mathrm{ft}$ high? Hint. At $t=0$, $h=9$ and in (17.61) $r=\frac{1}{12}, A=12$.
2. A water container, whose circular cross section is $6 \mathrm{ft}$ in diameter and whose height is $8 \mathrm{ft}$, is filled with water. It has a hole at the bottom of radius 1 in. When will the tank be empty?
3. Solve problem 2 if the tank rests on supports above ground so that its $8 \mathrm{ft}$ height is now in a horizontal direction, Fig. 17.62, and the hole of radius $1 \mathrm{in}$. is in its bottom.

![](https://cdn.mathpix.com/cropped/2023_07_30_ca1c1f7b3544dd2eae87g-198.jpg?height=343&width=720&top_left_y=552&top_left_x=225)

Figure 17.62

4. A water tank, in the shape of a conical funnel, with its apex at the bottom and vertical axis, is $12 \mathrm{ft}$ across the top and $18 \mathrm{ft}$ high. It has a hole at its apex of radius $1 \mathrm{in}$. When will the tank be empty if initially it is filled with water?
5. A water tank, in the shape of a paraboloid of revolution, measures $6 \mathrm{ft}$ in diameter at the top and is $3 \mathrm{ft}$ deep. A hole at the bottom is 1 in. in diameter. When will the tank be empty if initially it is filled with water?
6. A cylindrical tank is $12 \mathrm{ft}$ in diameter and $9 \mathrm{ft}$ high. Water flows into the tank at the rate of $\pi / 10 \mathrm{ft}^{3} / \mathrm{sec}$. It has a hole of radius $\frac{1}{2} \mathrm{in}$. at the bottom. When will the tank be full if initially it is empty? Hint. Adjust the first equation in (17.6) to reflect the increase in volume due to the water intake. Remember $d V=A d h$. After simplification, you should obtain

$$
\int_{h=0}^{9} \frac{d h}{12-\sqrt{h}}=\frac{1}{4320} \int_{t=0}^{t} d t .
$$

If you have trouble integrating, see hint given in answer.

![](https://cdn.mathpix.com/cropped/2023_07_30_ca1c1f7b3544dd2eae87g-198.jpg?height=209&width=338&top_left_y=1571&top_left_x=99)

Figure 17.621 B. First Order Linear Electric Circuit. The differential equation which results when an applied electromotive force $E$, an inductor $L$, and a resistor $R$ are connected in series is, Fig. 17.621,

$$
L \frac{d i}{d t}+R i=E
$$

For an understanding of this equation and for the meaning of these terms, read Lesson 30 . The units we shall adopt are

ohms for the coefficient of resistance of the resistor, henries for the coefficient of inductance of the inductor, volts for the electromotive force (also written as emf), amperes for the current.

7. Solve the differential equation (17.63), if $E=E_{0}$ and at $l=0$, the current $i=i_{0}$. What is the limiting current as $t \rightarrow \infty$ ?
8. Solve (17.63) if $E=E_{0} \sin \omega t$ and at $t=0, i=i_{0}$.
9. An inductance of 3 henries and a resistance of $30 \mathrm{ohms}$ are connected in series with an emf of 150 volts. At $t=0, i=0$. Find the current when $t=0.01$ sec.
10. An inductance of 2 henries and a resistance of $20 \mathrm{ohms}$ are connected in series with an emf of $100 \sin 150 t$ volts. At $t=0, i=0$. Find the current when $t=0.01$ sec.
11. When an emf is disconnected from a circuit in which a current is flowing, i.e., at $t=0, E=0$, the current is called an induced current. The term $L d i / d t$ is then referred to as an induced electromotive force. At the moment an emf is disconnected from a circuit in which there is a resistance of $30 \mathrm{ohms}$ and an inductance of 6 henries, the current $i=15 \mathrm{amp}$. (a) Find the current equation as a function of time. (b) When will the current be $7.5 \mathrm{amp}$ ?

C. Steady State Flow of Heat. When the inner and outer walls of a body, as for example the inner and outer walls of a house or of a pipe, are maintained at different constant temperatures, heat will flow from the warmer wall to the colder one. When each surface parallel to a wall has attained a constant temperature, we say that the flow of heat has reached a steady state. In a steady state flow of heat, therefore, each surface parallel to a wall, because its temperature is now constant, is called an isothermal surface. Isothermal surfaces at different distances from an interior wall will, of course, have different temperatures. In many cases the temperature of an isothermal surface is a function only of its distance $x$ from an interior wall, and the rate of flow of heat in a unit time across such a surface is proportional both to the area of the surface and to $d T / d x$, where $T$ is the temperature of the isothermal surface. Hence

$$
Q=-k A \frac{d T}{d x}
$$

where

$Q$ is the rate of flow of calories* of heat in 1 sec across an isothermal surface,

$k$, the proportionality constant, is called the thermal conductivity of the material that is between the walls,

A calorie is equal to the amount of heat required to change the temperature of 1 gram of water 1 degree centigrade. $A$ is the area in square centimeters of an isothermal surface, $T$ is the temperature in centigrade degrees of the isothermal surface, $x$ is the distance in centimeters of the isothermal surface from an interior wall.

The negative sign is used to indicate that heat flows from the interior wall of higher temperature to the exterior wall of lower temperature.

With the help of (17.64), solve the following problems.

12. The inner and outer radii of a hollow spherical shell are $4 \mathrm{~cm}$ and $9 \mathrm{~cm}$ respectively. The thermal conductivity of the material between the walls is $0.75 \mathrm{cal} / \mathrm{deg}-\mathrm{cm}$-sec. The inner surface is kept at a constant temperature of $100^{\circ} \mathrm{C}$ and the outer surface at $0^{\circ} \mathrm{C}$. Find:

(a) The rate of heat loss per second flowing outward through the exterior of the shell.

(b) The temperature $T$ of a surface $5 \mathrm{~cm}$ from the center.

Hint. In (17.64) $A=4 \pi r^{2}$, where $r$, which replaces $x$ in the formula, is the radius of an isothermal surface. Initial conditions are: $r=4, r=100$; $r=9, T=0$.

Note, from answer, that $Q$ has a constant value. In this example, it is $2160 \pi \mathrm{cal} / \mathrm{sec}$. Hence, through each isothermal surface, this much heat escapes each second. If a surface is nearer the center of the sphere, so that its area is smaller than the area of a surface farther away, more heat per unit area will escape each second from the nearer surface than from the farther one. The total loss of heat through each surface is, however, the same.

13. A steam pipe of negligible thickness has an inside radius of $6 \mathrm{~cm}$. It is insulated with $3 \mathrm{~cm}$ coating of magnesia, whose thermal conductivity is 0.000175 . The interior of the pipe is maintained at $100^{\circ} \mathrm{C}$ and the outer surface at $0^{\circ} \mathrm{C}$. Find:

(a) The rate of heat loss per second flowing outward from each meter length of pipe.

(b) The temperature of an isothermal surface whose radius is $8 \mathrm{~cm}$.

Hint. In (17.64), $A=2 \pi r(100)$, where $r$, which replaces $x$ in the formula, is the radius of an isothermal surface.

D. Pressure-Atmospheric and Oceanic. We consider a column of air of cross-sectional area $A$, of height $d h$ and at a distance $h$ units from the surface of the earth, Fig. 17.65. For simplicity, we assume the earth is a plane, the air is at rest, and is unaffected by changes of latitude, longitude, and temperature. The positive direction is upwards. Let $p$ be the pressure ${ }^{*}$ on this column of air, due to the weight of all the air above it. Hence the total force $P$ on a column of air of cross-sectional area $A$ is $P=A p$. By differentiation we obtain

(a)

$$
d P=A d p
$$

*Pressure is the force acting perpendicularly on a unit area of surface. which gives the change of the total force $P$ for a change in the pressure $p$. Let $\rho$ be the weight of a unit volume of air. Therefore the weight of a column of air of height $d h$ and cross-sectional area $A$ is

$$
(A d h) \rho \text {. }
$$

The decrease in total force as one goes from the height $h$ to the height $h+d h$ must be equal to the weight of the column of air of thickness $d h$. Hence equating the negative of (a) with (b), we obtain

$$
\text { (17.66) }-A d p=\rho A d h, \quad \frac{d p}{d h}=-\rho,
$$

which states that the rate of change of air pressure with height is equal to the negative of the weight of a unit volume of air at that height. The units we shall use are: pounds per cubic foot for $\rho$, pounds

![](https://cdn.mathpix.com/cropped/2023_07_30_ca1c1f7b3544dd2eae87g-201.jpg?height=362&width=414&top_left_y=415&top_left_x=709)

Figure 17.65 per square foot for $p$, and feet for height.

If $p$ is oceanic pressure and $h$ is the depth below sea level, then (17.66) becomes

$$
\frac{d p}{d h}=\rho,
$$

where $\rho$ is the weight per cubic foot of ocean water.

With the aid of (17.66) and (17.67), solve the following problems.

14. Assume that the air is an isothermal gas. Therefore by Boyle's law, its pressure and density are related by the formula

$$
\rho=k p \text {. }
$$

(a) Determine the pressure of the air as a function of the height $h$ above the earth if at sea level the air pressure is $14.7 \mathrm{lb} / \mathrm{sq}$ in. Hint. In $(17.66)$, replace $\rho$ by $k p$.

(b) What is the value of the constant $k$ in (17.671) if $\rho=0.081 \mathrm{lb} / \mathrm{cu} \mathrm{ft}$ at sea level?

(c) What is the air pressure at $10,000 \mathrm{ft}$, at $15,000 \mathrm{ft}$, at $70,000 \mathrm{ft}$, at $50 \mathrm{mi}$ ?

(d) Show that the pressure is zero only when $h$ is infinite.

(e) Express the density of air as a function of height. Hint. In (17.66) replace $d p$ by $d \rho / k$ as obtained from (17.671).

(f) What is the weight of the air at an altitude of $1000 \mathrm{ft}$, of $5000 \mathrm{ft}$, of $10,000 \mathrm{ft}$, of $50,000 \mathrm{ft}$ ?

15. If air expands adiabatically, i.e., without gaining or losing heat, then $p=k \rho^{1.4}$.

(a) Find the pressure of the air as a function of height. Assume that at sea level $p=14.7 \mathrm{lb} / \mathrm{sq}$ in. and $\rho=0.081 \mathrm{lb} / \mathrm{cu} \mathrm{ft}$. Hint. First find the value of $k$. Then in $(17.66)$ replace $\rho$ by $(p / k)^{5 / 7}$. (b) How high is the atmosphere, i.e., at what height is $\rho=0$ ? Hint. In (17.66) replace $d p$ by $1.4 k \rho^{0.4} d p$. Solve for $\rho$. Then find $h$ when $\rho=0$.

16. Assume that the weight $\rho$ of a cubic foot of sea water, under a pressure of $p \mathrm{lb} / \mathrm{ft}^{2}$ is given by the formula

$$
\rho=k\left(1+2 \cdot 10^{-8} p\right) \mathrm{lb} / \mathrm{ft}^{3}
$$

and is $64 \mathrm{lb} / \mathrm{ft}^{3}$ at sea level.

(a) Find the value of $k$. Hint. At sea level $p=0$.

(b) Find the pressure of sea water as a function of its depth below sea level. Hint. In (17.67), replace $\rho$ by its value as given in (17.672).

(c) Find the weight per cubic foot of sea water as a function of depth. Hint. In (17.67), replace $d p$ by $10^{8} d \rho / 2 k$ as determined from (17.672).

(d) What is the pressure and density of sea water at $20,000 \mathrm{ft}$ below sea level?

17. If the earth is assumed spherical instead of planar, show that (17.66) becomes

$$
\frac{d p}{d h}=-\rho-\frac{2 p}{R+h},
$$

where $R$ is the radius of the earth. Hint. See Fig. 17.69. The volume of a thin spherical shell of air at a distance $r$ units from the center of the earth and thickness $d r$ is $4 \pi r^{2} d r$. Its weight, therefore, is $\left(4 \pi r^{2} d r\right) \rho$. The total

![](https://cdn.mathpix.com/cropped/2023_07_30_ca1c1f7b3544dd2eae87g-202.jpg?height=240&width=485&top_left_y=937&top_left_x=357)

Figure 17.69

force $P$ on this spherical surface due to the weight of the air outside it is $P=4 \pi r^{2} p$. Hence $d P=4 \pi\left(r^{2} d p+2 r p d r\right)$. Set $-d P$, which is the decrease in the total force $P$ as one goes from the distance $r$ to the distance $r+d r$, equal to the weight of the spherical shell.

E. Rope or Chain Around a Cylinder. When a rope or chain, assumed uniform, flexible, and inextensible, is wound around a rough cylindrical post, a small force applied at one end of the rope or chain can control a much larger force applied at the other end. For example, a man can hold in check a large weight by winding a rope attached to it, a sufficient number of times about a pole.

For a post, whose axis is horizontal, it has been proved that the differential equation

$$
\frac{d T}{d \theta}=\delta r(\cos \theta+\mu \sin \theta)+\mu T
$$

expresses the tension $T$ in the rope or chain at a point $P$ on it, when the rope or chain is just on the verge of slipping, see Fig. 17.71. In equation (17.7),

$T$ is the pull or tension on the rope at any point $P$ on it, in pounds, $r$ is the radius of the cylinder, in feet,

$\mu$ is the coefficient of friction between rope and post,

$\delta$ is the weight of the rope or chain in pounds per foot, $\theta$ is the radial angle of $P$.

With the help of (17.7), solve the following problems.

18. Show that the solution of (17.7) is

$$
T=\frac{\delta r}{1+\mu^{2}}\left[\left(1-\mu^{2}\right) \sin \theta-2 \mu \cos \theta\right]+c e^{\mu \theta} .
$$

19. A chain weighs $\delta \mathrm{lb} / \mathrm{ft}$. It hangs over a circular cylinder with horizontal axis and radius $r \mathrm{ft}$. One end of the chain is at $A$, Fig. 17.71. How far must the other end extend below $D$, so that the chain is on the verge of slipping. Hint. The initial conditions are: $\theta=0, T=0$, and $\theta=\pi, T=l \delta$, where $l$ is the length of the portion of the chain overhanging at $D$.
20. A chain weighs $\delta \mathrm{lb} / \mathrm{ft}$. It hangs over a circular cylinder with horizontal axis and radius $r \mathrm{ft}$. One end of the chain is at $B$, Fig. 17.71, the other end just reaches to $D$. What is the least value of $\mu$ so that the chain will not slip? Hint. The initial conditions are: $\theta=\pi / 2, T=0$, and if the chain is not to slip, then at $\theta=\pi$, $T=0$.
21. A chain weighs $1 \mathrm{lb} / \mathrm{ft}$. It hangs over $\mathrm{a}$ circular cylinder with horizontal axis and radius $\frac{1}{2} \mathrm{ft}$. One end of the chain reaches three quarters around the top to $C$, Fig. 17.71; the other end extends below $D$. The coefficient of friction between chain and cylinder is $\frac{1}{2}$. If the chain is on the verge of slipping, find the length $l$ of the overhang.
22. If the axis of the cylinder is vertical, then

![](https://cdn.mathpix.com/cropped/2023_07_30_ca1c1f7b3544dd2eae87g-203.jpg?height=358&width=395&top_left_y=897&top_left_x=715)

Figure 17.71 the rope's or chain's weight which now acts vertically so that it does not press down upon the cylinder, has little effective force. Hence, in formula (17.7), $\delta$ which is the weight per unit length of rope, may be taken to be zero. The formula then simplifies to

$$
\frac{d T}{d \theta}=\mu T .
$$

With the help of (17.73), solve the following problem. A longshoreman is holding a ship by means of a hawser wound around a vertical post. The ship is pulling on the rope with a force of 5 tons. If the coefficient of friction between rope and post is $\frac{1}{3}$, and the man exerts a force of $50 \mathrm{lb}$ to hold the ship, approximately how many turns of rope is he using? Hint. Initial condition is $\theta=0, T=50$. We want $\theta$ when $T=10,000$.

F. Motion of a Complex System. Solve problems 23-27 below, by use of Newton's law of motion $F=m a=m v(d v / d y)$, where $F$ is the algebraic sum of all the forces acting on a body of mass $m$, and $a$ is the acceleration of the center of gravity of the body.

23. A 24-ft chain weighing $\delta \mathrm{lb} / \mathrm{ft}$ hangs over a frictionless support, which is more than $24 \mathrm{ft}$ above the floor. Initially the chain is held at rest with $10 \mathrm{ft}$ overhanging on one side of the support and $14 \mathrm{ft}$ on the other side, Fig. 17.74.

![](https://cdn.mathpix.com/cropped/2023_07_30_ca1c1f7b3544dd2eae87g-204.jpg?height=453&width=510&top_left_y=373&top_left_x=345)

Figure 17.74

How long after its release and with what velocity will the chain leave the support? Hint. When $12 \mathrm{ft}$ of shain overhang on each side of the support, the chain is in equilibrium. Call this position $y=0$. Then if the distance of one end of the chain from equilibrium is $y$, the distance of the other end is $-y$. The effective force moving the chain is therefore $2 y \delta$. The mass of the chain is $24 \delta / 32$. Initial conditions are $\ell=0, v=0, y=2$.

24. In problem 23 , assume that the $14-\mathrm{ft}$ overhang just touches the floor. Compute the velocity with which the other end will leave the support.
25. A chain is $12 \mathrm{ft}$ long. Six feet of the chain are held extended on a frictionless flat table which is more than $12 \mathrm{ft}$ above the ground, the other $6 \mathrm{ft}$ hangs over the table. When and with what velocity will the end of the chain leave the table after its release? Hint. $m=12 \delta / 32, F=y \delta$, where $y$ is the distance of the overhanging part of the chain from the edge of the table and $\delta$ is its weight per foot. Initial conditions are $t=0, y=6, v=0$.
26. Assume, in problem 25 , that the table is only $4 \mathrm{ft}$ above the ground. With what velocity will the chain of problem 25 leave the table?
27. It has been proved that when a mass particle slides without friction down a fixed curved path whose equation is $y=f(x)$, its differential equation of motion, with upward direction positive, is

$$
v d v=-g d y,
$$

where $g$ is the acceleration due to gravity, $v$ is the velocity of the particle along the curve, and $y$ is the vertical position of the particle at time $t$. Therefore, $v=d s / d t$, where $s$ is the distance the particle has moved along the curved path. Show that the solution of (17.75), with initial conditions $t=0$, $v=v_{0}, y=y_{0}$, is

$$
v^{2}=\left(\frac{d s}{d l}\right)^{2}=v_{0}^{2}+2 g\left(y_{0}-y\right) .
$$

By means of the substitution in (17.76) of $d s / d t=\sqrt{1+(d y / d x)^{2}} d x / d t$, the equation of the path $y=f(x)$ and the initial condition $y_{0}=f\left(x_{0}\right)$, show that (17.76) becomes

$$
\left(\frac{d x}{d t}\right)^{2}=\frac{v_{0}^{2}+2 g\left[f\left(x_{0}\right)-f(x)\right]}{1+\left[f^{\prime}(x)\right]^{2}} .
$$

The solution of (17.77) will give $x$ as a function of $t$. With $x$ known, we can determine $y$ by means of the given equation of the path $y=f(x)$.

Use (17.77) to solve the following problem. A particle moves along a smooth wire, shaped in the form of the parabola $x^{2}=-y$. Initially it is at the origin and has a velocity of $4 \mathrm{ft} / \mathrm{sec}$. Find the position of the particle at the end of 5 sec. Hint. $f(x)=-x^{2}, f\left(x_{0}\right)=0, f^{\prime}(x)=-2 x$.

G. Variable Mass. Rocket Motion. In the straight line motion problems thus far considered, the mass of a particle or of a body remained constant throughout the motion. If, however, the mass itself is also changing with time, decreasing or increasing, then Newton's second law of motion no longer holds and must be modified. It has been proved, in the case of a body of variable mass moving in a straight line, that the differential equation governing its motion is given by

$$
m \frac{d v}{d t}=F+u \frac{d m}{d t}
$$

where

$m$ is the mass of the body at time $t$, $v$ is the velocity of the body at time $t$,

$F$ is the algebraic sum of all the forces acting on the body at time $t$, $d m$ is the mass joining or leaving the body in the time interval $d t$, $u$ is the velocity of $d m$ at the moment it joins or leaves the body, relative to an observer stationed on the body.

Note that (17.78) differs from Newton's second law of motion by the term $u d m / d t$.

If a mass $d m$ leaves the system in the time interval $d t, d m / d t$ will be a negative quantity; if it joins the system, $d m / d t$ will be a positive quantity.

With the aid of (17.78), solve the following problems.

28. A rocket, which weighs $32 \mathrm{M} \mathrm{lb}$ and contains fuel weighing $32 m_{0} \mathrm{lb}$, is propelled straight up from the surface of the earth by burning $32 k \mathrm{lb}$ of fuel per second and expelling it backwards at a constant velocity of $A \mathrm{ft} / \mathrm{sec}$ relative to an observer on the rocket. Assume that the only force acting on the rocket is that of gravity. Find the velocity of the rocket and the distance it travels as functions of time. Take positive direction upward. Hint. In (17.78), the variable mass $m$ at time $t$ is $m=M+m_{0}-k t$; therefore $d m / d t=-k$. The relative velocity $u$ of $d m$ is $-A$. The force of gravity at time $t$ is $F=$ $-\left(M+m_{0}-k t\right) g$. Answers are

$$
v=-g t-A \log \left(1-\frac{k}{M+m_{0}} t\right), 0 \leqq t<\frac{M+m_{0}}{k},
$$

$$
\begin{aligned}
y=A t-\frac{1}{2} g t^{2}+\frac{A}{k}\left(M+m_{0}-k t\right) \log \left(1-\frac{k}{M+m_{0}} t\right) \\
0 \leqq t<\frac{M+m_{0}}{k} .
\end{aligned}
$$

Nore. If the rocket moves in free space so that it is not subject to the gravitational force of the earth, then $F=0$ in (17.78) and $g=0$ in (17.79) and (17.8).

29. (a) Show that, when the rocket's fuel of problem 28 is exhausted, it has reached a theoretical height of

$$
y=\frac{A m_{0}}{k}-\frac{g}{2}\left(\frac{m_{0}}{k}\right)^{2}+\frac{A M}{k} \log \left(\frac{M}{M+m_{0}}\right) .
$$

Hint. The mass $m_{0}$ of the fuel will be exhausted in time $t=m_{0} / k$. Substitute this value in (17.8).

(b) Show that its velocity at that moment is

$$
v=-\frac{g m_{0}}{k}-A \log \left(\frac{M}{M+m_{0}}\right) \text {. }
$$

30. A rocket of mass $M$, containing fuel of mass $m_{0}$, falls to the earth from a great height. It burns an amount $k$ of its mass per second and ejects it downward with a constant velocity relative to an observer on the rocket of $A \mathrm{ft} / \mathrm{sec}$. Find the distance it falls in time $t$. Take positive downward direction. Hint. See problem 28 .
31. A rocket and its fuel have mass $m_{0}$. At the moment it starts to burn an amount $k$ of its mass per second, it is moving with a velocity $v_{0}$. The fuel is ejected backwards with just enough velocity, so that the ejected fuel is motionless in space. Find the subsequent velocity and distance equations of the rocket as functions of time. Hint. For the ejected fuel to be motionless relative to an observer on the earth, the backward velocity of the fuel must equal the forward velocity of the rocket; remember, the fuel at the instant of ejection has the same forward velocity $v$ as the rocket itself. Relative to an observer on the rocket, however, it will seem to him as if he were standing still and the ejected fuel moving away from him at the rate of $-v \mathrm{ft} / \mathrm{sec}$, where $+v \mathrm{ft} / \mathrm{sec}$ is his own velocity in the positive direction. Therefore $u$ in (17.78) is $-v$. The variable mass $m=m_{0}-k t$ and $d m / d t=-k$.
32. Solve problem 31 if the rocket were moving in free space. Hint. $F=0$ in (17.78).
33. A body moves in a straight line in free space with a velocity of $v_{0} \mathrm{ft} / \mathrm{sec}$. Initially its mass is $m_{0}$ and as it moves it adds to its mass $k$ slugs per second. Find its velocity and distance equations as functions of time. Hint. In $(17.78), F=0$ and assuming the added mass $d m$ is stationary in space, then its velocity relative to an observer on the body is $-v$, where $v$ is the velocity of the body. See problem 31 .
34. A spherical raindrop falls under the influence of gravity. Its mass increases by the addition of stationary moisture particles at a rate which is proportional to its surface area. Initially its radius $r=r_{0}$. Find its acceleration, velocity, and distance equations as functions of time. Take positive direction downward. Show that if initially $r_{0}=0$, the acceleration has the constant value $g / 4$. For hints, see answer section. First try to solve without making use of these hints.
35. A chain unwinds from coil held at rest. It falls straight down under the influence of gravity, which is the only acting force. Initially $l \mathrm{ft}$ of the chain are unwound. Find its velocity as a function of time. Take positive direction downward. For hints see answer section.
36. Solve problem 35 , if the chain must first slide along a frictionless plane inclined at an angle $\theta$ with the horizontal before dropping straight down. Assume that the coil is held at rest at a distance $l \mathrm{ft}$ from one end of the plane and that initially one end of the chain is at the end of the plane. For hints, see answer section.

## 7. H. Rotation of the Liquid in a Cylinder.

37. A vessel of water is rotated about a vertical axis with a constant angular velocity $\omega$. Show that when the water is motionless relative to the vessel, the surface of the water assumes the form of a paraboloid of revolution. Find the equation of the curve made by a vertical cross section through the axis of the cylinder. Hint. See Fig. 17.9. Two forces act on a particle of water at $P$ :

![](https://cdn.mathpix.com/cropped/2023_07_30_ca1c1f7b3544dd2eae87g-207.jpg?height=485&width=756&top_left_y=777&top_left_x=218)

Figure 17.9

one downward due to the weight $m g$ of the particle; the other $m \omega^{2} x$ due to the centrifugal force* of revolution. Since the particle of water is motionless, the resultant of these two forces must be perpendicular to the surface of the water at $P$; if it were not, then the resultant would itself have a component of force in a direction tangent to the curve and thus cause the particle of water at $P$ to move. You can therefore equate $\tan \theta=d y / d x$ with $m \omega^{2} x / m g$.

38. Assume that the rotating vessel of problem 37 is a cylinder containing a liquid whose weight per unit volume is $\rho$. If the pressure on the axis of the vessel is $p_{0}$, show that the pressure at the surface of the liquid at a distance $r$ from the axis is given by the differential equation $d p / d r=\rho \omega^{2} r / g$. Then show that its solution is $p=p_{0}+\left(\rho \omega^{2} r^{2} / 2 g\right)$. Hint. As in problem 37, use the fact that an element of volume is in equilibrium under the pressures on its surfaces and centrifugal force.

For a definition of centrifugal force see Lesson $30 \mathrm{M}, \mathrm{A}$. 39. Assume the cylinder of problem 38 contains a gas whose weight per unit volume is $\rho$ and which obeys Boyle's law $p=k \rho$, where $p$ is pressure and $k$ a constant. Show that $p=p_{0} e^{\omega^{2} r^{2} / 2 k q}$. Hint. Replace $\rho$ by $p / k$ in the formula given in problem 38 .

## 8. ANSWERS 17M.

l. (a) $36 / \pi \min$ (b) $12(3-\sqrt{5}) / \pi \min$.
2. $18 \sqrt{2} \mathrm{~min}$.
4. $30.5 \mathrm{~min}$.

3. $32 \sqrt{6} / \pi \mathrm{min}$.
4. $12 \sqrt{3}$ min.
5. Make the substitution $u=12-\sqrt{h} ; 65$ min.
6. $i=\frac{E_{0}}{R}\left(1-e^{-R u L}\right)+i_{0} e^{-R u L} ; i=E_{0} / R$.
7. $i=\frac{E_{0}}{\omega^{2} L^{2}+R^{2}}\left[R \sin \omega t-\omega L \cos \omega t+\omega L e^{-R t L}\right]+i_{0} e^{-R t / L}$.
8. $0.476 \mathrm{amp}$.
9. (a) $i=15 e^{-5 t}$.
(b) $0.14 \mathrm{sec}$.
10. $0.299 \mathrm{amp}$.
11. (a) $Q=2160 \pi \mathrm{cal} / \mathrm{sec}$.
(b) $64^{\circ} \mathrm{C}$.
12. (a) $8.6 \pi \mathrm{cal} / \mathrm{sec} \quad$ (b) $29^{\circ} \mathrm{C}$.
13. (a) $p=14.7 e^{-k h} \mathrm{lb} / \mathrm{sq}$ in. $=2117 e^{-k h} \mathrm{lb} / \mathrm{sq} \mathrm{ft}$.

(b) 0.000038 .

(c) $10.0 \mathrm{lb} / \mathrm{sq}$ in., $8.3 \mathrm{lb} / \mathrm{sq}$ in., $1.0 \mathrm{lb} / \mathrm{sq}$ in., $0.00060 \mathrm{lb} / \mathrm{sq}$ in.

(e) $\rho=0.081 e^{-0.000038 h}$.

(f) $0.078 \mathrm{lb} / \mathrm{cu} \mathrm{ft}, 0.067,0.055,0.012$.

15. (a) $p^{2 / 7}=p_{0}^{2 / 7}-\frac{2}{7} k^{-5 / 7} h$, where $p_{0}=14.7 \mathrm{lb} / \mathrm{sq}$ in. $=2116.8 \mathrm{lb} / \mathrm{sq} \mathrm{ft}$ and $k=2116.8(0.081)^{-7 / 5}$.

(b) $h=\frac{7}{2}\left(p_{0} / \rho_{0}\right)=\frac{7}{2}(2116.8 / 0.081)=91,500 \mathrm{ft}=17 \frac{1}{3} \mathrm{mi}$.

16. (a) $k=64 . \quad$ (b) $p=5 \cdot 10^{7}\left(e^{128 h / 10^{8}}-1\right) . \quad$ (c) $\rho=64 e^{128 k / 10^{8}}$. (d) $1,296,500 \mathrm{lb} / \mathrm{ft}^{2}, 65.7 \mathrm{lb} / \mathrm{ft}^{3}$.
17. $l=\frac{2 r \mu}{1+\mu^{2}}\left(1+e^{x \mu}\right)$. Note that for a fixed $\mu, l$ is a function only of $r$.
18. $2 \mu=\left(1-\mu^{2}\right) e^{\pi \mu / 2}, \mu=0.7324$.
19. $l=0.63 \mathrm{ft}$.
20. $\theta=15.9$ radians $=$ slightly more than $2 \frac{1}{2}$ turns of rope about the post.
21. $v=-\frac{4}{3} \sqrt{210} \mathrm{ft} / \mathrm{sec} ; t=\sqrt{\frac{5}{8}} \log (6+\sqrt{35}) \mathrm{sec}$.
22. $18.1 \mathrm{ft} / \mathrm{sec}$.
23. $17.0 \mathrm{ft} / \mathrm{sec} ; \sqrt{\frac{3}{8}} \log (2+\sqrt{3})$ sec.
24. $15.3 \mathrm{ft} / \mathrm{sec}$.
25. $x=20 \mathrm{ft}, y=-400 \mathrm{ft}$, or $x=-20 \mathrm{ft}, y=-400 \mathrm{ft}$.
26. $y=\frac{3}{2} g t^{2}-A t-\frac{A}{k}\left(M+m_{0}-k t\right) \log \left(1-\frac{k t}{M+m_{0}}\right)$,

$0 \leqq t<\frac{M+m_{0}}{k}$.

31. $v=\frac{g}{2 k}\left(m_{0}-k t\right)+\frac{2 k m_{0} v_{0}-g m_{0}^{2}}{2 k\left(m_{0}-k t\right)}$,

$y=\frac{g}{4 k^{2}}\left(2 m_{0} k t-k^{2} t^{2}\right)-\frac{2 k m_{0} v_{0}-g m_{0}^{2}}{2 k^{2}} \log \left(1-\frac{k}{m_{0}} t\right)$.

32. $v=m_{0} v_{0} /\left(m_{0}-k t\right), \quad y=\frac{m_{0} v_{0}}{k} \log \frac{m_{0}}{m_{0}-k t}$.
33. $v=m_{0} v_{0} /\left(m_{0}+k t\right), \quad y=\frac{m_{0} v_{0}}{k} \log \frac{m_{0}+k t}{m_{0}}$. 34. First show, see Exercise 15D, 12, that $r=r_{0}+k t / \delta$, where $r$ is the radius of the raindrop at time $t, k$ is a proportionality constant and $\delta$ is the mass per cubic foot of water. Hence $d r=k d t / \delta$. The velocity $u$ of the particle $d m$ is $-v$ where $v$ is the velocity of the raindrop at time $t$, since relative to an observer moving with the raindrop, it is as if he were stationary and the particle were moving to him, in the negative upward direction, with a velocity equal to his own velocity at the moment the moisture particle attaches itself to the raindrop. The variable mass $m$ at time $t$ is $m=4 \pi r^{3} \delta / 3$; $d m / d t=k 4 \pi r^{2}$. Answers are

$$
\begin{aligned}
\text { Acceleration } a & =\frac{g}{4}\left(1+\frac{3 r_{0}^{4}}{r^{4}}\right), \\
\text { Velocity } v & =\frac{\delta}{k} \frac{g}{4}\left(r-\frac{r_{0}^{4}}{r^{3}}\right), \\
\text { Distance } y & =\frac{g}{8}\left(\frac{\delta}{k}\right)^{2}\left(r^{2}+\frac{r_{0}^{4}}{r^{2}}-2 r_{0}^{2}\right) .
\end{aligned}
$$

Replacing $r$ by $r_{0}+k t / \delta$ in the above equations will give acceleration, velocity, and distance equations as functions of time.

35. The mass of chain at time $t$ is $(l+y) \delta$, where $\delta$ is the mass of chain per unit length and $l+y$ is the distance fallen in time $t$. Therefore $d m / d t=$ $\delta d y / d t=\delta v$. The velocity of a piece $d m$ of the chain as it leaves the spool, relative to a person moving with the chain is $-v$, where $v$ is the velocity of the chain at time $t$. Change $d v / d t$ in (17.78) to $v d v / d y$. Answer is

$$
v^{2}(l+y)^{2}=\frac{2 g}{3}\left[(l+y)^{3}-l^{3}\right] .
$$

36. See problem 35. Acting force now is $[(l \sin \theta) \delta+y \delta] g$, where $y$ is the vertical distance the chain has fallen in time $t$. Answer is

$$
v^{2}\left(l+y^{2}\right)=\frac{g}{3}\left[3 l y \sin \theta(2 l+y)+y^{2}(3 l+2 y)\right] .
$$

37. $y=\omega^{2} x^{2} / 2 g$, if the origin is taken at the lowest point of the parabola.
