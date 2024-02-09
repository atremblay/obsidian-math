# Flying Plane Subjected to Wind coming at an angle (Cartesian)

A pilot always keeps the nose of his plane pointed toward a city T due west of his starting point. If his speed is $v$ miles per hour and a wind is blowing with a velocity $w$ in a direction which makes an angle $\alpha$ with the vertical, find the equation of the plane's path. Assume that it starts from a flying field which is at a distance $a$ miles from $T$.

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


### Solution in Cartesian Coordinates
^35ce11

See [[#^07afa4|this example]] for a special case of this where $\alpha=0$

We shall give two methods by which this problem may be solved. 

##### Method 1

> [!blank-container|right-small]
> 
> ![[Images/rotation of ref.gif]]


Choose the axes so that the direction of the wind becomes the $y$ axis. The initial conditions at $t=0$, therefore, become

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
            xmax={\A*1.25},
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
            node [mark=*, pos=0, above right, font=\Large] {$P_0(a\cos(\alpha), a\sin(\alpha))$};

        \node at (0,0) [font=\LARGE, below left] {$T(0,0)$};
        
        \addplot[] 
            coordinates {(0,0) (\X, {func(\X)})}
            node [
                pin={
                [pin edge={thick}, 
                font=\large,
                anchor=north, 
                pin distance=2.5cm
                ]40: {$P(x,y)$}}
            ]{};
        
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

        \draw[->, line width=1.1pt] (rel axis cs:0.8,0.81) -- (rel axis cs:0.8,0.9) 
        node[left,pos=0.5, font=\Large] {$+$};
        \draw[->, line width=1.1pt] (rel axis cs:0.81,0.8) -- (rel axis cs:0.9,0.8) 
        node[below,pos=0.5, font=\Large] {$+$};

        \coordinate (A) at (axis cs:0,0); 
        \coordinate (B) at (axis cs:\X,\Y); 
        \draw[thick] 
        let 
            \p1 = (B), 
            \n1 = {atan2(\y1,\x1)},
    
        in (A) ++(0:1.5cm) arc[start angle=0, end angle=\n1, radius=1.5cm] 
            node[pos=0.7, right, font=\Large] { \( \theta \) };

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




The differential equations of motion, however, are exactly the same as those in Example 17.1, namely

$$
\frac{d x}{d t}=-v \cos \theta, \quad \frac{d y}{d t}=-v \sin \theta+w \tag{a}
$$

Proceeding just as we did in [[Vertical Wind > Cartesian|this example]], we obtain the same thing as [[Vertical Wind > Cartesian#^5d2bb9]],

$$
\ln \left(u+\sqrt{1+u^{2}}\right)=-k \ln x+\ln C, \quad k=w / v \tag{b}
$$

But now at $t=0, x=a \cos \alpha, u=y / x=(a \sin \alpha) /(a \cos \alpha)=\tan \alpha$. Substituting these values in (c), and solving for $c$, we obtain

$$
C=(\tan \alpha+\sec \alpha)(a \cos \alpha)^{k} \tag{c}
$$

Hence (c) becomes, after replacing $u$ by its value $y / x$,

$$
\ln \left(\frac{y}{x}+\sqrt{\frac{x^{2}+y^{2}}{x^{2}}}\right)+\log x^{k}=\ln \left((\tan \alpha+\sec \alpha)(a \cos \alpha)^{k}\right) \tag{d}
$$

Simplification of (d) gives

$$
x^{k-1}\left(y+\sqrt{x^{2}+y^{2}}\right)=(\tan \alpha+\sec \alpha)(a \cos \alpha)^{k}, \quad k=w / v . \tag{e}
$$

Replacing $k$ by its value $w / v$, we obtain finally as the equation of the plane's path

$$
\boxed{x^{(w / v)-1}\left(y+\sqrt{x^{2}+y^{2}}\right)=(\tan \alpha+\sec \alpha)(a \cos \alpha)^{w / v}} . \tag{f}
$$


##### Method 2

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
where $Q(x,y)$ and $P(x,y)$ are [[../../../Definitions/Homogeneous Function|homogeneous functions]]. Remember $v, w, \cos \alpha$, and $v \sin \alpha$ are constants. Hence it can be solved by [[../../../Solutions/Variable Substitution in DE|Variable Substitution in DE]] . The initial conditions are $x=a, y=0$. $x$ is always positive, so no need to worry about absolute values. 

Using [[../../../Solutions/Variable Substitution in DE#1. Substitution $u(t) = frac{x(t)}{t}$|this substitution]] $\displaystyle u=\frac{y}{x}, y^{\prime}=u^{\prime} x+u$ in (c)
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

Second one is much easier using [[../../../../Calculus/Integration/Substitution rule|Substitution rule]]. Let’s declare $z=\cos \alpha-u \sin \alpha$ and $dz=-\sin \alpha du$

$$
\begin{align}
\textcolor{violet}{\int_0^u \frac{- \sin \alpha}{(\cos \alpha-u \sin \alpha)}du} &= \int_0^u \frac 1 z dz \\
&=\ln{z}+C \\
&= \ln\left(\cos \alpha-u \sin \alpha\right) \Big \vert_0^u\\
&=\ln\left(\cos \alpha-u \sin \alpha\right) - \ln\left(\cos \alpha\right)\\
&=\textcolor{violet}{\ln\left(\frac{\cos \alpha-u \sin \alpha}{\cos \alpha}\right)} \tag{f}
\end{align}
$$

Using [[../../../../Trigonometry/Hyperbolic Identity#^5d2c7c|this identity]]  for $\operatorname{arctanh}$ in (e) for the first part

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
> ![[Images/plane_with_wind_at_angle.svg]]
> > Four different wind angles $\alpha$. From top to bottom, 0, 30, 45 and 60 degrees from the vertical. $v=500$, $w=100$


Put (g), (h) and (f) together
$$
\begin{align}
-\ln\left(\frac x a\right) &= \frac 1 k \left(\textcolor{lime}{\frac{1}{2} \ln \left(\frac{\sqrt{u^2+1}+\sin \alpha+u \cos \alpha}{\sqrt{u^2+1}-\sin \alpha-u \cos \alpha}\right)} - \textcolor{cyan}{\frac{1}{2} \ln \left(\frac{1+\sin \alpha}{1-\sin \alpha}\right)} \right) + \textcolor{violet}{ \ln\left(\frac{\cos \alpha-u \sin \alpha}{\cos \alpha}\right) }\\
-\ln\left(\frac x a\right) -   \ln\left(\frac{\cos \alpha-u \sin \alpha}{\cos \alpha}\right)  &= \frac 1 {2k}  \ln \left(\frac{(1-\sin \alpha)\left(\sqrt{u^2+1}+\sin \alpha+u \cos \alpha\right)}{(1+\sin \alpha)\left(\sqrt{u^2+1}-\sin \alpha-u \cos \alpha\right)}\right) 
\end{align}
$$

After applying the log rules, we end up with (where $\displaystyle u=\frac y x$)
$$
\boxed{\left(\frac {x\left(\cos \alpha-u \sin \alpha\right)} {a\cos \alpha}\right)^{-2k}= \frac{(1-\sin \alpha)\left(\sqrt{u^2+1}+\sin \alpha+u \cos \alpha\right)}{(1+\sin \alpha)\left(\sqrt{u^2+1}-\sin \alpha-u \cos \alpha\right)}}
$$



This is an implicit equation. I can’t simplify more than that. So let’s just plot a few values of $\alpha$. 

It simplifies quite a bit if $\alpha=0$. Ends up just like [[Vertical Wind > Cartesian#^5d2bb9]]
$$
\begin{align}
\left(\frac {x} {a}\right)^{-2k}&= \frac{\sqrt{u^2+1}+u }{\sqrt{u^2+1}-u} \\
&= \frac{\sqrt{u^2+1}+u }{\sqrt{u^2+1}-u} \frac{\sqrt{u^2+1}+u }{\sqrt{u^2+1}+u} \\
&= \frac{\left(\sqrt{u^2+1}+u\right)^2}{u^2+1-u^2}  \\
\left(\frac {x} {a}\right)^{-2k}&= \left(\sqrt{u^2+1}+u\right)^2 \\
\left(\frac {x} {a}\right)^{-k}&= \sqrt{u^2+1}+u \\
\end{align}
$$


