
## 1. LESSON 34. Plane Motions Giving Rise to Systems of Equations.

In Lesson 16, we discussed the motion of a particle constrained to move along a straight line. In this lesson, we consider the motion of a particle free to move in a plane.

LESSON 34A. Derivation of Velocity and Acceleration Formulas. If a particle is free to move in a plane, then a change in the direction of its velocity will be equally as important as a change in the magnitude of its velocity. As mentioned in Lesson $16 \mathrm{C}$, quantities in which both magnitude and direction play.a role are called vectors.

Since Newton's second law of motion is also applicable to particles which move in a plane, we have, by (16.1),

$$
F=m a=m \frac{d v}{d t}=m \dot{v} .
$$

Remark. A dot over a variable means its derivative with respect to time; two dots over the variable means its second derivative with respect to time. Hence $\dot{x} \equiv d x / d t, \vec{x} \equiv d^{2} x / d t^{2}, v \equiv d v / d t$, etc.

The mass $m$ is not a vector quantity. We therefore see, by (34.1), that the acceleration of a particle acted on by a force, not only has magnitude $F / m$, but also has the same direction as $F$.

In Fig. 34.11, the vector $F$ represents the magnitude and direction of a force $F$. It is convenient to break up this vector force into two components, one, $F_{x}$, to represent that part of the force which accelerates the particle in the $x$ direction, the other, $F y$, to represent that part of the force which accelerates the particle in the

![](https://cdn.mathpix.com/cropped/2023_10_07_819562498d978d1c8215g-473.jpg?height=264&width=485&top_left_y=1277&top_left_x=642)

Figure 34.11 $y$ direction. If the inclination of the force $F$ is $\theta$, we see from Fig. 34.11, that

$$
F_{x}=F \cos \theta, \quad F_{y}=F \sin \theta .
$$

Since $F_{x}=$ mass times $a_{x}$, the acceleration of a particle in the $x$ direction and $F_{y}=$ mass times $a_{y}$, the acceleration of a particle in the $y$ direction, we obtain from (34.12), the system of equations

$$
\begin{aligned}
& F \cos \theta=F_{x}=m a_{x}=m \frac{d^{2} x}{d t^{2}}=m \ddot{x}, \\
& F \sin \theta=F_{y}=m a_{y}=m \frac{d^{2} y}{d t^{2}}=m \ddot{y} .
\end{aligned}
$$

> [!blank-container|right-medium]
> ![[Images/free_motion_1.svg]]
> > Figure 1


In a similar manner we can break up any vector quantity into its $x$ and $y$ components. In Fig. (34.14) we have shown, for example, the $x$ and $y$ components of a velocity vector $\mathrm{v}$.


For certain problems, it is often convenient to use polar coordinates instead of rectangular coordinates. The vector quantity is then broken up into two components: one along the radial $r$ direction, the other in a direction perpendicular to it. In Fig. 34.15, we have broken up the vector $v$ into these two components $v_{r}$ and $v_{\theta}$.


Figure 34.15

> [!blank-container|right-medium]
> ![[Images/free_motion_2.svg]]
> > Figure 2

Let $(x, y)$ be the coordinates of a point $P$ in a rectangular system and $(r, \theta)$ its coordinates in a polar system, Fig. 34.15. Then we see from the figure, that

$$
x=r \cos \theta, \quad y=r \sin \theta . \tag{a}
$$

Differentiation of (a) gives

$$
\begin{aligned}
& \frac{d x}{d t}=\cos \theta \frac{d r}{d t}-r \sin \theta \frac{d \theta}{d t}, \\
& \frac{d y}{d t}=\sin \theta \frac{d r}{d t}+r \cos \theta \frac{d \theta}{d t} .
\end{aligned} \tag{b}
$$

A second differentiation gives

$$
\begin{aligned}
& \frac{d^{2} x}{d t^{2}}=\cos \theta \frac{d^{2} r}{d t^{2}}-2 \sin \theta \frac{d r}{d t} \frac{d \theta}{d t}-r \cos \theta\left(\frac{d \theta}{d t}\right)^{2}-r \sin \theta \frac{d^{2} \theta}{d t^{2}} \\
& \frac{d^{2} y}{d t^{2}}=\sin \theta \frac{d^{2} r}{d t^{2}}+2 \cos \theta \frac{d r}{d t} \frac{d \theta}{d t}-r \sin \theta\left(\frac{d \theta}{d t}\right)^{2}+r \cos \theta \frac{d^{2} \theta}{d t^{2}}
\end{aligned} \tag{c}
$$

These formulas, (b) and (c), are valid for every value of $\theta$. Hence they must hold in particular when $\theta=0$. But when $\theta=0, x=r$ and the direction perpendicular to $r$ is the $y$ direction. Hence the components of velocity and acceleration in a radial direction and in a direction perpendicular to it are

$$
v_{r}=\frac{d x}{d t}, \quad v_{\theta}=\frac{d y}{d t}, \quad a_{r}=\frac{d^{2} x}{d t^{2}}, \quad a_{\theta}=\frac{d^{2} y}{d t^{2}}
$$

Substituting the first two equations of (34.19) in (34.17), the second two in (34.18), we obtain with $\theta=0$,

$$
\begin{aligned}
& v_{r}=\frac{d r}{d t}=\dot{r}, \quad v_{\theta}=r \frac{d \theta}{d t}=r \dot{\theta} . \\
& a_{r}=\frac{d^{2} r}{d t^{2}}-r\left(\frac{d \theta}{d t}\right)^{2}=\ddot{r}-r \hat{\theta}^{2}, \\
& a_{\theta}=2 \frac{d r}{d t} \frac{d \theta}{d t}+r \frac{d^{2} \theta}{d t^{2}}=2 \dot{r} \dot{\theta}+r \dot{\theta} .
\end{aligned}
$$

Formulas (34.2) and (34.21) give respectively the components of the velocity and acceleration vectors of a particle along the radial axis and in a direction perpendicular to it, at the point $P$ where the curve crosses the $x$ axis. Since the $x$ axis can be chosen in any direction, these equations are valid for every point $P$ of the particle's path.

## 2. EXERCISE 34A

1. In Fig. 34.22, we have shown the $x$ and $y$ components of a velocity vector $\mathbf{v}$ as well as its components in a radial direction and in a direction perpendicular to it. With the aid of this diagram, prove (34.2). Hint. 


> [!blank-container|right-large]
> ![[Images/free_motion_3.svg]]
> > Figure 3


First show that

$$
v_{x}=\frac{d x}{d t}=v \cos \alpha, \quad v_{y}=\frac{d y}{d t}=v \sin \alpha .
$$

Then show that

(b)

$$
\begin{aligned}
v_{r} & =v \cos (\alpha-\theta)=v \cos \alpha \cos \theta+v \sin \alpha \sin \theta, \\
& =\frac{d x}{d t} \cos \theta+\frac{d y}{d t} \sin \theta \\
&= \left(\cos \theta \frac{d r}{d t}-r \sin \theta \frac{d \theta}{d t}\right)\cos\theta + \left(\sin \theta \frac{d r}{d t}+r \cos \theta \frac{d \theta}{d t}\right) \sin \theta \\
&= \cos^2 \theta \frac{d r}{d t}\cancel{-r \sin \theta \cos\theta\frac{d \theta}{d t}} + \sin^2 \theta \frac{d r}{d t}+\cancel{r \sin\theta \cos \theta \frac{d \theta}{d t}} \\
&= (\cos^2 \theta + \sin^2\theta) \frac{d r}{d t} \\
&= \frac{dr}{dt}
\end{aligned}
$$

In (b), replace $d x / d t$ and $d y / d t$ by their values as given in (34.17).

Similarly, show that

(c)

$$
\begin{aligned}
v_{\theta} & =v \sin (\alpha-\theta) =v \sin \alpha \cos \theta-v \cos \alpha \sin \theta \\
& =\frac{d y}{d t} \cos \theta-\frac{d x}{d t} \sin \theta \\
& =\left(\sin \theta \frac{d r}{d t}+r \cos \theta \frac{d \theta}{d t}\right) \cos \theta-\left(\cos \theta \frac{d r}{d t}-r \sin \theta \frac{d \theta}{d t}\right) \sin \theta \\
& =\cancel{\sin \theta \cos\theta \frac{d r}{d t}}+r \cos^2 \theta \frac{d \theta}{d t} \cancel{-\sin\theta \cos \theta \frac{d r}{d t}}+r \sin^2 \theta \frac{d \theta}{d t} \\
& =r \frac{d\theta}{dt}\left( \cos^2 \theta  + \sin^2 \theta \right) \\
& =r \frac{d\theta}{dt} \\
\end{aligned}
$$

In (c), replace $d y / d t$ and $d x / d t$ by their values as given in (34.17).

![](https://cdn.mathpix.com/cropped/2023_10_07_819562498d978d1c8215g-476.jpg?height=459&width=918&top_left_y=1206&top_left_x=133)

Figure 34.23

2. In Fig. 34.23, we have shown the $x$ and $y$ components of an acceleration vector a as well as its components in a radial direction and in a direction perpendicular to it. With the aid of this diagram, prove (34.21). Hint. First show that

$$
a_{x}=\frac{d^{2} x}{d t^{2}}=a \cos \alpha, \quad a_{y}=\frac{d^{2} y}{d t^{2}}=a \sin \alpha .
$$

Then show that

$$
\begin{aligned}
a_{r} & =a \cos (\alpha-\theta)=a \cos \alpha \cos \theta+a \sin \alpha \sin \theta \\
& =\frac{d^{2} x}{d t^{2}} \cos \theta+\frac{d^{2} y}{d t^{2}} \sin \theta .
\end{aligned}
$$

In (b), replace $d^{2} x / d t^{2}$ and $d^{2} y / d t^{2}$ by their values as given in (34.18). Similarly, show that

$$
\begin{aligned}
a_{\theta} & =a \sin (\alpha-\theta)=a \sin \alpha \cos \theta-a \cos \alpha \sin \theta \\
& =\frac{d^{2} y}{d t^{2}} \cos \theta-\frac{d^{2} x}{d t^{2}} \sin \theta .
\end{aligned}
$$

In (c) replace $d^{2} y / d t^{2}$ and $d^{2} x / d t^{2}$ by their values as given in (34.18).

3. A particle of mass $m$ is attracted to a fixed point $O$ by a force $F$. The particle moves in a plane with a constant speed but not in a straight line. Show that the particle moves in a circle with center at $o$. Hint. The component $a_{t}$ of the acceleration vector $a$ in a direction tangent to the path of the particle measures the change in the speed of the particle. Since this speed is constant, $a_{t}=0$. Hence the acceleration acts only in a direction perpendicular to the path of the particle. And since $F$ and $a$ are in the same direction, $F$ and the constant speed $v_{0}$ are perpendicular to each other. Then show $d y / d x=-x / y$.

## 3. LESSON 34B. The Plane Motion of a Projectile.

Example 34.3. A particle of mass $m$ is projected from the earth with a velocity $v_{0}$ at an angle $\alpha$ with the horizontal. The only force acting on it is that of gravity. Assuming a level terrain, find:

1. The equation of the particle's path.
2. Its horizontal range.
3. The maximum height it will reach.
4. The value of $\alpha$ for which the range will be a maximum.

5 . When the particle will reach the ground.

Solution. Refer to Fig. 34.31. We take the $x$ and $y$ axes in a plane which is perpendicular to the ground, and which contains the given velocity vector $\mathrm{v}_{0}$. The $z$ axis is, as usual, perpendicular to both $x$ and $y$ axes. Since by assumption the only active force $F$ is that of gravity, the components of $F$ in the $x, y, z$ directions are respectively $F_{x}=0, F_{y}=$ $-m g, F_{z}=0$. Hence

$$
m \ddot{x}=0, \quad m \ddot{y}=-m g, \quad m \ddot{z}=0 .
$$

464 Probleas: Systeas. Spectar 2nd Order Equations

Integration of (a) gives

(b)

$$
v_{x}=\dot{x}=c_{1}, \quad v_{y}=\dot{y}=-g t+c_{2}, \quad v_{z}=z=c_{3} .
$$

Since the velocity vector $\mathrm{v}_{0}$ lies in the $x y$ plane, it follows that at $t=0$, i.e., at the moment of projection, see Fig. 34.31,

$$
v_{x}=v_{0} \cos \alpha, \quad v_{y}=v_{0} \sin \alpha, \quad v_{z}=0,
$$

where $\alpha$ is the angle which $\mathrm{v}_{0}$ makes with the horizontal $x$ axis. Substituting these values in (b), we find

$$
c_{1}=v_{0} \cos \alpha, \quad c_{2}=v_{0} \sin \alpha, \quad c_{3}=0 .
$$

Hence (b) becomes

(e) $\quad v_{x}=\dot{x}=v_{0} \cos \alpha, \quad v_{y}=\dot{y}=-g t+v_{0} \sin \alpha, \quad v_{z}=\dot{z}=0$.

Integration of (e) gives

(f) $x=\left(v_{0} \cos \alpha\right) t+c_{4}, \quad y=-g \frac{t^{2}}{2}+\left(v_{0} \sin \alpha\right) t+c_{5}, \quad z=c_{6}$.

If we now choose our origin at the point where the particle is projected, then at $t=0, x=0, y=0, z=0$. Substituting these values in (f), we

![](https://cdn.mathpix.com/cropped/2023_10_07_819562498d978d1c8215g-478.jpg?height=337&width=510&top_left_y=960&top_left_x=358)

Figure 34.31

find $c_{4}=0, c_{5}=0, c_{6}=0$. The parametric equations of the path of the particle are therefore

Since $z=0$, it follows that a projectile subject only to a gravitational force moves in a plane containing the vector $\mathrm{v}_{0}$.

To find the equation of the path in rectangular coordinates, we eliminate $t$ by solving the first equation in (g) for $t$ and substituting this value in the second equation. There results

$$
y=(\tan \alpha) x-\left(\frac{g \sec ^{2} \alpha}{2 v_{0}^{2}}\right) x^{2}
$$

which is the equation of a parabola through the origin. Since the coefficient of $x^{2}$ is negative, the curve is concave downward.

The horizontal range of the projectile, i.e., the distance from the origin to the point where the particle strikes the ground, is obtained by setting $y=0$ in $(\mathrm{h})$, for when $y=0$ the particle is at ground level. Equation (h) then becomes, if $\alpha \neq \pi / 2$,

$$
\begin{aligned}
& 0=(\sin \alpha) x-\frac{g}{2} \frac{x^{2}}{v_{0}^{2} \cos \alpha}, \quad x=\frac{v_{0}^{2}}{g}(2 \sin \alpha \cos \alpha), \\
& x=\frac{v_{0}^{2}}{g} \sin 2 \alpha
\end{aligned}
$$

which gives the horizontal range of the particle.

The maximum height is reached when $y^{\prime}=0$. Therefore, differentiating (h) with respect to $x$ and setting $y^{\prime}=0$, we obtain

$$
0=\tan \alpha-\frac{g \sec ^{2} \alpha}{v_{0}^{2}} x, \quad x=\frac{v_{0}^{2}}{g} \sin \alpha \cos \alpha .
$$

When $x$ has the value in (j), $y$ by (h) has the value

$$
y=\frac{v_{0}^{2}}{g} \sin ^{2} \alpha-\frac{v_{0}^{2}}{2 g} \sin ^{2} \alpha=\frac{v_{0}^{2}}{2 g} \sin ^{2} \alpha,
$$

which gives the maximum height reached by the particle.

The range will be a maximum when, in (i), $\alpha$ is so chosen that $x$ is a maximum. Hence differentiating the last equation in (i) with respect to $\alpha$, and setting $d x / d \alpha=0$, we have

$$
0=\frac{2 v_{0}^{2}}{g} \cos 2 \alpha, \quad \alpha=\frac{\pi}{4} .
$$

The range therefore will be a maximum if the projectile is fired at an angle of $45^{\circ}$. By (i), this maximum range is $v_{0}^{2} / g$.

The particle will reach the ground when $y=0$. Setting $y=0$ in $(\mathrm{g})$, we obtain

$$
\left(v_{0} \sin \alpha\right) t-\frac{g t^{2}}{2}=0, \quad t=\frac{2 v_{0}}{g} \sin \alpha,
$$

which is the time it takes the particle to reach the ground.

## 4. EXERCISE 34B

In problems 1-14, assume the air resistance is negligible.

1. A projectile is fred from the earth with a velocity of $1600 \mathrm{ft} / \mathrm{sec}$ at an angle of $45^{\circ}$. Find the equation of motion, the maximum height reached and the range of the projectile. 2. Start with the equation of motion (h) of Example 34.3.

(a) Show that the coordinates of the vertex of the parabola are

$$
\left(\frac{v_{0}^{2} \sin \alpha \cos \alpha}{g}, \frac{v_{0}^{2} \sin ^{2} \alpha}{2 g}\right) .
$$

(b) Show that the distance of the vertex to the focus is $v_{0}^{2} \cos ^{2} \alpha / 2 g$, and therefore that the equation of the directrix is $y=v_{0}^{2} / 2 g$. Note that the equation has no $\alpha$ in it. Hence the parabolic orbits of all projectiles fired with a given velocity have the same directrix regardless of the angle at which they are fired.

(c) Finally show that this constant height of the directrix above the horizontal is the distance a projectile reaches when fired straight up with an initial velocity of $v_{0} \mathrm{ft} / \mathrm{sec}$.

If you have succeeded in answering the above questions, you have proved that every parabolic orbit of a projectile fired with the same velocity $v_{0}$ has the same constant directrix whose height is the distance the projectile would reach if fired vertically.

3. We showed in Example 34.3 that if a projectile is fired at an angle of $45^{\circ}$, its range will be a maximum and will equal $v_{0}{ }^{2} / g$. An artillery piece, whose muzzle velocity is $v_{0} \mathrm{ft} / \mathrm{sec}$, is located at a distance $D<v_{0}^{2} / g$ from an object at the same level as itself. Show that there are two angles at which the artillery piece can be fired and hit the object-one as much greater than $45^{\circ}$ as the other is less. Find these angles. Hint. In (h) of Example 34.3, you want $\alpha$ such that when $y=0, x=D$. Make use of the identity $2 \sin \alpha \cos \alpha=\sin 2 \alpha=\cos \left(2 \alpha-\frac{\pi}{2}\right)$.
4. The muzzle velocity of an artillery piece is $800 \mathrm{ft} / \mathrm{sec}$. Assuming a level terrain, answer the following questions.

(a) An object is $3.8 \mathrm{mi}$ away. Can it be hit?

(b) An object is $15,000 \mathrm{ft}$ away. At what angles must the artillery piece be fired in order to hit the object?

(c) What is the maximum height reached by the shell of (b)?

(d) When did the shell reach the object?

(e) If a mountain of height $6000 \mathrm{ft}$ is $4000 \mathrm{ft}$ from the artillery piece, is it still possible to hit the object?

5. A projectile, fired with a velocity of $96 \mathrm{ft} / \mathrm{sec}$, reaches its maximum height in 2 sec. Assume a level terrain.

(a) Find the angle of projection of the particle. Hint. $\frac{d y}{d x}=\frac{d y / d t}{d x / d t}$. Therefore $d y / d x=0$ if $d y / d t=0$.

(b) Find the maximum height reached by the particle.

(c) What is the range of the projectile from the point fired?

6. A projectile is fired from a height of $y_{0} \mathrm{ft}$ above a level terrain, with a velocity of $v_{0} \mathrm{ft} / \mathrm{sec}$ and at an angle $\alpha$ with the horizontal. Find:

(a) The equation of the particle's path.

(b) Its horizontal range-take the $x$ axis on ground level.

(c) Its maximum height.

(d) When it will reach the ground.

(e) At what angle and with what velocity it will strike the ground. (Hint. If $\theta$ is the angle, $\tan \theta=(d y / d t) /(d x / d t)$ and $\left.|v|=\sqrt{(d x / d t)^{2}+(d y / d t)^{2}}\right)$.

(f) The value of $\alpha$ that will make the range a maximum. 7. A projectile is fired from a height of $50 \mathrm{ft}$ above a level terrain with a velocity of $64 \mathrm{ft} / \mathrm{sec}$ at an angle of $45^{\circ}$. Answer the questions asked for in 6 , with the exception of (f). In regard to (f), find the angle of projection that will make the horizontal range a maximum and the value of this maximum range.

8. The maximum range of a projectile when fired on a level terrain is $1000 \mathrm{ft}$.

(a) What is its muzzle velocity?

(b) What is the maximum horizontal distance it can travel if the projectile is fired from a 40-ft-high platform? Hint. First find the firing angle for maximum horizontal range, see problem 6 .

9. The maximum distance a boy can throw a ball on level ground is $100 \mathrm{ft}$. Neglecting the height of the boy, find

(a) The velocity with which the ball leaves his hand,

(b) The maximum horizontal distance he can throw the ball if he stands on a roof which is $40 \mathrm{ft}$ above the ground. Hint. First find the throwing angle for maximum horizontal range, see problem 6 .

10. Two athletes, one $6 \frac{1}{2} \mathrm{ft}$ tall, the other $5 \frac{1}{2} \mathrm{ft}$ tall, can each put a shot with the same velocity of $36 \mathrm{ft} / \mathrm{sec}$. At what angle should the shot leave each athlete's hand in order to get the maximum horizontal range? Assume the shot leaves from heights of $6 \mathrm{ft}$ and $5 \mathrm{ft}$ respectively. How much farther will the taller athlete's throw go?
11. Answer the questions in problem 6 , excepting (f), if $\alpha=0$, i.e., if the projectile is fired horizontally from a distance $y_{0} \mathrm{ft}$ above the horizontal.
12. A projectile is fired with a velocity $v_{0}$ at an angle $\alpha$ with the horizontal. The terrain makes an angle $\beta$ with the horizontal.

(a) Find the range of the projectile. Hint. Call $R$ the range of the projectile. Then the projectile will hit the terrain when $x=R \cos \beta$ ard $y=$ $R \sin \beta$. Substitute in (h) of Example 34.3.

(b) Find the value of $\alpha$ which will make the range a maximum. Hint. Make use of double angle formulas.

(c) What is the maximum range?

13. In problem 3, we gave two angles at which a projectile could be fired in order to hit an object located within range and on the same level as the firing weapon.

(a) Solve this same problem if the object to be hit is on the top of a hill whose angle of inclination is $\beta$ and whose distance $D$ from the firing point is less than or equal to the range $v_{0}^{2} / g(1+\sin \beta)$, as given in (c) of problem 12. Hint. Call $(X, Y)$ the coordinates of the object. Then $D=\sqrt{X^{2}+Y^{2}}, \sin \beta=Y / D, \cos \beta=X / D$. Replace $R$ by $D$ in answer to $12(\mathrm{a})$. Solve for $\alpha$. Use the fact that

and

$$
\cos A \sin B=\frac{1}{2}[\sin (A+B)-\sin (A-B)]
$$

$$
\sin (2 \alpha-\beta)=\cos \left(2 \alpha-\beta-\frac{\pi}{2}\right)
$$

(b) Show, by means of the solution found in (a), that it is possible to hit an object only if its $X$ and $Y$ coordinates satisfy the inequality

$$
\sqrt{X^{2}+Y^{2}}+Y \leqq v_{0}^{2} / g
$$

, Hint. Use the fact-see answer to (a)-that

$$
\left(g X^{2}+Y_{v_{0}}^{2}\right) /\left(v_{0}{ }^{2} \sqrt{X^{2}+Y^{2}}\right)
$$

must be $\leqq 1$. Note that if $Y=0$, so that object is on a level terrain, $X \leqq v_{0}^{2} / g$ as we saw previously.

14. A man is hunting with a gun whose muzzle velocity is $224 \mathrm{ft} / \mathrm{sec}$. He aims for a bird on the top of a tree $150 \mathrm{ft}$ high and $1500 \mathrm{ft}$ away. Is the bird in danger of being hit?

In problems 1-14, we ignored air resistance. In the problems below, we shall assume the projectile is fired from the earth and is subject not only to a gravitational force but also to an air resistance which is proportional to the first power of the velocity. We shall also assume that the force of the air resistance acts in a direction opposite to that of the velocity, i.e., that it acts along a tangent to the projectile's path and in a direction to oppose the motion. Call $R$ the proportionality factor of the air resistance.

15. A projectile is fired on a level terrain at an angle $\alpha$ with the horizontal and with a velocity of $\nu_{0} \mathrm{ft} / \mathrm{sec}$.

(a) Find the parametric equations of the particle's path. Hint. Modify equation (a) of Example 34.3 to take into account the components of the force of the air resistance in the $x$ and $y$ directions.

(b) What is the maximum height reached by the particle? Hint. Set $\frac{d y}{d x}=\frac{d y / d t}{d x / d t}$ equal to zero.

16. A projectile is fired in a horizontal direction with a velocity of $v_{0} \mathrm{ft} / \mathrm{sec}$ from a height of $y_{0} \mathrm{ft}$. Find the parametric equations of its path.
17. An anti-aircraft gun fires a shell almost vertically with an initial velocity of $v_{0} \mathrm{ft} / \mathrm{sec}$. The horizontal component of the air resistance is therefore negligible. Assume the gun makes an angle $\alpha$ with the horizontal, and the vertical component of resistance is $R d y / d t$.

(a) Find the parametric equations of the path of the shell.

(b) Assume the shell weighs $60 \mathrm{lb}$, the muzzle velocity is $2000 \mathrm{ft} / \mathrm{sec}$, the angle of elevation is $80^{\circ}$, and the vertical component of the air resistance is $1 / 20 d y / d t$. Find the parametric equations of the shell's path, the maximum height attained by it, and the time required to reach this maximum height.

## 5. ANSWERS 34B

1. $y=x-x^{2} / 80,000,20,000 \mathrm{ft}, 80,000 \mathrm{ft}$.
2. $\alpha=\frac{\pi}{4} \pm \frac{1}{2} \operatorname{Arccos} \frac{D_{g}}{v_{0}^{2}}$.
3. (a) No. The maximum range is $3.7879 \mathrm{mi}$.

(b) $\left(\frac{\pi}{4} \pm 0.36\right) \mathrm{rad}$.

(c) $1693 \mathrm{ft}$ or $8307 \mathrm{ft}$, approx.

(d) $20.6 \mathrm{sec}$ or $45.6 \mathrm{sec}$, approx.

(e) Yes. When $x=4000 \mathrm{ft}$, the height $y$ of the projectile is approximately $6498 \mathrm{ft}$, if the larger angle of elevation is used.

5. (a) $\alpha=\operatorname{Arcsin}\left(\frac{2}{3}\right) . \quad$ (b) $y=64 \mathrm{ft} . \quad$ (c) $x=143 \mathrm{ft}$.
6. (a) $x=\left(v_{0} \cos \alpha\right) t, \quad y=y_{0}+\left(v_{0} \sin \alpha\right) t-\frac{g t^{2}}{2}$;

$y=y_{0}+(\tan \alpha) x-\frac{g \sec ^{2} \alpha}{2 v_{0}^{2}} x^{2}$. (b) Range is given by the positive value of $x$ for which

$$
\frac{g \sec ^{2} \alpha}{2 v_{0}^{2}} x^{2}-(\tan \alpha) x-y_{0}=0
$$

(c) $y=y_{0}+\frac{v_{0}^{2} \sin ^{2} \alpha}{2 g}$.

(d) $t=x /\left(v_{0} \cos \alpha\right)$, where $x$ is given by (b),

(e) $\tan \theta=\frac{v_{0} \sin \alpha-g t}{v_{0} \cos \alpha}$,

$|v|=\sqrt{v_{0}^{2}-2 v_{0}(\sin \alpha) g t+(g t)^{2}}$, where $t$ is given by $(d)$.

(f) $\sin \alpha=v_{0} / \sqrt{2\left(v_{0}^{2}+g y_{0}\right)}$.

$\begin{array}{lll}\text { 7. (a) } y=50+x-\frac{x^{2}}{128} & \text { (b) } x=166.4 \mathrm{ft} . & \text { (c) } 82 \mathrm{ft} \text {. }\end{array}$

$\begin{array}{ll}\text { (d) } t=3.7 \mathrm{sec} . & \text { (e) } 122^{\circ} 00^{\prime}, 85.6 \mathrm{ft} / \mathrm{sec} \text {. }\end{array}$

(f) $\sin \alpha=0.6, \alpha=37^{\circ}$, approx., $171 \mathrm{ft}$, approx.

8. (a) $v=80 \sqrt{5} \mathrm{ft} / \mathrm{sec}$. $\quad$ (b) $1039 \mathrm{ft}$, approx. ( $\alpha=44^{\circ}$, approx.).
9. (a) $40 \sqrt{2} \mathrm{ft} / \mathrm{sec} . \quad$ (b) $134 \mathrm{ft}\left(\alpha=37^{\circ}\right.$, approx.).
10. Use the answer given in $6(\mathrm{f})$ to find the angle $\alpha$ for each athlete. Then use the range equation as given in $6(\mathrm{~b})$.
11. (a) $x=v_{0} t, y=y_{0}-\frac{g t^{2}}{2}, y=y_{0}-\frac{g}{2 v_{0}^{2}} x^{2}$.

(b) $x=v_{0} \sqrt{2 y_{0} / g} \mathrm{ft}$.

(c) $y=y_{0} \mathrm{ft}$.

(d) $t=x / v_{0}$, where $x$ is given by (b).

(e) $\tan \theta=-g t / v_{0},|v|=\sqrt{v_{0}^{2}+(g t)^{2}}$.

12. (a) Range $R=\frac{2 v_{0}^{2} \cos \alpha \sin (\alpha-\beta)}{g \cos ^{2} \beta}$.

![](https://cdn.mathpix.com/cropped/2023_10_07_819562498d978d1c8215g-483.jpg?height=86&width=821&top_left_y=1171&top_left_x=135)

13. (a) $\alpha=\frac{\pi}{4}+\frac{\beta}{2} \pm \frac{1}{2} \operatorname{Arc} \cos \left(\frac{g X^{2}+Y_{v_{0}}{ }^{2}}{v_{0}{ }^{2} \sqrt{X^{2}+Y^{2}}}\right)$.
14. No. See $13(b)$.
15. (a) $x=\frac{m}{R} v_{0} \cos \alpha\left(1-e^{-R t / m}\right)$,

$$
y=\frac{m}{R}\left(\frac{m g}{R}+v_{0} \sin \alpha\right)\left(1-e^{-R t / m}\right)-\frac{m g}{R} t,
$$

where $R$ is the proportionality factor of the air resigtance.

(b) $t=\frac{m}{R} \log \left(1+\frac{R v_{0}}{m g} \sin \alpha\right)$,

$$
y=\frac{m v_{0}}{R} \sin \alpha-\frac{m^{2} g}{R^{2}} \log \left(1+\frac{R v_{0}}{m g} \sin \alpha\right) \text {. }
$$

16. $x=\frac{m v_{0}}{R}\left(1-e^{-R t / m}\right), y=y_{0}+\frac{m^{2} g}{R^{2}}\left(1-e^{-R t / m}\right)-\frac{m g}{R} t$.
17. (a) $x=\left(v_{0} \cos \alpha\right) t, y=$ as in $15(a)$.

(b) $x=347.3 t, y=118,861\left(1-e^{-0.0267 t}\right)-1200 t, 30,153 \mathrm{ft}, 36.4 \mathrm{sec}$.

