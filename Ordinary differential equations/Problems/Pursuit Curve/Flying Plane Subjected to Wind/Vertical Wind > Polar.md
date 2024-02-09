# Flying Plane Subjected to Wind (Polar)

## Vertical Wind
^07afa4

A pilot always keeps the nose of his plane pointed toward a city T due west of his starting point. If his speed is $v$ miles per hour and a wind is blowing from the south at the rate of $w$ miles per hour, find the equation of the plane's path. Assume that it starts from a flying field which is at a distance $a$ miles from $T$.

### Solution in Polar Coordinates

The components of the wind velocity $w$ in the radial and transverse directions are respectively (for the derivation of these formulas, see [[../../../Motion of a Particule/Free motion|Free motion]]).

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
        \pgfmathsetmacro\wendx{\X};
        \pgfmathsetmacro\wendy{\Y*1.5};
        
        \pgfmathsetmacro\wrendx{\X*1.15};
        \pgfmathsetmacro\wrendy{func(\X)*1.15};

        % Calculate the aspect ratio 
        \pgfmathsetmacro{\aspectRatio}{10/20}; % height/width 


        \pgfmathsetmacro\wtstartx{\wrendx};
        \pgfmathsetmacro\wtstarty{\wrendy};
        \pgfmathsetmacro\MMM{-1/((\wrendy - \wstarty)/((\wrendx-\wstartx)*\aspectRatio))};
        \pgfmathsetmacro\AAA{(\wrendx-\wstartx)*0.3};
        \pgfmathsetmacro\wtendx{\wendx};
        \pgfmathsetmacro\wtendy{\wendy};
        
        \pgfmathsetmacro\wyendx{\X};
        \pgfmathsetmacro\wyendy{\Y + (\dendy - \vendy)};
        \pgfmathsetmacro\wx{\dendy - \vendy}; 
        \pgfmathsetmacro\wxendx{\X};
        \pgfmathsetmacro\wxendy{\Y*1.1};
        
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
            node [pos=0.3, above, font=\Large] {$r$};
        
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
 
            

        \addplot[
            ->, 
            -latex, 
            line width=1pt,
        ] 
            coordinates {(\wstartx, \wstarty) (\wrendx, \wrendy)}
            node [pos=0.7, font=\large, below right] {$\mathbf{w}\sin(\theta)$};

        \addplot[
            ->, 
            -latex, 
            line width=1pt,
        ] 
            coordinates {(\wtstartx, \wtstarty) (\wtendx, \wtendy)}
            node [pos=0.5, font=\large, right] {$\mathbf{w}\cos(\theta)$};
        
        \addplot[
            ->, 
            -latex, 
            line width=1pt,
        ] 
            coordinates {(\wstartx, \wstarty) (\wendx, \wendy)} 
            node [pos=0.5, left, font=\Large] {$\mathbf{w}$}
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
    
        \coordinate (E) at (axis cs:\wtendx,\wtendy);
        \draw[thick] 
        let 
            \p1 = (B), 
            \n1 = {atan2(\y1,\x1)},
        in (E) ++(270:1cm) arc[start angle=270, end angle=270+\n1, radius=1cm] 
            node[midway,  below, font=\Large] {$\theta$};

    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```


Hence the effective velocity of the plane in the radial direction, taking into account the wind's speed $w$ and the plane's speed $v$, is

$$
\frac{d r}{d t}=-v+w \sin \theta \tag{b}
$$

Dividing (b) by the second equation in (a), we obtain

$$
\frac{\frac{dr}{dt}}{r\frac{d\theta}{dt}}=\frac{d r}{r d \theta}=\frac{-v+w \sin \theta}{w \cos \theta}=-\frac{v}{w} \sec \theta+\tan \theta \tag{c}
$$

Let

$$
k=-\frac{v}{w} .
$$

Then (c) becomes

$$
\frac{d r}{rd\theta}=k \sec \theta+\tan \theta \tag{d}
$$

Its solution, by integration, is

$$
\begin{align}
\frac{d r}{r d \theta}&=k \sec \theta+\tan \theta \\
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
\boxed{r=a(\sec \theta+\tan \theta)^{-v / w} \sec \theta} \tag{h}
$$

or

$$
\boxed{r \cos \theta=a(\sec \theta+\tan \theta)^{-v / w}} \tag{i}
$$