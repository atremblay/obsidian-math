
 

> [!Definition] Pursuit Curve
> The path traced by a body which always moves in the direction of a fixed point or of another moving object is called a pursuit curve.
> 
> > [!important]
> > Look at [[Flying Plane Subjected to Wind/Vertical Wind > Cartesian#Flip the script]]. Chosing the direction of the movement can set you up for failure. If it becomes very ugly, consider changing something. Reversing the direction or perhaps using another substitution of [[../../Solutions/Variable Substitution in DE|Variable Substitution in DE]]


## Examples
- [[Flying Plane Subjected to Wind/Table of Content|Flying Plane Subjected to Wind]]
- [[Swimming across a river]]
- [[Walking on a turntable]]





6. A boy stands at A. By means of a string $l$ ft long, he holds a boat, which is in the water at $B$. He starts walking in a direction perpendicular to $A B$, always keeping the string taut. Find the equation of the boat's path. Ignore the height of the boy above the horizontal and assume that the string is always tangent to the path. The resulting curve is known as a tractrix. Hint. The slope of the tractrix is: $\tan \theta=d y / d x$.

```tikz
\usepackage{pgfplots}
\usetikzlibrary{calc}
\pgfplotsset{compat=1.16,width=15cm,height=10cm}
\begin{document}
\begin{tikzpicture}
    \newcommand\A{2};
    \begin{axis}[
        axis line style={line width=1.5pt},
        draw={rgb,255:red,209;green,209;blue,209}, 
        text={rgb,255:red,209;green,209;blue,209}, 
        fill={rgb,255:red,209;green,209;blue,209}, 
        axis lines*=middle,
        samples=200,
        enlargelimits,
        ticks=none,
        declare function={
            derivative(\x)=(-1/sinh(\x));
            paramx(\x)=\A*(\x-tanh(\x));
            paramy(\x)=\A/cosh(\x);
            }
        ]
        \newcommand\X{1.6};
        \pgfmathsetmacro\B{paramy(\X) - (derivative(\X) * paramx(\X))}; 
        \pgfmathsetmacro\zero{sinh(\X) * \B)}; 
        \pgfmathsetmacro\XX{paramx(\X)};
        \pgfmathsetmacro\YY{paramy(\X)}; 
        \addplot[
            domain=0:2.5,
            samples=50,
            draw={rgb,255:red,163;green,190;blue,140},
            ultra thick
        ]({paramx(x)},{paramy(x)})
        node [pos=0, font=\Large, right] {$B(0,l)$}; 
        \node at (0,0) [font=\large, above right] {$A(0,0)$}; 
        \addplot[
            domain=0.5:\zero,
            samples=50,
            thick
        ]{derivative(\X) * \x + \B};
        
        \addplot[mark=*] coordinates {(\XX,\YY)}
            node[pin={[font=\Large, pin edge={ultra thick}]60:{Rope anchor}}]{};
        
        \addplot[ultra thick] coordinates {(\XX, 0) (\XX,\YY)}
            node [pos=0.5, left, font=\Large] {$y=l \cdot sech(t)$};
        
        \addplot[
            ultra thick, 
            draw={rgb,255:red,235;green,203;blue,139},
            text={rgb,255:red,235;green,203;blue,139}
        ] coordinates {(\XX, \YY) (\zero,0)}
        node [pos=0.5, font=\Large, below] {$l$};
        \addplot[
            ultra thick, 
            draw={rgb,255:red,235;green,203;blue,139},
            text={rgb,255:red,235;green,203;blue,139}
        ] coordinates {(0, 0) (0,\A)}
        node [pos=0.4, font=\Large, left] {$l$};

        \node at ({\XX/2},0) [font=\large, below]  {$x=l(t- tanh (t))$};
        \node at ({(\zero + \XX)/2},0) [font=\Large, below]  {$\sqrt{l^2+y^2}$};

        \coordinate (A) at (axis cs:\zero,0); 
        \coordinate (B) at (axis cs:\XX,\YY); 
        \draw[thick] 
        let 
            \p1 = ($(B)-(A)$), 
            \n1 = {atan2(\y1,\x1)},
    
        in (A) ++(0:0.5cm) arc[start angle=0, end angle=\n1, radius=0.5cm] 
            node[midway, above, font=\Large] { \( \theta \) };
            
    \end{axis}
\end{tikzpicture}
\end{document}
```


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


## 5. LESSON $17 \mathrm{M}$.

## 