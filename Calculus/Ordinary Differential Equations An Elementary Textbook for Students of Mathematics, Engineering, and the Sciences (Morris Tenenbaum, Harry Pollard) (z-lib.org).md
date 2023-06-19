substance loses $4 \mathrm{lb}$ of moisture in $1 \mathrm{hr}$, how much time is required for the substance to lose 80 percent of its moisture content? Assume the substance loses moisture at a rate that is proportional to its moisture content and to the difference between the moisture content of saturated air and the moisture content of the air.

## ANSWERS 15E

1. $8 \frac{1}{3}$ units. 2. 1.80 units.
2. $x=\frac{s_{1} s_{2}\left[e^{k\left(\varepsilon_{1}-s_{2}\right) t}-1\right]}{s_{1} e^{k\left(s_{1}-t_{2}\right) t}-s_{2}}, x=\frac{s_{1}^{2} k t}{1+s_{1} k t}$.
3. (a) $7.47 \mathrm{lb}$.
(b) $5.6 \mathrm{~min}$.
4. (a) $x=180 t /(2+9 t)$.

(b) $20 \mathrm{lb}$.

6. (a) $59.2 \mathrm{lb}$.

(b) $18.2 \mathrm{~min}$.

![](https://cdn.mathpix.com/cropped/2023_06_11_07481b882bec5cb4f96ag-152.jpg?height=43&width=843&top_left_y=657&top_left_x=75)

LESSON 16. Motion of a Particle Along a Straight LineVertical, Horizontal, Inclined.

In this lesson we discuss a wide variety of problems involving the motion of a particle along a straight line. In Lesson 34, we shall discuss the motion of a particle moving in a plane.

By Newton's first law of motion, a body at rest will remain at rest, and a body in motion will maintain its velocity, (i.e., its speed and direction), unless acted upon by an outside force. By his second law, the rate of change of the momentum of a body (momentum $=$ mass $\times$ velocity) is proportional to the resultant external force $F$ acting upon it. In mathematical symbols, the second law says

$$
F=k m \frac{d v}{d t}
$$

where $m$ is the mass of the body, $v$ its velocity, and $k>0$ is a proportionality constant whose value depends on the units used. If these are foot for distance, pound for force, slug for mass ( $=1 / 32$ pound), second for time, then $k=1$ and (a) becomes

$$
F=m \frac{d v}{d t}=m a=m \frac{d^{2} s}{d t^{2}}
$$

where $a$ is the rate of change in velocity, commonly called the acceleration of the particle, and $s$ is the distance the particle has moved from a fixed point. A force of $1 \mathrm{lb}$ therefore will give a mass of 1 slug an acceleration of $1 \mathrm{ft} / \mathrm{sec}^{2}$. Remember that $F$, a, and $v$ are vector quantities, i.e., they not only have magnitude but also direction. (For a discussion of a vector quantity, see Lesson 16C.) Hence it is always essential in a problem to indicate the positive direction. If we write

(b)

$$
\frac{d v}{d t}=\frac{d v}{d s} \frac{d s}{d t}
$$

and recognize that $v=d s / d t$, then (b) becomes

$$
\frac{d v}{d t}=v \frac{d v}{d s}
$$

Hence we can also write (16.l) as

$$
F=m v \frac{d v}{d s}
$$

Newton also gave us the law of attraction between bodies. If $m_{1}$ and $m_{2}$ are the masses of two bodies whose centers of gravity are $r$ distance apart, the force of attraction between them is given by

$$
F=k \frac{m_{1} m_{2}}{r^{2}}
$$

where $k>0$ is a proportionality constant.

LESSON 16A. Vertical Motion. Let, see Fig. 16.13,

$M=$ mass of the earth, assumed to be a sphere, $m=$ mass of a body in the earth's gravitational field, $R=$ the radius of the earth, $y=$ the distance of the body above the earth's surface.

![](https://cdn.mathpix.com/cropped/2023_06_11_07481b882bec5cb4f96ag-153.jpg?height=308&width=485&top_left_y=1156&top_left_x=357)

Figure 16.13

By (16.12) the force of attraction between earth and body is (we assume their masses are concentrated at their respective centers)

$$
F=-G \frac{M m}{(R+y)^{2}}
$$

The proportionality constant $G$ which we have used in place of $k$ is called the gravitational constant. The negative sign is necessary because the resulting force acts downward toward the earth's center, and our positive direction is upward. If the distance $y$ of the body above the earth's surface is small compared to the radius $R$ of the earth, then the error in writing (16.14) as

$$
F=-\frac{G M m}{R^{2}}
$$

is also small. $\{R=4000$ miles approximately so that even if $y$ is as high as 1 mile above the earth, the difference between using $(4000 \times 5280)^{2}$ feet and $(4001 \times 5280)^{2}$ feet in the denominator is relatively negligible.] By (16.1) with $y$ replacing $s$, we can write (16.15) as

$$
m \frac{d^{2} y}{d t^{2}}=-\frac{G M m}{R^{2}}
$$

Since $G, M$, and $R$ are constants, we may replace $G M / R^{2}$ by a new constant which we call $g$. We thus finally obtain for the differential equation of motion of a falling body in the gravitational field of the earth,

$$
m \frac{d^{2} y}{d t^{2}}=-g m, \quad m \frac{d v}{d t}=-g m
$$

where $v=d y / d t$. The minus sign is necessary because we have taken the upward direction as positive (see Fig. 16.13) and the force of the earth's attraction is downward. From (16.17), we have

$$
\frac{d^{2} y}{d t^{2}}=-g
$$

The constant $g$ is thus the acceleration of a body due to the earth's attractive force, commonly known as the force of gravity. Its value varies slightly for different locations on the earth and for different heights. For convenience we shall use the value $32 \mathrm{ft} / \mathrm{sec}^{2}$.

Integration of (16.18) gives the velocity equation

$$
v\left(=\frac{d y}{d t}\right)=-g t+c_{1}
$$

And by integration of (16.19), we obtain the distance equation

$$
y=-\frac{g t^{2}}{2}+c_{1} t+c_{2}
$$

Example 16.21. A ball is thrown upward from a building which is 64 feet above the ground, with a velocity of $48 \mathrm{ft} / \mathrm{sec}$. Find:

1. How high the ball will rise.
2. How long it will take the ball to reach the ground.
3. The velocity of the ball when it touches the ground. Solution (Fig. 16.22). By (16.2), with $g=32$,

$$
y=-16 t^{2}+c_{1} t+c_{2}
$$

Differentiation of (a) gives

$$
v=-32 t+c_{1} \text {. }
$$

If the origin is taken at ground level, the initial conditions are $t=0, y=64$,

![](https://cdn.mathpix.com/cropped/2023_06_11_07481b882bec5cb4f96ag-155.jpg?height=247&width=432&top_left_y=253&top_left_x=676)
$v=48$. Inserting these values in (a) and (b), we obtain

Figure 16.22

$$
c_{2}=64, \quad c_{1}=48
$$

Hence (a) and (b) become respectively

$$
y=-16 t^{2}+48 t+64, \quad v=-32 t+48 .
$$

The ball will continue to rise until its velocity is zero. By (d), when $v=0, t=1.5$ seconds, and when $t=1.5$ seconds, $y=100$ feet. Hence the ball will rise 100 feet above the ground.

When the ball is at ground level, $y=0$, and by (d) when $y=0$, $t=4$ seconds. Hence the ball will reach the ground in 4 seconds. Its velocity at that moment will then be, by the second equation in (d),

$$
v=(-32)(4)+48=-80 \mathrm{ft} / \mathrm{sec} \text {. }
$$

The negative sign indicates that the ball is moving in a downward direction.

Comment 16.23. In the above example, we ignored the very important factor of air resistance. In a real situation, this factor cannot be thus ignored. Air resistance varies, among other things, with air density and with the speed of the object. Furthermore, air density itself changes with height and with time. It is different for different heights and may be different from day to day. The factor of air resistance in a real problem is thus a complicated one.

When, therefore, we assume in the examples which follow, a constant atmosphere and an air resistance which is dependent only on the speed of the object, we have simplified the practical problem enormously. And when in addition we suppose that this simplified air resistance is proportional to an integral power of the speed, we have simplified the problem considerably further. There is no valid reason why air resistance may not be proportional to the logarithm of the speed or to the square root of the speed, etc.

In all cases, however, air resistance always acts in a direction to oppose the motion. Example 16.24. A body of mass $m$ slugs is dropped from a height of 5000 feet. Find the velocity and the distance it will fall in time $t$. Assume that the force of the air resistance is proportional to the first power of the velocity, the proportionality constant being $m / 40$.

Solution (Fig. 16.241). The force of the air resistance is given as $(m / 40) v$. The downward force due to the weight of the mass $m$ is $m g$ pounds. Hence the differential equation of motion (16.17) must be modified to read, with the positive direction dowmward (remember mass $\times$ acceleration of a body = the net $\downarrow+$ forces acting upon it),

$y=5,000 \quad$ Ground

Figure 16.241

(a)

$$
m \frac{d v}{d t}=g m-\frac{m}{40} v
$$

Note that the force of gravity $g m$ is now positive since it acts in the chosen positive direction. This equation can be solved by the method of Lesson $6 \mathrm{C}$ or $11 \mathrm{1B}$. Using the latter method, we write (a) as

(b)

$$
\frac{d v}{d t}+\frac{1}{40} v=g
$$

The integrating factor by $(11.12)$ is $e^{t / 40}$. The solution of (b) is therefore

$$
v=40 g+c_{1} e^{-i / 40}
$$

Integration of (c) gives

$$
y=40 g t-40 c_{1} e^{-t / 40}+c_{2}
$$

If the origin is taken at the point where the body is dropped, then the initial conditions are $t=0, v=0, y=0$. Substituting these values in (c) and (d), we find

$$
c_{1}=-40 g, \quad c_{2}=-1600 g
$$

Hence the two required equations are

$$
v=40 g\left(1-e^{-t / 40}\right), \quad y=40 g\left(t+40 e^{-t / 40}-40\right)
$$

Comment 16.25. We see from (f), that as $t \rightarrow \infty$, the velocity $v \rightarrow 40 g$. This means that when a resisting force is present, the velocity does not increase indefinitely with time but approaches a limiting value beyond which it will not increase. This limiting velocity is called the terminal velocity of the falling body. In this example, it is $40 \mathrm{ft} / \mathrm{sec}$. Example 16.26. The problem and initial conditions are the same as in Example 16.24 excepting that the force of the air resistance is assumed to be proportional to the second power of the velocity. Find the velocity of the body as a function of time and also the terminal velocity of the body.

Solution. Here the force of the air resistance is $(m / 40) v^{2}$. Hence (a) of Example 16.24 must be modified to read
(a) $m \frac{d v}{d t}=m g-\frac{m}{40} v^{2}, \quad \frac{d v}{d t}=\frac{40 g-v^{2}}{40}, \quad \frac{d v}{40 g-v^{2}}=\frac{1}{40} d t$.

Integrating the last equation in (a) and inserting the initial conditions as limits of integration, we obtain

$$
\int_{v=0}^{v} \frac{d v}{(2 \sqrt{10 g})^{2}-v^{2}}=\frac{1}{40} \int_{t=0}^{t} d t
$$

Its solution is
(c) $\frac{1}{4 \sqrt{10 g}} \log \left(\frac{2 \sqrt{10 g}+v}{2 \sqrt{10 g}-v}\right)=\frac{1}{40} t, \quad \frac{2 \sqrt{10 g}+v}{2 \sqrt{10 g}-v}=e^{(\sqrt{10 g} / 10) t}$.

Solving the last equation for $v$, we obtain

$$
v=2 \sqrt{10 g}\left(\frac{e^{(\sqrt{10 g} / 10) t}-1}{e^{(\sqrt{10 g} / 10) t}+1}\right)
$$

which gives the velocity of the body as a function of $t$.

As $t \rightarrow \infty$, we see from (d) that $v \rightarrow 2 \sqrt{10 g}$. This is the terminal velocity of the body.

Example 16.27. A raindrop falls from a motionless cloud. Find its velocity as a function of the distance it falls. Assume it is subject to a resisting force which is proportional to the second power of the velocity. Also find its terminal velocity.

Solution. Taking the downward direction as positive, the differential equation of motion (16.17) must be modified to read

$$
m \frac{d v}{d t}=m g-k v^{2}
$$

where $k>0$ is a proportionality constant. Since we wish to find $v$ as a function of the distance $y$, we replace $d v / d t$ by its equal as given in (16.11). Hence (a) becomes

$$
m v \frac{d v}{d y}=m g-k v^{2}, \quad \frac{v d v}{m g-k v^{2}}=\frac{d y}{m}
$$

If the cloud is taken as the origin, then the initial conditions are $y=0$, $v=0$. Integration of (b) and insertion of the initial conditions give

(c)

$$
\int_{v=0}^{\nabla} \frac{v d v}{m g-k v^{2}}=\frac{1}{m} \int_{y=0}^{y} d y
$$

whose solution is

(d)

$$
\begin{gathered}
-\frac{1}{2 k} \log \left(\frac{m g-k v^{2}}{m g}\right)=\frac{y}{m}, \\
m g-k v^{2}=m g e^{-2 k y / m}, \\
v^{2}=\frac{m g}{k}\left(1-e^{-2 k y / m}\right) .
\end{gathered}
$$

As $t \rightarrow \infty$, the distance $y$ the raindrop falls approaches infinity, and as $y \rightarrow \infty$, we see from (d) that $v^{2} \rightarrow m g / k$. Hence the terminal velocity is

$$
\text { T. V. }=\sqrt{m g / k} \text {. }
$$

Note. Since the body is falling and the downward direction is positive, the positive square root must be taken for the velocity in the last equation of (d).

Comment 16.28. The terminal or limiting velocity has no $y$ in it, and is therefore independent of the height from which the raindrop falls. It is also independent of the initial velocity. From actual experience we know that a raindrop reaches its limiting velocity in a finite and not in an infinite time. This is because other factors also operate to slow the raindrop's velocity.

Comment 16.29. A body falling in water encounters a resistance just as does the body falling in air. If the magnitude of the velocity is small, the resistance of the water is approximately proportional to the first power of the velocity. The differential equation of motion (16.17) therefore becomes, with the downward direction positive,

$$
m \frac{d v}{d t}=m g-k v
$$

which is similar to (a) of Example 16.24.

Example 16.3. A man with a parachute jumps at a great height from an airplane moving horizontally. After 10 seconds, he opens his parachute. Find his velocity at the end of 15 seconds and his terminal velocity (i.e., the approximate velocity with which he will float to the ground). Assume that the combined weight of man and parachute is 160 pounds, and the force of the air resistance is proportional to the first power of the velocity, equaling $\frac{1}{2} v$ when the parachute is closed and $10 v$ when it is opened. Solution. For the first 10 seconds of fall, the differential equation of motion (16.17) of the man is, with positive direction doumward,
(a)

$$
m \frac{d v}{d t}=m g-\frac{1}{2} v
$$

Here the downward force $m g$ is eq̧ual to 160 pounds and the mass $m=$ 160/32. Hence (a) becomes

$$
\frac{160}{32} \frac{d v}{d t}=160-\frac{1}{2} v, \quad \frac{d v}{d t}+\frac{1}{10} v=32
$$

Its solution, by the method of Lesson $11 B$, is

$$
v=320+c e^{-0.1 t}
$$

If we take the origin at the point of jump, then $t=0, v=0$. Hence by (c), we find $c=-320$ so that

$$
v=320\left(1-e^{-0.1 t}\right)
$$

When $t=10$

(e) $\quad v=320\left(1-e^{-1}\right)=320(0.6321)=202.3 \mathrm{ft} / \mathrm{sec}$.

Starting with the tenth second, the differential equation (16.17) becomes (remember the resistance is now $10 v$ )

$$
\frac{160}{32} \frac{d v}{d t}=160-10 v, \quad \frac{d v}{d t}+2 v=32
$$

whose solution is

$$
v=16+c e^{-2 t}
$$

Inserting in (g) the initial condition which, by (e), is $t=0, v=202.3$, we find $c=186.3$. Hence $(g)$ becomes

$$
v=16+186.3 e^{-2 t}
$$

When $t=5$, i.e., 5 seconds after the parachute opens and 15 seconds after his jump,

$$
\begin{aligned}
v=16+186.3 e^{-10} & =16+186.3(0.000045) \\
& =16+0.008=16.008 \mathrm{ft} / \mathrm{sec} .
\end{aligned}
$$

The terminal velocity is [in (h) let $t \rightarrow \infty$ ] $16 \mathrm{ft} / \mathrm{sec}$. We see from (i), therefore, that only 5 seconds after the parachute is opened, the man is already floating to earth with a practically steady velocity of $16 \mathrm{ft} / \mathrm{sec}$. Comment 16.31. In deriving formula (16.17) for a vertically falling body, we ignored the distance $y$ of the object above the earth's surface, since we assumed it to be relatively small in comparison with the radius $R$ of the earth. If, however, the dis-

![](https://cdn.mathpix.com/cropped/2023_06_11_07481b882bec5cb4f96ag-160.jpg?height=324&width=484&top_left_y=303&top_left_x=103)

Figure 16.33 tance of the object is very far above the earth's surface, then this distance cannot be thus ignored. In this case (16.14) becomes

(16.32) $F=-G \frac{M m}{r^{2}}$,

where $M$ is the mass of the earth considered as being concentrated at its center and $r$ is the distance of the body of mass $m$ from this center (Fig.

16.33). Replacing in (16.32) the value of $F$ as given in (16.1), we obtain

$$
m \frac{d v}{d t}=-G \frac{M m}{r^{2}}, \quad \frac{d v}{d t}=-\frac{G M}{r^{2}}
$$

Since $G$ and $M$ are constants, we can replace $G M$ by a new constant $k$. There results

$$
\frac{d v}{d t}=-\frac{k}{r^{2}}, \quad \frac{d^{2} r}{d t^{2}}=-\frac{k}{r^{2}}
$$

where $v=d r / d t$. From (16.35) we deduce that the acceleration of a body in the gravitational field of the earth varies inversely as the square of the distance of the body from the center of the earth.

Example 16.36. A body is shot straight up from the surface of the earth with an initial velocity $v_{0}$. Assuming no air resistance, find:

1. The velocity $v$ of the body as a function of the distance $r$ from the center of the earth.
2. Its velocity when it is 4000 miles above the earth's surface.
3. How high the body will rise.
4. The magnitude of the initial velocity $v_{0}$ in order that the body may escape the earth, i.e., in order that it may never return to the earth.
5. The time $t$ as a function of the distance $r$ of the body from the earth's center.

Solution (Fig. 16.361). We take the origin at the center of the earth, and call $R$ the radius of the earth. Then, by (16.35),

$$
a=\frac{d v}{d t}=-\frac{k}{r^{2}}
$$

Substituting in (a), the initial conditions $r=R, a=-g$, we find $k=g R^{2}$. Hence (a) becomes

$$
\frac{d v}{d t}=-\frac{g R^{2}}{r^{2}}
$$

Since we wish to find $v$ as a function of the distance $r$, we replace $d v / d t$ by its equivalent value as given in (16.11). Hence (b) becomes

$$
v \frac{d v}{d r}=-\frac{g R^{2}}{r^{2}}, \quad v d v=-\frac{g R^{2}}{r^{2}} d r
$$

Integration of (c) and insertion of the initial conditions $v=v_{0}, r=R$, give

$$
\int_{v=v_{0}}^{\nabla} v d v=-g R^{2} \int_{r=R}^{r} \frac{d r}{r^{2}}
$$

whose solution is

$$
v^{2}=v_{0}^{2}+\frac{2 g R^{2}}{r}-2 g R=v_{0}^{2}+2 g R\left(\frac{R}{r}-1\right)
$$

Hence the answer to question 1 is

$$
v= \pm \sqrt{v_{0}^{2}+2 g R\left(\frac{R}{r}-1\right)}
$$

the positive sign is to be used when the body is rising, the negative sign when it is falling. When the body is 4000 miles above the earth's surface,

![](https://cdn.mathpix.com/cropped/2023_06_11_07481b882bec5cb4f96ag-161.jpg?height=416&width=486&top_left_y=1104&top_left_x=353)

Figure 16.361

$r=8000(R=4000$ miles approximately). Inserting this value in (f), we obtain

(g) $v= \pm \sqrt{v_{0}^{2}+2 g R\left(\frac{R}{8000}-1\right)}= \pm \sqrt{v_{0}^{2}-4000 g}$,

which is the answer to question 2 . The body will continue to rise until $v=0$. Hence by (f)

$$
0=v_{0}^{2}+2 g R\left(\frac{R}{r}-1\right), r=\frac{2 g R^{2}}{2 g R-v_{0}^{2}},
$$

which is the distance the body will rise above the center of the earth if fired with an initial velocity $v_{0}$. Subtracting $R$ from this value will give the distance the body will rise above the earth's surface. This is the answer to question 3 .

The body will escape the earth, i.e., it will never return to the earth, if $r$ increases with time. This means we want $r$ to become infinite as the velocity $v$ of the body approaches zero. By (e) we see that if $v_{0}^{2}=2 g R$, then $r \rightarrow \infty$ as $v \rightarrow 0$. Hence the answer to question 4 is

$$
\begin{aligned}
v_{0} & =\sqrt{2 g R}=\sqrt{(2)(32)(4000)(5280)} \\
& =36,765 \mathrm{ft} / \mathrm{sec}=7 \mathrm{mi} / \mathrm{sec} \text {, }^{*} \text { approx., } \\
& =25,100 \mathrm{mi} / \mathrm{hr}, \text { approx., }
\end{aligned}
$$

which is the escape velocity of a body if air resistance is ignored.

The answer to question 5 is somewhat more difficult to obtain. In (e) replace $v$ by $d r / d t$ and let

$$
a=2 g R^{2}, \quad b=v_{0}^{2}-2 g R .
$$

Hence (e) becomes

(k) $\quad v=\frac{d r}{d t}= \pm \sqrt{\frac{a}{r}+b}= \pm \sqrt{\frac{a+b r}{r}}= \pm \frac{1}{r} \sqrt{a r+b r^{2}}$.

When the body is rising, the velocity is positive and we can, therefore, write (k) as

$$
d t=\frac{r d r}{\sqrt{a r+b r^{2}}}=\frac{1}{2 b} \frac{(a+2 b r) d r}{\sqrt{a r+b r^{2}}}-\frac{a}{2 b} \frac{d r}{\sqrt{a r+b r^{2}}}
$$

If we assume $b<0$, i.e., if we assume [see (i)] $v_{0}^{2}<2 g R$, so that the body cannot escape the earth, then integration of (l) gives

(m) $t=c+\frac{1}{b} \sqrt{a r+b r^{2}}-\frac{a}{2 b \sqrt{-b}} \operatorname{Arcsin}\left(\frac{-2 b r-a}{a}\right), b<0$

where $a, b$ have the values given in ( $\mathrm{j})$.

Substituting in (m) the initial conditions $t=0, r=R$, we obtain

(n) $c=-\frac{1}{b} \sqrt{a R+b R^{2}}+\frac{a}{2 b \sqrt{-b}} \operatorname{Arcsin}\left(\frac{-2 b R-a}{a}\right), \quad b<0$.

With this value of $c,(\mathrm{~m})$ defines $t$ as a function of $r$ for a rising body.

*With $g=32 \mathrm{ft} / \mathrm{sec}^{2}, v_{0}=6.96 \mathrm{mi} / \mathrm{sec}$ instead of $7 \mathrm{mi} / \mathrm{sec}$. However, $g$ is closer to $32.17 \mathrm{ft} / \mathrm{sec}^{2}$. With this value of $\theta, v_{0}=6.98 \mathrm{mi} / \mathrm{sec}$. Remark. When the body is falling, $v$ is negative. Hence for a falling body, we must, in ( $k$ ), take

$$
v=-\sqrt{\frac{a}{r}+b}
$$

in order to arrive at an equation comparable to (l) above. Or, if you wish, you may use formula (d) of Example 16.38 following. It gives the time of a falling body as a function of $r$ with initial conditions $t=0, v=0$, $r=r_{0}$.

If we assume that $b=0$, i.e., if we assume $v_{0}^{2}=2 g R$ [see (j)] so that the body will escape the earth, then (e) becomes
(p) $v=\frac{d r}{d t}=\frac{\sqrt{2 g} R}{r^{1 / 2}}, \quad r^{1 / 2} d r=\sqrt{2 g} R d t, \quad \frac{2}{3} r^{3 / 2}=\sqrt{2 g} R t+C$.

When $t=0, r=R$. Hence $c=\frac{2}{3} R^{3 / 2}$. Therefore the last equation in (p) becomes

$$
t=\frac{2}{3 R \sqrt{2 g}}\left(r^{3 / 2}-R^{3 / 2}\right)
$$

Comment 16.37. 1. Note from (16.35) that, as $r \rightarrow \infty$, the acceleration $d^{2} r / d t^{2}$ due to the gravitational force of the earth approaches zero. This means that the influence of the earth's gravitational field, although never zero, becomes insignificant.

2. From (e) we observe that when $v_{0}^{2}=2 g R$, the escape velocity of the body, the velocity equation reduces to $v^{2}=2 g R^{2} / r$. Hence as $r$ gets larger, the velocity of the body will continue to get smaller until such time as it enters the gravitational field of another heavenly body. And if $v_{0}^{2}>2 g R$ so that $v_{0}^{2}-2 g R$ equals a positive constant $k^{2}$, then the velocity equation (e) reduces to $v^{2}=k^{2}+\frac{2 g R^{2}}{r}$. Hence as $r \rightarrow \infty$,
3. From equation (q) above, which expresses time as a function of $r$ with $v_{0}^{2}=2 g R$, we see that $t$ also approaches infinity as $r \rightarrow \infty$.

Example 16.38. A body falls from interstellar space at a distance $r_{0}$ from the center of the earth. Find:

1. Its velocity $v$ as a function of the distance $r$, where $r$ is measured from the center of the earth.
2. Its velocity when it reaches the surface of the earth.
3. The time $t$ as a function of the distance $r$.

Take the earth's center as the origin and the outward direction as positive.

Solution. The differential equation of motion of the body is the same as that of (c) in the previous example. Integration of this equation and insertion of the initial conditions gives

(a)

Its solution is

$$
\int_{v=0}^{v} v d v=-g R^{2} \int_{r=r_{0}}^{r} \frac{d r}{r^{2}} .
$$

$$
v^{2}=2 g R^{2}\left(\frac{1}{r}-\frac{1}{r_{0}}\right)
$$

which is the answer to question 1.

When $r=R$, i.e., when the body is at the surface of the earth, its velocity is, by (b),

$$
v=-\sqrt{2 g R-\frac{2 g R^{2}}{r_{0}}} .
$$

The negative sign is needed because the body is moving toward the earth and the outward direction is positive. This is the answer to question 2. From (c) we see that if $r_{0}$ is very large, i.e., if the body is extremely far away from the earth, then $2 g R^{2} / r_{0}$ is very close to zero, and $|v|$ is extremely close to, but less than $\sqrt{2 g R}$. This means that a body falling from outer space can never exceed a velocity equal to $\sqrt{2 g R}$. As we saw in (i) of Example 16.36, $\sqrt{2 g R}=25,100$ miles/hour. Hence, if air resistance is ignored, a body falling from interstellar space will have a velocity at the earth's surface which differs extremely little from 25,100 miles/hour.

We leave it to you as an exercise to show that the answer to question 3 is

(d)

$$
\begin{aligned}
t & =\frac{\sqrt{r_{0}}}{R \sqrt{2 g}}\left[\sqrt{r_{0} r-r^{2}}+\frac{r_{0}}{2}\left(\frac{\pi}{2}-\operatorname{Arcsin} \frac{2 r-r_{0}}{r_{0}}\right)\right] \\
& =\frac{\sqrt{r_{0}}}{R \sqrt{2 g}}\left(\sqrt{r_{0} r-r^{2}}+\frac{r_{0}}{2} \operatorname{Arccos} \frac{2 r-r_{0}}{r_{0}}\right) .
\end{aligned}
$$

Hint. Start with (b) and follow the method we used in Example 16.36 to find the answer to question 5. You do not need to make the substitutions (j) of Example 16.36.

Comment 16.39. In solving the two previous problems, we assumed no air resistance. Since there is air resistance, the escape velocity $v_{0}$ would have to be sufficiently greater than $\sqrt{2 g R}=25,100$ miles/hour to overcome this resistance. If, however, the body emerged from the earth's atmosphere, which is rare 100 miles above its surface, , with a velocity equal to or perhaps very slightly more than $25,100 \mathrm{miles} / \mathrm{hour}$, it would escape the earth. Conversely, the formula for the velocity of a body falling from outer space will give fairly accurate results until the object reaches the earth's atmosphere or about 100 miles from its surface. Con-

"A calculation made from an analysis of one of our satellite's orbits shows that the density of atmosphere at 932 miles above the earth is one thousand million millionths the density of air at sen level. sidering the great distances involved, 100 miles is relatively insignificant, but its importance is tremendous. It complicates the whole problem of exit and reentry of satellites.

