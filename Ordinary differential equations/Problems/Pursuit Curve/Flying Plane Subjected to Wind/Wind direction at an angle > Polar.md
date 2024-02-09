
# Flying Plane Subjected to Wind coming at an angle (Polar)

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


### Solution in Polar Coordinates

The angle of the wind $\alpha$ has to be measured from a frame of reference. It could be either the x or the y-axis. It turns out that the math is much cleaner with the former. Trust yourself, you tried it and it became a mess. You ended up playing with $\frac \pi 2 = \alpha + \psi + \theta$ where $\psi$ was basically what you see here as $\alpha - \theta$. So we will slightly change the statement to have $\alpha$ be measured from the x-axis.


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



The derivative of the distance $r$ with respect to time is the sum of the velocity of the plane plus how much the wind is blowing the plane away from its destination $\mathbf{w}\cos(\alpha - \theta)$.

The derivative of the angle $\theta$ with respect to time is how much the wind is make the plane rotate (think how side winds makes you move laterally) $\mathbf{w}\sin(\alpha - \theta)$.

$$
\frac{dr}{dt}=-v+w \cos(\alpha-\theta),\qquad r\frac{d \theta}{d t}=w \sin(\alpha-\theta) \tag{a}
$$

Dividing the first equation in (a) by the second will give us a derivative of the distance $r$ with respect to $\theta$.
$$
\begin{align}
\frac{d r}{r d \theta}&=\frac{-v+w \cos(\alpha-\theta)}{w \sin(\alpha-\theta)}=\frac{k+\cos (\alpha-\theta)}{\sin (\alpha-\theta)}  \tag{b}\\
\int \frac{1}{r} \frac{dr}{d\theta} d\theta&=\int \frac{k+\cos (\alpha-\theta)}{\sin (\alpha-\theta)} \tag{c}\\
\int \frac{1}{r} d r&=\int \frac{k}{\sin(\alpha-\theta)} d \theta+\int \cot (\alpha-\theta) d \theta \tag{d}\\
\end{align}
$$

Using the know trigonometric identities and integrals listed [[../../../../Calculus/Integration/Trigonometry|here]] we can go from (d) to (e)

$$
\begin{align}
\ln r+C&=k \left[\ln \left(\cos\left(\frac{\alpha-\theta}{2}\right)\right)-\ln\left(\sin \left(\frac{\alpha-\theta}{2}\right)\right)\right]  - \ln\left(\sin\left(\alpha-\theta\right)\right) \tag{e}\\
\end{align}
$$

Then (e) all simplifies quite nicely by applying $\exp$ on both sides and using logarithmic rules
$$
\begin{align}
\ln r+C&= k\ln \left(\frac{\cos\left(\frac{\alpha-\theta}{2}\right)}{\sin \left(\frac{\alpha-\theta}{2}\right)}\right) - \ln\left(\sin\left(\alpha-\theta\right)\right)\\
&= \ln \frac{
    \left(\displaystyle \frac{
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
    \left(\displaystyle \frac{
        \cos\left(\frac{\alpha-\theta}{2}\right)
        }{
        \sin \left(\frac{\alpha-\theta}{2}\right)
        }
    \right)^k
    }{
    \sin\left(\alpha-\theta\right)
    }
 \tag{f}\\
\end{align}
$$

With (f) we can plug in the initial conditions to find the value of $C$. When $t=0\rightarrow r=a, \theta = 0$

$$
\begin{align}
C&=\frac{1}{a} \frac{
    \left(\displaystyle \frac{
        \cos\left(\frac{\alpha-0}{2}\right)
        }{
        \sin \left(\frac{\alpha-0}{2}\right)
        }
    \right)^k
    }{
    \sin\left(\alpha-0\right)
    } \\
&=\frac{1}{a} \frac{
    \left(\displaystyle \frac{
        \cos\left(\frac{\alpha}{2}\right)
        }{
        \sin \left(\frac{\alpha}{2}\right)
        }
    \right)^k
    }{
    \sin\left(\alpha\right)
    } \tag{g}\\
\end{align}
$$

The value of $C$ is found at (g) and we replace it in (f)
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
In this case, the angle of the wind is calculated from the x-axis. So a vertical wind is at $\displaystyle \alpha = \frac{\pi}{2}$. This is to see if we get the same answer as [[Vertical Wind > Polar]]

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
After using the [[../../../../Calculus/Integration/Trigonometry|Half Angle]] property of both $\sin$ and $\cos$, we can simplify the $\sqrt{2}$ in the numerator and denominator
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

