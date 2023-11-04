

> [!Definition] Relative Pursuit Curve
> If the origin of a coordinate system is not fixed on the ground but is attached to a pursued object, then the path described by a pursuer, moving always in the direction of the object he is pursuing, is called a relative pursuit curve. It is, in effect, the path traced by the pursuing body as seen by an observer in the pursued object.

## Example 
### Statement
A fighter plane whose speed is $V_{F}$ is chasing a bomber plane whose speed is $V_{B}$. The nose of the fighter plane is always pointed toward the bomber which is flying in a direction making an angle $\beta$ with the horizontal. Find the path traced by the fighter as plotted by an observer in the bomber.

### Solution 
Call $(0,0)$ the origin of a coordinate system fixed on the ground, and $(\overline{0}, \overline{0})$ the origin of a coordinate system which is moving with the bomber plane. The position of the fighter plane at any time $t$ is therefore given by two sets of coordinates, one $(x, y)$ with respect to the fixed ground origin $(0,0)$ and the other, $(x, y)$ with respect to the moving origin $(\overline{0},\overline{0})$.

```tikz
\usepackage{pgfplots}
\usetikzlibrary{calc}
\pgfplotsset{compat=1.16,width=15cm,height=10cm}
\begin{document} 
    \begin{tikzpicture}
        \pgfmathsetmacro\xf{1.8};
        \pgfmathsetmacro\yf{3};
        \pgfmathsetmacro\xb{5};
        \pgfmathsetmacro\yb{1};
        \pgfmathsetmacro\Vb{2};
        \pgfmathsetmacro\Bangle{50};
        \pgfmathsetmacro\Vbx{\Vb*cos(\Bangle)};
        \pgfmathsetmacro\Vby{\Vb*sin(\Bangle)};
        \pgfmathsetmacro\Vf{2.3};
        \pgfmathsetmacro\Fangle{90+atan((\xb-\xf)/(\yf-\yb))};
        \pgfmathsetmacro\Vfx{\Vf*cos(180-\Fangle)};
        \pgfmathsetmacro\Vfy{\Vf*sin(180-\Fangle)};

        \begin{axis}[
            axis line style={line width=1.5pt},
            draw={rgb,255:red,209;green,209;blue,209}, 
            text={rgb,255:red,209;green,209;blue,209}, 
            fill={rgb,255:red,209;green,209;blue,209}, 
            axis lines=middle,
            samples=200,
            enlargelimits,
            ticks=none,
            xmax=7, xmin=0,
            ymax=4, ymin=0
        ];
        \draw[->, line width=1.1pt] (rel axis cs:0.8,0.91) -- (rel axis cs:0.8,1) 
        node[left,pos=0.5, font=\Large] {$+$};
        \draw[->, line width=1.1pt] (rel axis cs:0.81,0.9) -- (rel axis cs:0.9,0.9) 
        node[below,pos=0.5, font=\Large] {$+$};
        % Bomber components
        \addplot [
            ->,
            -latex,
            line width =1.5pt
        ] coordinates {(\xb,\yb) ({\xb+\Vbx}, \yb)}
            node [midway, below, font=\large] {$V_b \cos(\beta)$};
        \addplot [
            ->,
            -latex,
            line width =1.5pt
        ] coordinates {({\xb+\Vbx},\yb) ({\xb+\Vbx}, {\yb+\Vby})}
            node [midway, right, font=\large] {$V_b \sin(\beta)$};
        \addplot [
            ->,
            -latex,
            line width =1.5pt
        ] coordinates {(\xb,\yb) ({\xb+\Vbx}, {\yb+\Vby})}
            node [midway, above left, font=\large] {$V_b$};
        \coordinate (A) at (axis cs:\xb,\yb); 
        \coordinate (B) at (axis cs:{\xb+\Vbx},{\yb+\Vby}); 
        \draw[thick] 
        let 
            \p1 = ($(B)-(A)$),
            \n1 = {atan2(\y1,\x1)},
        in (A) ++(0:1.2cm) arc[start angle=0, end angle=\n1, radius=1.2cm] 
            node[midway, right, font=\Large] { $\beta$ };
        \addplot[mark=*] coordinates {(\xb,\yb)}
        node [
            pin={
                [font=\large, 
                pin edge={thick}, 
                anchor=south, 
                pin distance=1.5cm
                ]270:{$Q(\overline{0},\overline{0}) \equiv (x_b,y_b) =$ position of bomber at a time $t$}}
            ]{};

        \addplot [
            ->,
            -latex,
            line width =1.5pt
        ] coordinates {(\xf,\yf) ({\xf+\Vfx}, {\yf-\Vfy})}
            node [midway, above right, font=\large] {$V_f$};
        \addplot [
            -,
            -latex,
            line width =1.5pt
        ] coordinates {(\xf,\yf) (\xf, {\yf-\Vfy})}
            node [
                midway, 
                font=\large,
                pin={
                [font=\large, 
                pin edge={thick}, 
                anchor=east, 
                pin distance=2cm
                ]95:{$V_f\sin(\pi-\theta)$}}
            ] {};
        \addplot [
            ->,
            -latex,
            line width =1.5pt
        ] coordinates {(\xf,{\yf-\Vfy}) ({\xf+\Vfx}, {\yf-\Vfy})}
            node [midway, below, font=\large] {$V_f\cos(\pi-\theta)$};
        \addplot[mark=*] coordinates {(\xf,\yf)}
            node [above right,font=\large]
            {$P(x,y) \equiv (\overline{x},\overline{y})=$ position of fighter at time $t$};
        \coordinate (C) at (axis cs:\xf,\yf); 
        \draw[thick] let 
            \p1 = ($(C)-(A)$),
            \n1 = {atan2(\y1,\x1)},
        in (A) ++(0:1cm) arc[start angle=0, end angle=\n1, radius=1cm] 
            node[pos=0.75, above right, font=\Large] { $\theta$ };
        \draw[thick] let 
            \p1 = ($(C)-(A)$),
            \n1 = {atan2(\y1,\x1)},
        in (A) ++(\n1:1.2cm) arc[start angle=\n1, end angle=180, radius=1.2cm] 
            node[pos=0.5, left, font=\Large] { $\pi-\theta$ };
            
        \addplot [] coordinates {(\xf,\yb) (\xb+2, \yb)};
        \addplot [
            draw={rgb,255:red,163;green,190;blue,140}, 
            text={rgb,255:red,163;green,190;blue,140}, 
        ] 
            coordinates {(\xf,\yb) (\xb, \yb)} 
            node[midway, below, font=\large]{$\overline{x}=x_b-x$};
        \addplot [
            |-|,
            draw={rgb,255:red,235;green,203;blue,139},
            text={rgb,255:red,235;green,203;blue,139}
        ] 
            coordinates {({\xf-0.3},\yf) ({\xf-0.3}, \yb)} 
            node[midway, left, font=\large]{$\overline{y}=y_b-y$};
            
        \addplot [] coordinates {(\xf,\yb) (\xf, \yf)};
        \addplot [] coordinates {(\xb,\yb) (\xf, \yf)};
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```

As measured from an observer on the ground, the effective velocities of the fighter plane in the $x$ and $y$ directions are

$$
\begin{align}
\frac{d x}{d t}&=V_{F} \cos (\pi-\theta)=-V_{F} \cos \theta, \tag{a} \\
\frac{d y}{d t}&=-V_{F} \sin (\pi-\theta)=-V_{F} \sin \theta .
\end{align}
$$

If two planes are approaching each other along a straight line, then to an observer in one plane, it will seem as if he were standing still and the other plane were coming toward him with a speed equal to the sum of the speeds of the two planes. If the two planes are moving away from each other along a straight line, then it will seem to an observer in one plane as if he were standing still and the other plane were going away from him with a speed equal to the sum of the speeds of the two planes. If, however, the planes are going in the same direction along a straight line, then to an observer in a pursued plane it will seem, if his speed is slower than the other, as if he were standing still and the other plane were coming toward him with a speed equal to the difference of the speeds of the two planes. If his speed is greater than the other, then it will seem to him as if he were standing still and the other plane were going away from him with a speed equal to the difference of the speeds of the two planes.

Here the horizontal component of fighter and bomber velocities are in the same direction. Hence, to an observer in the bomber (we assume the fighter's component is greater than the bomber's), it is as if he were standing still, and the fighter plane were coming toward him in the positive $x$ direction with a velocity equal to the difference of their two $x$ components of velocity, i.e., the rate of change of $x$ to an observer in the bomber is

$$
\frac{d \overline{x}}{d t}=-V_{F} \cos \theta-V_{B} \cos \beta \tag{b}
$$

The vertical components of fighter and bomber velocities are in opposite directions and pointed toward each other. Hence, the velocity of the fighter relative to an observer in the bomber, is in the negative $y$ direction and is the sum of their two vertical velocities, i.e., the rate of change of $\eta$ is

$$
\frac{d \overline{y}}{d t}=-\left(V_{F} \sin \theta+V_{B} \sin \beta\right) \tag{c}
$$

Dividing (c) by (b), we obtain

$$
\frac{d \overline{y}}{d \overline{x}}=\frac{V_{F} \sin \theta+V_{B} \sin \beta}{V_{F} \cos \theta+V_{B} \cos \beta}, \tag{d}
$$

which has the same form as [[Pursuit Curve#^60e0f6|this example]]  which is a [[../../Definitions/Homogeneous Function|homogeneous function]]. Remember $V_{F}, V_{B} \sin \beta$, and $V_{B} \cos \beta$ are constants. Hence it can be solved by [[../../Solutions/Variable Substitution in DE|Variable Substitution in DE]]. The initial conditions will depend on the distance and direction of the fighter plane from the bomber plane at $t=0$, i.e., at the moment when the pursuit began.

## Example 

### Statement
Solve the problem of Example 17.4 by using polar coordinates.

### Solution
To an observer in the bomber, when he moves to the right it appears to him as if he were standing still and the fighter plane were moving to the left. Hence, to an observer in the bomber, when he moves with a velocity $\mathbf{V}_{B}$, represented by the lower vector in Fig. 17.51, it appears to him as if he were standing still and the fighter is

[](https://cdn.mathpix.com/cropped/2023_07_30_ca1c1f7b3544dd2eae87g-194.jpg?height=436&width=991&top_left_y=288&top_left_x=104)

Figure 17.51

```tikz
\usepackage{pgfplots}
\pgfplotsset{compat=1.16} 

\begin{document}

\begin{tikzpicture} 
%\begin{polaraxis}
%\addplot+[domain=0:360,samples=100] {sin(x)};
%\end{polaraxis}
\end{tikzpicture}

\end{document}
```

moving with a velocity $\mathbf{V}_{B}$ represented by the upper vector in this figure. The radial and transverse components of the upper $\mathbf{V}_{B}$ are

$$
\frac{d r}{d t}=-V_{B} \cos (\theta-\beta), \quad r \frac{d \theta}{d t}=V_{B} \sin (\theta-\beta) . \tag{a}
$$

As seen from an observer in the bomber, therefore, the effective velocity of the fighter in the radial direction is (remember it is the sum of the two velocities in the same negative $r$ direction)

$$
\frac{d r}{d t}=-V_{F}-V_{B} \cos (\theta-\beta) \tag{b}
$$

and in the transverse direction is

$$
r \frac{d \theta}{d t}=V_{B} \sin (\theta-\beta) \tag{c}
$$

Dividing (b) by (c), we obtain

$$
\frac{d r}{r d \theta}=-\frac{\cos (\theta-\beta)}{\sin (\theta-\beta)}-\frac{V_{F}}{V_{B}} \csc (\theta-\beta) . \tag{d}
$$

Its solution is
$$
\log r+\log c =-\log \sin (\theta-\beta)-\frac{V_{F}}{V_{B}} \log [\csc (\theta-\beta)-\cot (\theta-\beta)], \tag{e}
$$

which simplifies to

$$
\begin{align}
c r & =\frac{[\csc (\theta-\beta)-\cot (\theta-\beta)]^{-V_{P} / V_{B}}}{\sin (\theta-\beta)}  \tag{f}\\
& =\frac{1}{\sin (\theta-\beta)}\left[\frac{1-\cos (\theta-\beta)}{\sin (\theta-\beta)}\right]^{-V_{F} / V_{B}} \\
& =\frac{1}{[\sin (\theta-\beta)]^{1-\left(V_{F} / V_{B}\right)}[1-\cos (\theta-\beta)]^{V_{F} / V_{B}}} \cdot
\end{align}
$$

> The initial conditions are $t=0, r=r_{0}, \theta=\theta_{0}$.

Comment 17.52. As viewed by an observer in the bomber, it is as if he were standing still, and the path of the fighter as given in (f) (which is the resultant of both planes' motions) were due entirely to the fighter plane's movements.

Comment 17.521. By means of (f), we can draw a rough graph of the fighter plane's path, as seen from the observer in the bomber.

##### Case 1
If $V_{B}=V_{F}$, i.e., if the ground speed of both planes are the same, then $(f)$ reduces to

$$
c r=\frac{1}{1-\cos (\theta-\beta)}, \tag{g}
$$

which is the equation of a parabola [see (34.68) and comment following].

A rough graph of the path of the fighter plane as viewed from the bomber is given for this case in Fig. 17.53. It is evident that the fighter will never reach the bomber.

[](https://cdn.mathpix.com/cropped/2023_07_30_ca1c1f7b3544dd2eae87g-195.jpg?height=668&width=949&top_left_y=1109&top_left_x=129)

##### Case 2
If $V_{F}>V_{B}$, then $1-\left(V_{F} / V_{B}\right)<0$, and (f) can be written as

$$
c r=\frac{[\sin (\theta-\beta)]^{\left(V_{R} / V_{B}\right)-1}}{[1-\cos (\theta-\beta)]^{V_{F} / V_{B}}} \tag{h}
$$

A rough graph of the fighter plane as viewed from the bomber is given for this case in Fig. 17.54 (with $V_{F}=2 V_{B}$ ).

[](https://cdn.mathpix.com/cropped/2023_07_30_ca1c1f7b3544dd2eae87g-196.jpg?height=699&width=902&top_left_y=468&top_left_x=160)

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
