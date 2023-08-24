
Analysing the motion of a body or a particule will use [[../../Newtons's Laws/Second Law#Second law|Newton's second law]] 

$$F=ma=m\frac{\mathrm{d}^2x(t)}{\mathrm{d}t^2} \tag{1.1}$$
and [[../../Newtons's Laws/Second Law#Law of attraction|the law of attraction]]
$$F=k\frac{m_1m_2}{r^2} \tag{1.12}$$

## Vertical Motion

$M$ = mass of the earth, assumed to be a sphere
$m$ = mass of a body in the earth's gravitational field
$R$ = the radius of the earth
$x$ = the distance of the body above the earth's surface


> [!blank-container|right-medium]
> ![[../Images/earth_gravity.svg|400]]

By (1.12), the force of attraction between earth and body is (we assume their masses are concentrated at their respective centers) 
$$F=-G\frac{Mm}{(R+x)^2} \tag{1.13}$$
The proportionality constant $G$ which we have used in place of $k$ is called the **gravitational constant**. The negative sign is necessary because the resulting force acts downward toward the earth's center, and our positive direction is upward. If $x \ll R$ then
$$F\approx-G\frac{Mm}{R^2} \tag{1.14}$$

> [!NOTE]- The error in writing (1.14) is small 
> $R$ =4000 miles approximately so that even if $x$ is as high as 1 mile above the earth, the difference between using $(4000 \times 5280)^2$ feet and $(4001 \times 5280)^2$  feet in the denominator is relatively negligible.
> Because of this we typically drop the approximation $\approx$ and use a simple $=$

By combining (1.1) and (1.12) we can write 
$$m\frac{d^2x}{dt^2} = F = -G\frac{Mm}{R^2} \tag{1.15}$$
Since $G$, $M$ and $R$ are constants, we may replace $GM/R^2$ by a new constant which we call $g$. We thus finally obtain for the differential equation of motion of a falling body in the gravitational field of the earth,
$$\cancel{m}\frac{d^2x}{dt^2} = -g\cancel{m}, \hspace{1em} \cancel{m}\frac{dv}{dt}=-g\cancel{m},\tag{1.16}$$



where $v=\frac{dx}{dt}$. The minus sign is necessary because we have taken the upward direction as positive and the force of the earth's attraction is downward. From (1.16), we have
$$\frac{d^2x}{dt^2} = -g\tag{1.17}$$
The constant $g$ is thus the acceleration of a body due to the earth's attractive force, commonly known as the **force of gravity**. Its value varies slightly for different locations on the earth and for different heights. For convenience we shall use the value $32 ft/sec^2$.
Integration of (1.17) gives the velocity equation
$$v=\frac{dx}{dt}=-gt+c_1\tag{1.18}$$
and by integration of (1.18), we obtain the distance equation 
$$x(t)=-\frac{gt^2}{2} + c_1t + c_2 \tag{1.2}$$

> [!question]+ What is $G$? How was it found?
> 
> The gravitational constant, denoted by G, is a fundamental constant in physics that quantifies the strength of the gravitational force between two objects. It was first measured by the physicist Henry Cavendish in 1798 using a torsion balance experiment.
> 
> Cavendish's experiment involved two small lead spheres suspended from a thin wire. Two larger lead spheres were placed near the smaller spheres, and the gravitational attraction between them caused a small twisting of the wire. By measuring this twisting, Cavendish was able to determine the gravitational constant.
> 
> The value of G is approximately $6.67430 × 10^{-11} N(m/kg)^2$. It is a very small constant, indicating that the gravitational force is relatively weak compared to other fundamental forces in nature.


##### Example
#example #seperation_of_variable #integrating_factor #gravity
> [!Example] Statement
> A body of mass $m$ slugs is dropped from a height of 5000 feet. Find the velocity and the distance it will fall in time $t$. Assume that the force of the air resistance is proportional to the first power of the velocity, the proportionality constant being $m/40$.


> [!blank-container|right-small]
> ![[../Images/gravity_ex1.svg|300]]

The force of the air resistance is given as $(m/40)v$. The downward force due to the weight of the mass $m$ is $mg$ pounds. Hence the differential equation of motion (1.16) must be modified to read, with the *positive direction downward* (remember mass X acceleration of a body = the net forces acting upon it),
$$\cancel{m}\frac{dv}{dt}=g\cancel{m}-\frac{\cancel{m}}{40}v$$
Note that the force of gravity $gm$ is now positive since it acts in the chosen positive direction. 

> [!solution]- Alternative step - [[../Solutions/Separation of variables|Separation of variables]]
> With $g(v) = g - \frac{v}{40}$ and $f(t) = 1$
> > Do not confuse the $g$s. $g(v)$ is simply to reuse the notation from the Separation of variable page and $g$ is the constant determined above.
> $$
> \begin{align}
> \cancel{m}\frac{dv}{dt}=g\cancel{m}-\frac{\cancel{m}}{40}v \\
> \frac{1}{g-\frac{v}{40}}\frac{dv}{dt}=1 \\
> \int \frac{1}{g-\frac{v}{40}}\frac{dv}{dt} dt= \int dt \\
> \int \frac{1}{g-\frac{v}{40}} dv= \int dt \\
> -40\ln \left(g-\frac{v}{40}\right)= t + C_1 \\
> \ln \left(g-\frac{v}{40}\right)= -\frac{t}{40} + C_2 \\
> g-\frac{v}{40}= e^{-\frac{t}{40} + C_2} \\
> \frac{v}{40}= g - C_3e^{-\frac{t}{40}}\\
> v= 40g - Ce^{-\frac{t}{40}}\\
> \end{align}
> $$
> At $t=0$ the velocity is null
> $$
> \begin{align}
> v(0) = 0 &= 40g - Ce^{-\frac{0}{40}} \\
> &= 40g - C \\
> \iff C &= 40g
> \end{align}
> $$
> Giving $$v(t) = 40g(1 - e^{-\frac{t}{40}})$$

> [!Solution]- Alternative step - [[../Solutions/Integrating factor|Integrating factor]]
> $$v^\prime + \frac{v}{40} = g$$
> 
> With $p(t) = \frac{1}{40}$ and $f(t) = g$
> $$
> \begin{align}
> r(t) &= e^{\int p(t) dt} \\
>  &= e^{\int 1/40 dt} \\
>  &= e^{t/40} \\ 
> \end{align}
> $$
> Using this constant of integration
>  $$
>  \begin{align}
>  v(t) &= \frac{1}{r(t)} \int r(t)f(t)dt \\
>  &= \frac{1}{e^{t/40}} \int ge^{t/40}dt \\
>  &= \frac{40g}{e^{t/40}}(e^{t/40} + C)  \\
>  &=  40g\left(1 + \frac{C}{e^{t/40}}\right)  \\
>  \end{align}
> $$
> At $t=0$, $v(0)=0$
> $$
> \begin{align}
> v(0) = 0 &=  40g(1 + \frac{C}{e^{0/40}}) \\
> &=  40g(1 + C) \\
> \iff C = -1
> \end{align}
> $$
> Giving
> $$
> v(t) = 40g(1 - e^{-t/40})
> $$

Given that the formula for speed with respect to time is 
$$v(t) = 40g(1 - e^{-\frac{t}{40}})$$
We just need to integrate it with respect to time to get the position
$$
\begin{align}
\int v(t) dt &= \int 40g(1 - e^{-\frac{t}{40}}) dt \\
\int \frac{\mathrm{d}s}{\mathrm{d}t} dt &= \int 40g \mathrm{d}t - \int 40g e^{-\frac{t}{40}} dt \\
s &= 40g(t + C_1) + 40^2g( e^{-\frac{t}{40}} +C_2) \\
&= 40gt + 40^2ge^{-\frac{t}{40}} + C \\
&= 40g(t + 40e^{-\frac{t}{40}}) + C \\
\end{align}
$$

Since the positive direction is downward, we will chose the origin to be where the ball is dropped. So at $t=0$ the initial position $s(0)=0$  
$$
\begin{align}
s(0) = 0 &= 40g(0 + 40e^{-\frac{0}{40}}) + C \\
&= 1600g + C \\
\iff C &= - 1600g 
\end{align}
$$
Formula for the position is 
$$
\begin{align}
s(t) &= 40g(t + 40e^{-\frac{t}{40}}) - 1600g\\
&=40g(t + 40e^{-\frac{t}{40}} - 40) 
\end{align}$$
The body will reach the ground when $s(t) = 0$
$$
\begin{align}
s(t) = 0 &=40g(t + 40e^{-\frac{t}{40}} - 40) + 5000 \\
\frac{-5000}{40g} + 40 &=t + 40e^{-\frac{t}{40}}
\end{align}
$$

The ball cannot go lower than the ground. No idea how to solve this one above, but the dotted line is the maximum "height" at some time $t$.
```tikz
\usepackage{pgfplots}
\pgfplotsset{compat=1.16,width=10cm}
\begin{document} 
    \begin{tikzpicture}
        \begin{axis}[
            title={Position $s(t)$},
            axis lines=middle,
            xlabel=$t$,
            ylabel=$s(t)$,
            samples=200,
            enlargelimits,
            domain=0:20,
        ]
        \newcommand\G{32};
        \addplot[
            ultra thick,
            black
        ] {40*\G*(x + 40*exp(-x/40)-40)};
        \addplot[
            mark=none,
            black,
            dashed,
        ] plot coordinates { (0,5000) (20, 5000) };
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```

```tikz
\usepackage{pgfplots}
\pgfplotsset{compat=1.16,width=10cm}
\begin{document} 
    \begin{tikzpicture}
        \begin{axis}[
            title={Velocity $v(t)$},
            axis lines=middle,
            xlabel=$t$,
            ylabel=$v(t)$,
            samples=200,
            enlargelimits,
            domain=0:20,
        ]
        \newcommand\G{32};
        \addplot[
            ultra thick,
            black
        ] {40*\G*(1 - exp(-x/40))};
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```

##### Example
#example #gravity #seperation_of_variable 
> [!Example] Statement
> A body of mass $m$ slugs is dropped from a height of 5000 feet. Find the velocity and the distance it will fall in time $t$. Assume that the force of the air resistance is proportional to the **second** power of the velocity, the proportionality constant being $m/40$.

The force of the air resistance is given as $(m/40)v^2$

$$
\begin{align}
\cancel{m}\frac{dv}{dt}&=g\cancel{m}-\frac{\cancel{m}}{40}v^2 \\
\frac{dv}{dt}&=\frac{40g - v^2}{40} \\
\end{align}
$$

> [!solution] Alternative step - [[../Solutions/Separation of variables|Separation of variables]]
> With $g(v) = g - \frac{v^2}{40}$ and $f(t) = 1$
> > Do not confuse the $g$s. $g(v)$ is simply to reuse the notation from the Separation of variable page and $g$ is the constant determined above.
> $$
> \begin{align}
> \frac{dv}{dt}&=\frac{40g - v^2}{40} \\
> \frac{1}{40g-v^2}\frac{dv}{dt}&=\frac{1}{40} \\
> \int \frac{1}{40g-v^2}\frac{dv}{dt} dt&=\int \frac{1}{40} dt\\
> \int \frac{1}{40g-v^2} dv&=\frac{1}{40}  \int dt\\
> \end{align}
> $$
> At $t=0$ the velocity is null
> $$
> \begin{align}
> v(0) = 0 &= 40g - Ce^{-\frac{0}{40}} \\
> &= 40g - C \\
> \iff C &= 40g
> \end{align}
> $$
> Giving $$v(t) = 40g(1 - e^{-\frac{t}{40}})$$




##### Example
#example 
> [!Example] Statement
> A ball is thrown vertically upward from the ground with an initial velocity of $80 \mathrm{ft} / \mathrm{sec}$.[^1]

[^1]: No air resistance and that the object is near the earth's surface.


> [!Example] (a)
> Find its velocity and distance equations as functions of time. Take the origin at the point where the ball is thrown and the upward direction as positive.



> [!Example] (b)
> What are its velocity and height at the end of 1 sec?



> [!Example] (c)
> How long and how high will it rise?




> [!Example] (d)
>  When will it reach the ground and with what velocity?



> [!Example] (e)
> Would the results differ if the ball were a projectile weighing 5 tons?

---
##### Example
#example 
> [!Example] Statement
> A man leans over the side of a bridge and drops a stone. His stop watch shows that the stone touched the water in 2.1 seconds. How high is the bridge above the water?[^1]



---
##### Example
#example 
> [!Example] Statement
> A ball is given a downward velocity of $8^{\prime} \mathrm{ft} / \mathrm{sec}$ from a height of $120 \mathrm{ft}$ above the ground.[^1]


> [!Example] (a)
> When will it reach the ground and with what velocity will it strike the ground?



> [!Example] (b)
> How much lower must one stand in order to drop a ball and have it reach the ground at the same time as the first ball?



> [!Example] (c)
> With what velocity will the second ball strike the ground?




> [!Example] (d)
>   What is the significance of the negative sign of -3 seconds obtained in (a)? Hint. Substitute $t=-3$ in your velocity equation. Then solve this problem: If a ball is thrown upward from the ground with a velocity of $88 \mathrm{ft} / \mathrm{sec}$, how high will it go; at what height will its velocity be $8 \mathrm{ft} / \mathrm{sec}$ downward; when will it reach this height and velocity?

---

##### Example
#example 
> [!Example] Statement
> A person, $81 \mathrm{ft}$ above the ground, drops an object. With what velocity must a second person $180 \mathrm{ft}$ above the ground throw an object straight down in order that both objects reach the ground at the same time?[^1]






1. Verify the accuracy of the answer as given in the text to question 3 of Example 16.38.


In the problems below, weight in pounds is equal to mass in slugs times the acceleration of gravity in feet per second per second, i.e.,

$(16.391)$

$$
W=m g, \quad m=W / g
$$

##### Example
#example 
> [!Example] Statement
> A man weighs $160 \mathrm{lb}$ on earth.



> [!Example] (a)
> What is his mass?

> [!Example] (b)
> The acceleration of gravity on the surface of the moon is approximately one-sixth that of the earth. What will he weigh on the surface of the moon?

> [!Example] (c)
> Find formulas comparable to the velocity and distance equations (16.19) and (16.2) for the surface of the moon.

##### Example
#example 
> [!Example] Statement
> A ball thrown vertically upward from the surface of the earth with a velocity of $64 \mathrm{ft} / \mathrm{sec}$ will reach a maximum height of $64 \mathrm{ft}$ in $2 \mathrm{sec}$ (verify it). If thrown with the same velocity on the surface of the moon, find, by means of the formulas developed in 6(c), comparable figures for the maximum height reached and the time needed to attain this height.


##### Example
#example 
> [!Example] Statement
> If a man can high-jump $5 \mathrm{ft}$ on earth, how high will he jump on the moon and how much longer will he be in the air as compared with the time in the air on the earth? Assume his center of gravity is $2 \mathrm{ft}$ from the top of the bar. Hint. See problem 7.

##### Example
#example 
> [!Example] Statement
> A man whose weight is $160 \mathrm{lb}$ is in an elevater which is descending with an acceleration of $2 \mathrm{ft} / \mathrm{sec}^{2}$. What is his weight while riding in the elevator? Hint. Use (16.391); remember his mass is constant, and when the elevator accelerates down, he accelerates up.


In problems $10-17$ and 20,21 , 

##### Example
#example 
> [!Example] Statement
> A body of mass $m$ is dropped from a great height. [^2]

[^2]: Assume that the force $\boldsymbol{R}$ of the air resistance is proportional to the first power of the velocity, i.e., $\boldsymbol{R}=\boldsymbol{k v}$, and that the falling or rising body is near the earth's surface.



> [!Example] (a)
> Find its velocity and the distance it falls as functions of time. Take the positive direction as downward and the origin at the point where the body is dropped. Hint. In (a) of Example 16.24 replace $m / 40$ by $k$. Use your results to check the accuracy of the answers given in ( $f$ ).



> [!Example] (b)
> What is its terminal velocity?

##### Example
#example 
> [!Example] Statement
> Solve problem 10 if the body initially is given a downward velocity of $v_{0} \mathrm{ft} / \mathrm{sec}$. What is its terminal velocity? Compare with $10(\mathrm{~b})$ above. Note that the initial velocity does not affect the terminal velocity. [^2]

##### Example
#example 
> [!Example] Statement
> A body weighing $192 \mathrm{lb}$ is dropped from a great height. The proportionality constant $k$ of the air resistance is 12 .[^2]



> [!Example] (a)
> Find its velocity and the distance it falls as a function of time. Solve independently. Use results obtained in 10 only as a check.



> [!Example] (b)
> What is its terminal velocity?



> [!Example] (c)
> How far does it fall in $10 \mathrm{sec}$ ? What is its velocity at that moment?


##### Example
#example 
> [!Example] Statement
> Solve previous problem if the body is given an initial downward velocity of $170 \mathrm{ft} / \mathrm{sec}$ instead of being dropped. Solve independently. Use the results obtained in 11 only as a check. [^2]

##### Example
#example 
> [!Example] Statement
> When a paratrooper falls freely from a great height before opening his chute, his terminal velocity is approximately $175 \mathrm{ft} / \mathrm{sec}$. Assume a paratrooper and his chute together weigh $200 \mathrm{lb}$. [^2]

> [!Example] (a)
> Find the proportionality factor $k$ of the air resistance. Hint. Use the formula for T.V. found in 10 (b).

> [!Example] (b)
>Find his velocity and the distance he falls as a function of time.

> [!Example] (c)
> What is his velocity at the end of $8 \frac{3}{4} \mathrm{sec}, 17 \frac{1}{2} \mathrm{sec}, 26 \frac{1}{4} \mathrm{sec}, 32 \frac{5}{6} \mathrm{sec}$ ?

> [!Example] (d)
> How far has he fallen in $32 \frac{5}{6}$ sec?

##### Example
#example 
> [!Example] Statement
> Assume the paratrooper of problem 14 opens his chute when he has reached his terminal velocity of $175 \mathrm{ft} / \mathrm{sec}$, and that his chute is designed to give him a safe landing speed of $16 \mathrm{ft} / \mathrm{sec}$. [^2]

> [!Example] (a)
> What is the new value of $k$ ? Hint. Use the formula for T.V. found in problem 11.

> [!Example] (b)
> Find his velocity and the distance he falls after he opens his chute as functions of time. 


> [!Example] (c)
>What is his velocity at the end of $1 \mathrm{sec}, 2$ sec, $3 \mathrm{sec}, 4 \mathrm{sec}, 5 \mathrm{sec}$ ?

> [!Example] (d)
> How far has he fallen in $5 \mathrm{sec}$ ?

> [!Example] (e)
> Do your answers in (c) and (d) suggest a safe height at which he can open his chute?

> [!Example] (f)
> If he opens his chute at a height of $1040 \mathrm{ft}$, in how many seconds does he reach the ground?

##### Example
#example 
> [!Example] Statement
> A paratrooper jumps from a plane flying horizontally at a great height. When he feels that he has reached a steady velocity (i.e., when he has reached his terminal velocity), he opens his chute. Assume this steady velocity is $180 \mathrm{ft} / \mathrm{sec}$.[^2]

> [!Example] (a)
> Find his velocity and the distance he falls as a function of time before the chute opens. Hint. Use formula for T.V. found in $10(\mathrm{~b})$ and solve for $m / k$.

> [!Example] (b)
> Calculate his velocity at the end of $11 \frac{1}{4} \mathrm{sec}, 22 \frac{1}{2} \mathrm{sec}, 33 \frac{3}{4} \mathrm{sec}, 45 \mathrm{sec}$.

> [!Example] (c)
> How far has he fallen in 45 sec?

##### Example
#example 
> [!Example] Statement
> If the force of the air resistance is $50 \mathrm{lb}$ when a body is falling at a velocity of $25 \mathrm{ft} / \mathrm{sec}$, what is the value of the proportionality constant $k$ of the air resistance? For this $k$, find the terminal velocity of a falling body weighing $100 \mathrm{lb}$. If the body has an initial velocity of $20 \mathrm{ft} / \mathrm{sec}$, find its velocity and distance equations as functions of time.[^2]


##### Example
#example 
> [!Example] Statement
> A body weighing $96 \mathrm{lb}$ begins to sink as soon as it is placed in water. Two forces act on it to oppose its motion, an upward force due to the buoyancy of the object and a force due to the resistance of the water. Assume the buoyant force is $12 \mathrm{lb}$ and the resistance of the water is $6 v$. Take the origin on the surface of the water and downward direction as positive.


> [!Example] (a)
> Find the velocity and position of the body as functions of time.

> [!Example] (b)
> Find its terminal velocity.

##### Example
#example 
> [!Example] Statement
> The specific gravity of a body is defined as the ratio of its weight to the weight of an equal volume of water. Assume a body is released from the surface of a medium whose specific gravity is one-fourth that of the body and that the resistance offered by the medium is $m v / 3$.

> [!Example] (a)
> Find the velocity of the body as a function of time. Hint. The specific gravity of the medium equal to $\frac{1}{4}$ that of the body, implies that the medium's upward buoyant force is one-fourth the weight of the body.

> [!Example] (b)
> Find its terminal velocity.

##### Example
#example 
> [!Example] Statement
> A body of mass $m$ is shot straight up from the ground with an initial velocity of $v_{0} \mathrm{ft} / \mathrm{sec}$.[^2]

> [!Example] (a)
> Find the velocity and position of the body as functions of time. Take the positive direction upwards and the origin on the ground.

> [!Example] (b)
> How high will the body rise and when will it reach this maximum height?

##### Example
#example 
> [!Example] Statement
> An object weighing $64 \mathrm{lb}$ is shot straight up from the ground with an initial velocity of $96 \mathrm{ft} / \mathrm{sec}$. Assume the force of the air resistance is $4 v$.[^2]

> [!Example] (a)
> Find the velocity and position of the object as functions of time.

> [!Example] (b)
> How high will the body rise and when will it reach this maximum height? Check the accuracy of your answers in (a) and (b) with the formulas obtained in problem 20.

> [!Example] (c)
> When and with what velocity will it strike the ground? Note that the down trip takes longer than the up trip, whereas when there is no air resistance both times are the same. Note also that the return velocity is smaller than the initial velocity.

> [!Example] (d)
> Do you believe you would get the same answer for the velocity of the falling body when it strikes the ground and for the time of the down trip if you used the formulas for a falling body as found in problem 10, with $y$ having the value determined in (b)? Try it.



##### Example
#example 
> [!Example] Statement
>  In Example 16.27, we found the velocity $v$ of a falling body as a function of the distance fallen. Starting with equation (a) of this example, find the velocity and distance of a falling body as functions of time. Find its terminal velocity. Compare with (e) of Example 16.27.[^3]

[^3]:assume that the force $\boldsymbol{R}$ of air resistance is proportional to the second power of the velocity, i.e., $R=k v^{2}$, and that the falling or rising body is near the earth's surface.

##### Example
#example 
> [!Example] Statement
>  A body of mass $m$ falls from a great height. Its initial velocity is $v_{0} \mathrm{ft} / \mathrm{sec}$. Find:

> [!Example] (a)
> Its velocity as a function of the distance fallen. Take the positive direction downwards and the origin at the point of fall.

> [!Example] (b)
> Its velocity as a function of time.

> [!Example] (c)
> Its terminal velocity. Compare with (e) of Example 16.27 and with problem 22. Note that the initial velocity does not affect the terminal velocity.

##### Example
#example 
> [!Example] Statement
>  A body of mass $m$ is fired vertically upward from the ground at an initial velocity of $v_{0} \mathrm{ft} / \mathrm{sec}$.

> [!Example] (a)
> Find its velocity as a function of its height. Take positive direction upwards and origin on ground.

> [!Example] (b)
> Find its velocity as a function of time.

> [!Example] (c)
> Find its height as a function of time.

> [!Example] (d)
> When will it reach maximum height?

> [!Example] (e)
> How high will it rise?

Nore. These formulas are valid only while the body is rising When it begins to fall, formulas developed in Example 16.27 and problem 22 must be used. Compare with problem 21 where we used one formula to calculate the time for a round trip.

##### Example
#example 
> [!Example] Statement
>  With what velocity will the body of problem 24 return to the earth and how long will it take for its descent? Hint. Read note in problem 24. To find the velocity, use (d) of Example 16.27 with $y$ equal to the value of the maximum height as found in problem $24(\mathrm{e})$. Note that the returning velocity is less than the initial velocity $v_{0}$. To find the time of descent, use the formula for $y$ as found in problem 22. Here it is not easy to see that the time of descent is longer than the time of ascent.


##### Example
#example 
> [!Example] Statement
>  The velocity of a parachutist at the moment his chute opens is $160 \mathrm{ft} / \mathrm{sec}$. The force of the air resistance is $m v^{2} / 8$. Find:

> [!Example] (a)
> His subsequent velocity and distance as functions of time. Take the origin at the point where the chute opens and the positive direction downwards.

> [!Example] (b)
> His velocity 1 sec after his chute opens; 2 sec after.

> [!Example] (c)
> His terminal velocity.

> [!Example] (d)
> How far he falls in the first second; in the second second.

> [!Example] (e)
> Approximately when he reaches the ground if he is $1064 \mathrm{ft}$ above the earth when his chute opens. 

##### Example
#example 
> [!Example] Statement
>  A body of mass $m$ is dropped from a plane flying horizontally 1 mile above the earth. The force of the air resistance is $2 m k^{2} v^{2}$. The terminal velocity of the body is $100 \mathrm{ft} / \mathrm{sec}$. Find:

> [!Example] (a)
> The value of the constant $k$. (Hint. T.V. $=\sqrt{m g / k}$; replace $k$ by $2 m k^{2}$.)

> [!Example] (b)
> The velocity of the body as a function of time.

> [!Example] (c)
> The velocity of the body at the end of 3 sec.

> [!Example] (d)
> When the body reaches a velocity of $60 \mathrm{ft} / \mathrm{sec}$.

##### Example
#example 
> [!Example] Statement
>  A body falls from a great height. Its terminal velocity is $10 \mathrm{ft} / \mathrm{sec}$. (a) Find its velocity and distance equations as functions of time. Hint. T.V. = $\sqrt{m g / k}$. Solve for $k / m$. (b) Find its velocity equation as a function of distance.


##### Example
#example 
> [!Example] Statement
>   A paratrooper and his chute, which together weigh $192 \mathrm{ll}$, drop from an airplane moving horizontally. He opens his chute at the end of 10 sec. Assuming the proportionality constant of air resistance is $1 / 120$ when the chute is closed and $4 / 3$ when it is open, find:

> [!Example] (a)
> His velocity as a function of time before the chute is opened.

> [!Example] (b)
> His terminal velocity before the chute is opened.

> [!Example] (c)
> His velocity at the end of the first 10 sec.

> [!Example] (d)
> His velocity as a function of time after the chute is opened.

> [!Example] (e)
> His terminal velocity after the chute is opened.

> [!Example] (f)
> His velocity at the end of 15 sec, i.e., his velocity 5 sec after the chute is opened.

Solve independently and then check your results with the formulas found in problems 22 and 23.

##### Example
#example 
> [!Example] Statement
>  A man and his parachute weigh $192 \mathrm{lb}$. Assume that a safe landing velocity is $16 \mathrm{ft} / \mathrm{sec}$ and that air resistance is proportional to the square of the velocity, equaling $1 \mathrm{lb}$ for each square foot of cross-sectional area of the parachute when it is moving at $20 \mathrm{ft} / \mathrm{sec}$ at right angles to the direction of motion. What must the cross-sectional area of a parachute be in order that the paratrooper land safely? Hint. First find the force of the air resistance. Then find $k$ such that T.V. $=16$. Then find the number of square feet of parachute that will make the force of the air resistance equal to $k v^{2}$.


##### Example
#example 
> [!Example] Statement
>  With what velocity must a rocket be fired in order to reach a height of 400 $\mathrm{mi}$ above the earth; $4000 \mathrm{mi}$ above the earth? Solve independently. Check your results with (h) of Example 16.36 .[^4]

[^4]:assume no air resistance and that the object is far enough from the earth so that equation (16.35) applies. Use the following data: $R=4000$ miles, $g=32 \mathrm{ft} / \mathrm{sec}^{2}, \sqrt{2 g R}=6.96 \mathrm{mi} / \mathrm{sec}$.

##### Example
#example 
> [!Example] Statement
>  The "air" $200 \mathrm{mi}$ above the earth is so thin that it will hardly slow a space vehicle. It is called the $\mathbf{F}-2$ region of the atmosphere. What velocity should a rocket have at 200 milcs above the earth, if all its fuel is exhausted at that point, in order to go another $3800 \mathrm{mi}$ ?[^4]

##### Example
#example 
> [!Example] Statement
> A body is shot to a height of $400 \mathrm{mi}$ and then starts to fall. What is its velocity (a) when it has fallen $200 \mathrm{mi}$ and (b) when it is at the surface of the earth? Hint. Use (a) of Example 16.38 with $r_{0}=R+400=4400$. Note that the velocity, when it reaches the ground, is the same as the velocity required to propel it 400 miles upward. See problem 31.[^4]

##### Example
#example 
> [!Example] Statement
> Assume a body fals from rest at a distance of $61 R$, i.e., at a distance of $244,000 \mathrm{mi}$ from the center of the earth (equivalent to the moon's distance from the center of the earth). With what velocity and in how many hours will it reach the earth? 35. A body is fired straight up with an initial velocity equal to the escape velocity $\sqrt{2 g R}$. Find:[^4]

> [!Example] (a)
> The velocity of the projectile as a function of the distance $r$ from the center of the earth.

> [!Example] (c)
> When it will have reached $244,000 \mathrm{mi}$, the distance of the moon from the center of the earth.

Hint. Use (e) and (q) of Example 16.36.

##### Example
#example 
> [!Example] Statement
> A body is fired straight up with an initial velocity $v_{0}$ whose magnitude is less than escape velocity. When will it reach its maximum height? Hint. The maximum height the body will reach is given in (h) of Example 16.36. Express this value of $r$ in terms of $a$ and $b$ as defined in (j). Then use (m) and $(\mathrm{n})$. Noте. This time equation is valid only for a rising particle. If you wish to compute its return time, you must use the time equation given in (d) of Example 16.38, with $r_{0}$ equal to the height from which it begins its fall.[^4]

##### Example
#example 
> [!Example] Statement
> Show that if $v_{0}$ is very much less than the escape velocity $\sqrt{2 g R}$, then the time for the object to reach its maximum height, as given in problem 36 , is approximately $v_{0} / g$. Hint. Replace the Arc sin function by its series expansion, Arc $\sin x=x+x^{3} / 6+\cdots$. Then eliminate $v_{0}^{2}$ and higher powers of $v_{0}$. To see some justification for this elimination, let $v_{0}=\frac{1}{30}$ $\mathrm{mi} / \mathrm{sec}$ in the time equation of problem 36 .[^4]

##### Example
#example 
> [!Example] Statement
> A body is shot straight up from the surface of the moon with an initial velocity $v_{0}$. (a) Find the velocity $v$ of the body as a function of its distance $r_{m}$ from the center of the moon. (b) Find the escape velocity of the body. The radius of the moon is approximately $1080 \mathrm{mi}$; the acceleration of gravity on the surface of the moon is approximately one-sixth that of the earth. Take the outward direction from the moon as positive.[^4]

##### Example
#example 
> [!Example] Statement
> (a) Prove that if a particle were placed approximately nine-tenths of the distance $D$ from the center of the earth to the center of the moon on a line connecting moon to earth, Fig. 16.392, the particle would be at rest, i.e., the gravitational pulls of moon and earth on a particle placed nine-tenths of the distance from the center of earth to the center of the moon are equal. Assume the mass of the moon is $1 / 81$ the mass of the earth. Hint. Apply (16.32) to both earth and moon. Then equate the two forces.[^4]

![[../Images/Horizontal Motion.svg]]


Figure 16.392

> [!Example] (b)
> Set up the differential equation of motion of a particle shot from the surface of the earth toward the moon, considered fixed, taking into account both the earth's and moon's gravitational attractions. Solve the equation with $t=0, v=v_{0}$. Take the positive direction as outward from the earth, and the orgin at the earth's center.

> [!Example] (c)
> At what velocity must the particle be fired in order to reach the neutral point? Hint. In the velocity equation found in (b), you want $v=0$, when $r=\frac{9}{10} D$. Assume $R_{m}^{2}=6 R^{2} / 81, D=61 R+R / 4$, $g_{m}=g / 6$, where $R_{m}$ is the radius of the moon and $g_{m}$ is the acceleration due to the force of gravity of the moon. Note the small effect of the moon's gravitational attraction.

> [!Example] (d)
> At what velocity must a projectile be fired in order to reach the moon?

##### Example
#example 
> [!Example] Statement
>  If a body were dropped in a hole bored through the center of the earth, it would be attracted toward the center with a force directly proportional to the distance of the body from the center.

> [!Example] (a)
> With what velocity will it pass the center?

> [!Example] (b)
> When will it reach the other end of the hole?

NoтE. The motion of the body is known as simple harmonic motion. A fuller discussion of this motion can be found in Lesson 28.

## ANSWERS 16A

2. (a) $v=-32 t+80, y=-16 t^{2}+80 t$. (b) $48 \mathrm{ft} / \mathrm{sec}, 64 \mathrm{ft}$.
(c) $2 \frac{1}{2} \mathrm{sec}, 100 \mathrm{ft}$.
(d) $5 \mathrm{sec},-80 \mathrm{ft} / \mathrm{sec}$. (e) No.
3. $70.56 \mathrm{ft}$.
4. (a) $2 \frac{1}{2} \mathrm{sec}, 88 \mathrm{ft} / \mathrm{sec}$ (b) $20 \mathrm{ft}$ (c) $80 \mathrm{ft} / \mathrm{sec}$.

(d) If 3 sec ago, the ball were thrown upward from the ground with a velocity of $88 \mathrm{ft} / \mathrm{sec}$, it would reach a height of $121 \mathrm{ft}$ and have a velocity of $8 \mathrm{ft} / \mathrm{sec}$ as it passed the $120 \mathrm{ft}$ point on its way down.

5. $44 \mathrm{ft} / \mathrm{sec}$.
6. (a) 5 slugs. $\quad$ (b) $26 \frac{2}{3} l$ b. $\quad$ (c) $v=-\frac{16 t}{3}+c_{1}, y=-\frac{8}{3} t^{2}+c_{1} t+c_{2}$.
7. $384 \mathrm{ft}, 12 \mathrm{sec}$.
8. $15 \mathrm{ft}$ if one assumes he scales the bar horizontally so that he raises his center of gravity $2 \mathrm{ft}$ on earth; 6 times as long.
9. $150 \mathrm{lb}$.
10. (a) $v=\frac{m g}{k}\left(1-e^{-k t / m}\right), \quad y=\frac{m g}{k} t+\frac{m^{2} g}{k^{2}}\left(e^{-k t / m}-1\right)$.

(b) T.V. $=m g / k$.

11. $v=\frac{m g}{k}\left(1-e^{-k t / m}\right)+v_{0} e^{-k t / m}$,

$y=\frac{m g}{k} t-\left(\frac{m^{2} g}{k^{2}}-\frac{m v_{0}}{k}\right)\left(1-e^{-k t / m}\right)$.

$\mathrm{T} . \mathrm{V} .=m g / k$, same as in $10(\mathrm{~b})$.

12. (a) $v=16\left(1-e^{-2 t}\right), y=16 t+8\left(e^{-2 t}-1\right)$. (b) $16 \mathrm{ft} / \mathrm{sec}$.

(c) $y=8\left(19+e^{-20}\right)=152 \mathrm{ft}, v=16\left(1-e^{-20}\right)=16 \mathrm{ft} / \mathrm{sec}$.

13. (a) $v=16\left(1-e^{-2 t}\right)+170 e^{-2 t}=16+154 e^{-2 t}, y=16 t+77\left(1-e^{-2 t}\right)$.

(b) $16 \mathrm{ft} / \mathrm{sec}$.

(c) $237 \mathrm{ft}, 16 \mathrm{ft} / \mathrm{sec}$.

14. (a) $k=\frac{8}{7}$.

(b) $v=175\left(1-e^{-32 t / 175}\right), \quad y=175 t+\frac{175^{2}}{32}\left(e^{-32 t / 175}-1\right)$.

(c) $139.7 \mathrm{ft} / \mathrm{sec}, 167.9,173.6,174.6$. (d) $4791 \mathrm{ft}$. 15. (a) 12.5. (b) $v=16\left(1-e^{-2 t}\right)+175 e^{-2 t}, y=16 t+\frac{159}{2}\left(1-e^{-2 t}\right)$.

(c) $37.5 \mathrm{ft} / \mathrm{sec}, 18.9,16.4,16.05,16.01$.

(d) $159.5 \mathrm{ft}$.

(e) Over $160 \mathrm{ft}$ (f) Approx. $60 \mathrm{sec}$.

16. (a) $v=180\left(1-e^{-8 t / 45}\right), y=180 t+\frac{180^{2}}{32}\left(e^{-8 t / 45}-1\right)$.

(b) $v\left(11 \frac{1}{4}\right)=180\left(1-e^{-2}\right)=155.6 \mathrm{ft} / \mathrm{sec}, 176.7,179.6,179.9$.

(c) Approx. $7100 \mathrm{ft}$.

17. $k=2 ; \mathrm{T} . \mathrm{V} .=50 \mathrm{ft} / \mathrm{sec} ; v=50-30 e^{-0 t / 50}=50-30 e^{-0.64 t}$;

$y=50 t-\frac{1500}{g}\left(1-e^{-0 t / 50}\right)=50 t-\frac{375}{8}\left(1-e^{-0.64 t}\right)$.

18. (a) $v=14\left(1-e^{-2 t}\right), y=14\left[t-\frac{3}{2}\left(1-e^{-2 t}\right)\right]$.

(b) T.V. $=14 \mathrm{ft} / \mathrm{sec}$.

19. (a) $v=72\left(1-e^{-t / 3}\right)$,

(b) T.V. $=72 \mathrm{ft} / \mathrm{sec}$.

20. (a) $v=\frac{g m}{k}\left(e^{-k t / m}-1\right)+v_{0} e^{-k t / m}$,

$$
y=\left(\frac{g m^{2}}{k^{2}}+\frac{m v_{0}}{k}\right)\left(1-e^{-k t / m}\right)-\frac{g m}{k} t .
$$

(b) $y=\frac{m}{k}\left(v_{0}-\frac{g m}{k} \log \frac{k w_{0}+g m}{g m}\right), t=\frac{m}{k} \log \frac{k w_{0}+g m}{g m}$.

21. (a) $v=16\left(7 e^{-2 t}-1\right), y=56\left(1-e^{-2 t}\right)-16 t$.

(b) $y=8(6-\log 7)=32.4 \mathrm{ft}, t=\frac{1}{\mathrm{log}} 7=0.97 \mathrm{sec}$.

(c) $3.5 \mathrm{sec}$ approx., $-16 \mathrm{ft} / \mathrm{sec}$.

(d) Same answer.

22. $v=\sqrt{m g / k} \tanh \sqrt{g k / m} t=\sqrt{m g / k} \frac{e^{2 \sqrt{k g / m} t}-1}{e^{2 \sqrt{k g / m} t}+1}$,

$y=\frac{m}{k} \log \cosh \sqrt{g k / m} t=\frac{m}{k} \log \frac{e^{\sqrt{g k l m} t}+e^{-\sqrt{g k t m} t}}{2}$,

T.V. $=\sqrt{m g / k} \mathrm{ft} / \mathrm{sec}$.

For definitions of cosh and tanh, see (18.91) and (18.92).

23. (a) $v^{2}=\frac{m g}{k}\left(1-e^{-2 k y / m}\right)+v_{0}^{2} e^{-2 k y / m}$. Since the positive direction is downward, the positive square root must be taken for $v$ when the body is falling.

(b) $\frac{\sqrt{k} v+\sqrt{m g}}{\sqrt{k} v-\sqrt{m g}}=\frac{\sqrt{k} v_{0}+\sqrt{m g}}{\sqrt{k} v_{0}-\sqrt{m g}} e^{2 \sqrt{g k l m} !}$.

(c) T.V. $=\sqrt{m g / k} \mathrm{ft} / \mathrm{sec}$.

24. (a) $v^{2}=\frac{m g}{k}\left(e^{-2 k y / m}-1\right)+v_{0}^{2} e^{-2 k y / m}$. Since the positive direction is upward, the positive square root must be taken for $v$ when the body is rising.

(b) $t=\sqrt{m / k g}\left(\operatorname{Arctan} \sqrt{k / m g} v_{0}-\operatorname{Arctan} \sqrt{k / m g} v\right.$ ), or $v=\sqrt{g m / k} \tan (c-\sqrt{k g / m} t)$, where $c=\operatorname{Arctan} \sqrt{k / g m} v_{0}$. The formula for the time $t$ is valid only for a rising body. (c) $y=\frac{m}{k} \log \left[\frac{\cos (c-\sqrt{k g / m} t)}{\cos c}\right]$.

(d) $t=c \sqrt{m / k g}$.

(e) $y=\frac{m}{k} \log \sec c$.

25. $v=\sqrt{m g /\left(k v_{0}^{2}+m g\right)} v_{0} \mathrm{ft} / \mathrm{sec}$,

$t=\sqrt{m / g k} \cosh ^{-1} \sec c$ or $\cosh \sqrt{g k / m} t=\sec c$,

where $c=\operatorname{Arctan} \sqrt{k / g m} v_{0}$. For definition of cosh, see (18.91).

26. (a) $v=16 \frac{11+9 e^{-4 t}}{11-9 e^{-4 t}}, y=16 t+8 \log \left(5.5-4.5 e^{-4 t}\right)$.
(b) $16.49 \mathrm{ft} / \mathrm{sec}, 16.009 \mathrm{ft} / \mathrm{sec}$.
(c) $16 \mathrm{ft} / \mathrm{sec}$.
(d) $29.5 \mathrm{ft} ., 16.1 \mathrm{ft}$.
(e) 66 sec.
27. (a) $k=1 / 25$.

(b) $v=100 \frac{e^{0.64 t}-1}{e^{0.64 t}+1}$.

(c) $74.4 \mathrm{ft} / \mathrm{sec}$.

(d) 2.2 sec.

28. (a) $v=10 \tanh (3.2 t)=10 \frac{e^{6.4 t}-1}{e^{6.4 t}+1}$,

$$
y=\frac{100}{g} \log \cosh 3.2 t=\frac{100}{g} \log \frac{e^{3.2 t}+e^{-3.2 t}}{2} \text {. }
$$

(b) $v=10 \sqrt{1-e^{-0.64 y}}$.

29. (a) $v=151.8 \frac{e^{2 t \sqrt{10} / 15}-1}{e^{2 t \sqrt{10} / 15}+1}$.

(b) T.V. $=151.8 \mathrm{ft} / \mathrm{sec}$.

(c) $147.4 \mathrm{ft} / \mathrm{sec}$.

(d) $v=12 \frac{1.18 e^{16 t / 3}+1}{1.18 e^{16 t 3}-1}$.
(e) $12 \mathrm{ft} / \mathrm{sec}$.
(f) $12 \mathrm{ft} / \mathrm{sec}$.

30. $600 \mathrm{sq} \mathrm{ft}$.
31. $2.10 \mathrm{mi} / \mathrm{sec}, 4.92 \mathrm{mi} / \mathrm{sec}$.
32. $4.7 \mathrm{mi} / \mathrm{sec}$.
33. (a) $1.45 \mathrm{mi} / \mathrm{sec}$.

(b) $2.10 \mathrm{mi} / \mathrm{sec}$.

34. $6.9 \mathrm{mi} / \mathrm{sec}, 119$ hours.
35. (a) $v=R \sqrt{2 g / r}$,

(b) 50.6 hours.

36. $t=-\frac{1}{b} \sqrt{a R+b R^{2}}-\frac{a}{2 b \sqrt{-b}}\left[\frac{\pi}{2}-\operatorname{Arc} \sin \frac{-2 b R-a}{a}\right]$.

$$
\begin{aligned}
t & =\frac{g R^{2}}{\left(2 g R-v_{0}^{2}\right)^{3 / 2}}\left[\frac{v_{0} \sqrt{2 g R-v_{0}^{2}}}{g R}+\operatorname{Arccos}\left(\frac{g R-v_{0}^{2}}{g R}\right)\right] \\
& =\frac{g R^{2}}{\left(2 g R-v_{0}^{2}\right)^{3 / 2}}\left[\frac{v_{0} \sqrt{2 g R-v_{0}^{2}}}{g R}+2 \operatorname{Arcsin} \frac{v_{0}}{\sqrt{2 g R}}\right] .
\end{aligned}
$$

38. (a) $v^{2}=v_{0}^{2}+\frac{g R_{m}}{3}\left(\frac{R_{m}}{r_{m}}-1\right)$, where $R_{m}$ is the radius of the moon.

(b) $v_{0}=\sqrt{g R_{m} / 3}=1.48 \mathrm{mi} / \mathrm{sec}$.

39. (b) $v \frac{d v}{d r}=\frac{-g R^{2}}{r^{2}}+\frac{g_{m} R_{m}^{2}}{(D-r)^{2}}$,

$$
v^{2}=2 g \frac{R^{2}}{r}+\frac{2 g_{m} R_{m}^{2}}{D-r}+v_{0}^{2}-2 g R-\frac{2 g_{m} R_{m}^{2}}{D-R} .
$$

(c) $v_{0}=\sqrt{2 g R}(0.99)=99$ percent of the escape velocity of the earth when the gravitational pull of the moon is ignored.

(d) The velocity must have a positive value when it reaches the neutral point. See $(c)$.

40. (a) $4.9 \mathrm{mi} / \mathrm{sec}$. (b) $42.5 \mathrm{~min}$.



