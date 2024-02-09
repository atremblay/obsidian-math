## Statement

An insect steps on the edge of a turntable of radius $a$ that is rotating at a constant angular velocity $\alpha$. It moves straight toward the center of the table at a constant velocity $v_{0}$. Find the equation of its path in polar coordinates, relative to axes fixed in space. (Hint. If $\theta$ is the angle through which the turntable has rotated in time $t$ and $r$ is the distance of the insect from the center at that moment, then $d \theta / d t=\alpha, r(d \theta / d t)=r \alpha$.) Draw a graph of the equation if $a=10 \mathrm{ft}, \alpha=\pi / 4$ radians/sec, $v_{0}=1 \mathrm{ft} / \mathrm{sec}$. How many revolutions will the table have made by the time the insect reaches the center?

### Solution
Requires [[../../Motion of a Particule/Free motion|Free motion]]
$$\frac{dr}{dt}=-v, \qquad r\frac{d\theta}{dt}=r\alpha$$

$$
\begin{align}
\frac{dr}{\cancel{r}d\theta}&=\frac{-v}{\cancel{r}\alpha} \\
\int \frac{dr}{d\theta} d\theta &=\int \frac{-v}{\alpha} d\theta \\
\int dr &=\int \frac{-v}{\alpha} d\theta \\
r &= \frac{-v\theta}{\alpha} + C \tag{a}
\end{align} 
$$

With the initial conditions that when $t=0$ then $r=a$ and $\theta=0$. Replace in (a)
$$
\begin{align}
r &= \frac{-v\theta}{\alpha} + C \\
a &= \frac{-v0}{\alpha} + C \\
a&=C \tag{b}
\end{align} 
$$

Replace $C$ from (b) in (a)
$$
\boxed{r = a-\frac{v\theta}{\alpha}} \tag{c}
$$


> [!step]- Alternative Step
> Using the initial conditions in the integral limits will save the explicit calculation for $C$
> $$
> \begin{align}
> \frac{dr}{d\theta}&=\frac{-v}{\alpha} \\
> \int_a^r \frac{dr}{d\theta} d\theta &=\int_0^\theta \frac{-v}{\alpha} d\theta \\
> \int dr &=\int \frac{-v}{\alpha} d\theta \\
> r \bigg |_a^r &= \frac{-v\theta}{\alpha} \bigg |_0^\theta \\
> r-a &= \frac{-v\theta}{\alpha} 
> \end{align} 
> $$
> 
> $$\boxed{r =a - \frac{v\theta}{\alpha}}$$

Now, given the conditions of the statement $a=10$, $\alpha=\frac{\pi}{4}$ and $v=1$, plug that in (c)

$$
\begin{align}
\theta&=\frac{a\alpha}{v} \\
&=10 \frac \pi 4 \\
&= \frac{5 \pi}{2}
\end{align}
$$



5. Assume in problem 4 that the insect always moves in a direction parallel to the diameter drawn through the point where he steps on the table.

(a) Find the equation of its path relative to axes fixed in space.

(b) What kind of curve is it? Hint. Change the equation to rectangular coordinates if the polar form is unfamiliar to you.


```desmos-graph
y=10+40*\sin(x)/\pi
```
