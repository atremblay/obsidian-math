
Quantities that have both magnitude and direction, such as force, velocity, acceleration, are called vector

> [!blank-container|right-medium]
> ![[../Images/inclined_motion_1.svg]]

Figure 16.5 quantities. It is the usual custom to represent a vector quantity by a line with an arrowhead at one end. The magnitude of the vector is given by the length of the line and its direction by the inclination of the line. In Fig. 16.5, $P Q$ represents a vector quantity. With $P Q$ as hypotenuse, we construct a right triangle whose sides are parallel to the $x$ and $y$ axes. Then the vectors $\mathbf{P R}$ and $\mathbf{R Q}$ are called respectively the $\boldsymbol{x}$ component and the $\boldsymbol{y}$ component of the vector PQ. Their respective magnitudes are given by

$$
|\mathbf{P R}|=|P Q \cos \theta|, \quad|\mathbf{R Q}|=|P Q \sin \theta|,
$$

and their respective directions by the directions of the arrowheads.

Comment 16.511. A vector formerly was written with an arrow over it to distinguish it from a line segment. The current practice, and the one we shall adopt, is to use bold face type.

A vector may also be broken up into two or more components in any directions. In Fig. 16.52, we have broken up the vector $\mathbf{P Q}$ into four component vectors $\mathbf{P R}, \mathbf{R S}, \mathbf{S T}, \mathbf{T Q}$. Note that each vector begins where the other leaves off and that the final vector's arrowhead touches the original vector's arrowhead.

If a body moves along a line which is inclined to the horizontal, the effective force causing it to move downhill is that component of the gravitational force acting in a direction parallel to the motion (I.e. parallel to $\mathbf{PQ}$). The forces opposing the motion are the frictional force and the wind resistance. Here the frictional force is equal to the product of the coefficient of friction $\mu$ and the component of the gravitational force acting in a direction perpendicular to the motion.

##### Example
#example #gravitational_force
###### Statement
> A toboggan with two people on it weighs 520 pounds. It moves down a slope whose gradient is $5 / 12$. If the coefficient of sliding friction is $1 / 50$, and the force of the wind resistance is 5 times the velocity, find the time it will take the toboggan to reach the bottom of a $650 \mathrm{-ft}$ long incline. What would the terminal velocity be if the slope were of infinite length?

Let $\alpha$ be the angle of incline of the slide. Since its gradient $=5 / 12, \tan \alpha=5 / 12$. Hence (a) $\sin \alpha=5 / 13$ and $\cos$ $\alpha=12 / 13$. The gravitational force, which is the combined weight of sled

> [!multi-column]
> 
> >[!blank-container]
> > ![[../Images/inclined_motion_2.svg]]
> > (a)
> 
> >[!blank-container]
> > ![[../Images/inclined_motion_3.svg]]
> > (b)


and two people, equals 520 pounds. Therefore the magnitude of the component of the gravitational force in the direction of the incline (b) is $520 \sin \alpha=520(5 / 13)=200$ pounds. The magnitude of the component of the gravitational force perpendicular to the incline is $520 \cos \alpha=520(12 / 13)=480$ pounds. Since the coefficient of sliding friction is $1 / 50$, the sliding frictional force is $480(1 / 50)=9.6$ pounds. The mass of the body is $520 / 32$. Hence the differential equation of motion is (remember mass $\times$ acceleration = net force acting on the body)

$$\frac{520}{32} \frac{d v}{d t}=200-9.6-5 v=190.4-5 v, \quad \frac{d v}{d t}+\frac{4}{13} v=11.7 \tag{I}$$

Its solution by the method of Lesson $11 B$ is

$$
v=38+c_{1} e^{-4 t / 13} .
$$

The initial conditions are $t=0, v=0$. Hence $c_{1}=-38$, and (b) becomes

$$
v=38\left(1-e^{-4 t / 13}\right) \text {. }
$$

Integration of (c) gives

$$
s=38\left(t+\frac{13}{4} e^{-4 t / 13}\right)+c_{2} .
$$

If we take the origin at the starting point of the toboggan, then $t=0$, $s=0$. Therefore by $(\mathrm{d}), c_{2}=-123.5$. Hence (d) becomes

$$
s=38\left(t+\frac{13}{4} e^{-4 t / 13}\right)-123.5
$$

When $s=650$, we obtain from (e)

$$
20.4=t+\frac{13}{4} e^{-4 t / 13}
$$

whose solution is $t=20.4$ seconds approximately. This is the time it will take the toboggan to reach the bottom of the incline.

From (b), the terminal velocity is $38 \mathrm{ft} / \mathrm{sec}$ (let $t \rightarrow \infty$ ).

## EXERCISE 16C

In the problems below, take the origin at the start of the motion and the positive direction in the initial direction of motion.

1. A toboggan with four boys on it weighs $400 \mathrm{lb}$. It slides down a slope with a $30^{\circ}$ incline. Find the position and velocity of the toboggan as functions of time if it starts from rest and the coefficient of sliding friction is 30 . If the slope is $123.2 \mathrm{ft}$ long, what is the velocity of the toboggan when it reaches the bottom? Neglect air resistance.
2. A body weighing $400 \mathrm{lb}$ is shot up a $30^{\circ}$ incline with an initial velocity of $50 \mathrm{ft} / \mathrm{sec}$. The coefficient of sliding friction is $\frac{1}{10}$. (a) Find the position and velocity of the body as functions of time. (b) How long and how far will the body move before coming to rest? Neglect air resistance.
3. Two particles start from rest and from the same point on the circumference of a circle in a vertical plane. One moves along a vertical diameter; the other along a chord of the circle (any chord will do). If the only force acting on the particles is that of gravity, prove that both particles reach the circumference of the circle at the same time. 4. A body weighing $W \mathrm{lb}$ slides down a slope with an $\alpha^{\circ}$ incline. Its initial velocity is $v_{0} \mathrm{ft} / \mathrm{sec}$. The coefficient of sliding friction is $r$. Ignore air resistance.

(a) Express the velocity and the distance of the body as functions of time.

(b) Express its velocity as a function of distance.

(c) Express the time as a funetion of the distance.

Use these formulas to verify the accuracy of your answers to 1 .

5. A body weighing $W \mathrm{lb}$ is shot up a slope with an $\alpha^{\circ}$ incline. Its initial velocity is $v_{0} \mathrm{ft} / \mathrm{sec}$. The coefficient of sliding friction is $r$. Ignore air resistance.

(a) Find the position and velocity of the body as functions of time.

(b) How long and how far will the body move before coming to rest?

Use these formulas to verify the accuracy of your answers to 2 .

6. A body weighing $W \mathrm{lb}$, starting from rest, slides down a slope with an $\alpha^{\circ}$ incline. The coefficient of sliding friction is $r$ and the force of the air resistance is $k v \mathrm{lb}$.

(a) Express the velocity and the distance of the body as functions of time.

(b) Express its distance as a function of the velocity.

(c) What is its terminal velocity?

7. A boy and his sled weigh $96 \mathrm{lb}$. Starting from rest, he slides down an incline whose gradient is 3 . The coefficient of sliding friction is $\frac{1}{24}$, and the force of the air resistance is twice the velocity.
(a) Find the velocity and the distance of the sled as functions of time.
(b) What is his terminal velocity?
(c) When will he reach the bottom of the slope if it is $272 \mathrm{ft}$ long?
(d) What is his velocity at the bottom?

Solve independently. Use the formulas found in problem 6 to verify the accuracy of your answers.

8. A body weighing $W \mathrm{lb}$ is shot up an $\alpha^{\circ}$ incline with a velocity of $v_{0} \mathrm{ft} / \mathrm{sec}$. The coefficient of sliding friction is $r$ and the force of the air resistance is $k v$ lb.
(a) Express the velocity and the distance of the body as functions of time.
(b) How long and how far will it go?
9. A body weighing $64 \mathrm{lb}$ is shot up an incline, whose gradient is 3 , with a velocity of $96 \mathrm{ft} / \mathrm{sec}$. The coefficient of sliding friction is $\frac{1}{4}$ and the force of the air resistance is $10^{*} \mathrm{lb}$.

(a) Find the velocity and the distance of the body as functions of time.

(b) How long and how far will it go?

Solve independently. Use the formulas found in problem 8 to verify the accuracy of your answers.

10. A toboggan with two people on it weighs $300 \mathrm{lb}$. It starts from rest down a slope, 1 mile long, from a height $200 \mathrm{ft}$ above a horizontal level. The coefficient of sliding friction is $x$ proportional to the square of the velocity. When the velocity is $30 \mathrm{ft} / \mathrm{sec}$, this force is $6 \mathrm{lb}$.

(a) Find the velocity of the toboggan as a function of the distance and of the time.

(b) With what velocity will the toboggan reach the bottom of the slide?

(c) When will it reach the bottom?

(d) What would its terminal velocity be if the slide were infinite in length?

## ANSWERS 16C

1. $v=15.4 t, s=7.7 t^{2}, v=61.7 \mathrm{ft} / \mathrm{sec}$.
2. (a) $v=50-18.77 t, s=50 t-9.39 t^{2}$.

(b) $t=2.7 \mathrm{sec}, \mathrm{s}=66.6 \mathrm{ft}$.

4. (a) $v=v_{0}+g(\sin \alpha-r \cos \alpha) t ; s=v_{0} t+\frac{g}{2}(\sin \alpha-r \cos \alpha) t^{2}$.

(b) $v^{2}=v_{0}^{2}+2 g(\sin \alpha-r \cos \alpha) s$.

(c) $t=\frac{\sqrt{v_{0}^{2}+2 g s(\sin \alpha-r \cos \alpha)}-v_{0}}{g(\sin \alpha-r \cos \alpha)}$.

5. (a) $v=v_{0}-g(\sin \alpha+r \cos \alpha) t, s=v_{0} t-\frac{g}{2}(\sin \alpha+r \cos \alpha) t^{2}$.

(b) $t=v_{0} / g(\sin \alpha+r \cos \alpha) \sec , s=v_{0}^{2} / 2 g(\sin \alpha+r \cos \alpha)$.

6. (a) $v=\frac{W}{k}(\sin \alpha-r \cos \alpha)\left(1-e^{-k t / m}\right)$,

$$
s=\frac{W}{k}(\sin \alpha-r \cos \alpha)\left(t+\frac{m}{k} e^{-k t / m}-\frac{m}{k}\right)
$$

(b) $s=\frac{m}{k}\left(-v-\frac{A}{k} \log \frac{A-k v}{A}\right)$, where $A=m g(\sin \alpha-r \cos \alpha)$,

(c) $\mathrm{T} . \mathrm{V} .=\frac{W}{k}(\sin \alpha-r \cos \alpha)$.

7. (a) $v=27.2\left(1-e^{-2 t / 3}\right), s=27.2\left(t+\frac{3}{2} e^{-2 t / 3}-\frac{3}{2}\right)$.

(b) T.V. $=27.2 \mathrm{ft} / \mathrm{sec}$.

(c) $11.5 \mathrm{sec}$.

(d) $27.2 \mathrm{ft} / \mathrm{sec}$.

8. (a) $v=\frac{W(\sin \alpha+r \cos \alpha)}{k}\left(e^{-k t / m}-1\right)+v_{0} e^{-k t / m}$,

$$
s=\frac{W(\sin \alpha+r \cos \alpha)}{k}\left[\frac{m}{k}\left(1-e^{-k t m}\right)-t\right]+\frac{m v_{0}}{k}\left(1-e^{-k t m}\right) .
$$

(b) $t=\frac{m}{k} \log \left(1+\frac{k v_{0}}{W(\sin \alpha+r \cos \alpha)}\right)$ sec,

$$
s=\frac{m v_{0}}{k}-\frac{m^{2} g(\sin \alpha+r \cos \alpha)}{k^{2}} \log \left(1+\frac{k v_{0}}{W(\sin \alpha+r \cos \alpha)}\right) \text { ft. }
$$

9. In the formulas: $W=64, r=4, k=\frac{1}{16}, \sin \alpha=\frac{3}{5}, \cos \alpha=\frac{4}{5}$, $v_{0}=96, m=64 / 32=2$.
10. (a) $v=74.1 \frac{e^{0.105 t}-1}{e^{0.105 t}+1}, v^{2}=5484\left(1-e^{-0.00148}\right)$.

(b) $68 \mathrm{ft} / \mathrm{sec}$.

(c) 30 sec, approx.

(d) $74.1 \mathrm{ft} / \mathrm{sec}$.
