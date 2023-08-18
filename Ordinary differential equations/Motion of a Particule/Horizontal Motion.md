

Horizontal motion is typically subjected to [[../Definitions/Frictional Force|Frictional Force]]. In addition to the frictional force, a body also may be subject to a resisting force due to the air or other medium in which it moves.


##### Example 
#example #seperation_of_variable 
###### Statement
> An object on a sled is pulled by a force of 10 pounds across a frozen pond. Object and sled weigh 64 pounds. The coefficients of static and sliding friction are negligible. However, the force of the air resistance is twice the velocity of the sled. If the sled starts from rest, find its velocity at the end of 5 seconds and the distance it has traveled in that time. What is its terminal velocity?

Here $m=64 / 32=2$ and the force of the air resistance is given as $2 v$ pounds. Hence the differential equation of motion of the sled is (remember mass $\times$ acceleration of a body = net force acting upon it)

$$
2 \frac{d v}{d t}=10-2 v, \quad \frac{d v}{d t}+v=5 \tag{I}
$$

Its solution is

$$
v=5+c_{1} e^{-t} \tag{II}
$$

The initial condition is $t=0, v=0$. Hence $c_{1}=-5$, and ($II$) becomes

$$
v=5\left(1-e^{-t}\right) \text {. } \tag{III}
$$

When $t=5$,

$$v=5\left(1-e^{-5}\right)=5(1-0.0067)=5(0.9933)=4.97 \mathrm{ft} / \mathrm{sec} \tag{IV}$$.

By ($III$) the terminal velocity is found to be $5 \mathrm{ft} / \mathrm{sec}$.

To find the distance $x$ traveled in time $t$, we integrate ($III$) to obtain

$$
x=5\left(t+e^{-t}\right)+c_{2} \tag{V}
$$

When $t=0$ and $x=0$ (taking the origin at the starting position), we find, from ($V$), $c_{2}=-5$. Hence ($V$) becomes

$$
x=5\left(t+e^{-t}-1\right) \text {. } \tag{VI}
$$

And when $t=5$,

$$
x=5\left(5+e^{-5}-1\right)=5(4+0.0067)=20 \mathrm{ft} \text { approx. } \tag{VII}
$$


##### Example 
#example 
###### Statement
> A boy weighing 75 pounds runs for a slide and reaches it at a velocity of $10 \mathrm{ft} / \mathrm{sec}$. If the coefficient of sliding friction $\mu$ between his shoes and the ice is $1 / 25$, how far will he slide? Ignore wind resistance.

By Definition 16.4, the frictional force is $1 / 25 \cdot 75=3$ pounds. Since this is the only force which is opposing the motion and the mass of the boy is $75 / 32$, the differential equation of motion becomes

$$\frac{75}{32} \frac{d v}{d t}=-3, \quad \frac{d v}{d t}=-\frac{32}{25} \tag{I}$$

By (16.11), we can write (a) as

$$
v \frac{d v}{d x}=-\frac{32}{25}, \quad v d v=-\frac{32}{25} d x \tag{II}
$$

where $x$ is the distance measured from the beginning of the slide. Integration of ($II$) and insertion of the initial and final conditions results in

$$
\int_{v=10}^{0} v d v=-\frac{32}{25} \int_{x=0}^{x} d x \tag{III}
$$

Its solution is

$$
50=\frac{32}{25} x, \quad x=39 \mathrm{ft} \text { approx. } \tag{IV}
$$

##### Example 
#example 
###### Statement
> Solve the previous problem if a wind is blowing against the boy with a force equal to his velocity.

The differential equation (a) above must now be modified to read [with $d v / d t$ replaced by its equal $v(d v / d x)$, - see (16.11)],

$$
\begin{gather}
\frac{75}{32} v \frac{d v}{d x}=-3-v, \quad \frac{v d v}{v+3}=-\frac{32}{75} d x \tag{I}\\
\int_{v=10}^{0}\left(1-\frac{3}{v+3}\right) d v=-\frac{32}{75} \int_{x=0}^{x} d x
\end{gather}
$$

The solution of ($I$) is

$$-3 \log 3-10+3 \log 13=-\frac{32}{75} x, x=13.1\text{ ft approx.} \tag{II}$$ 


##### Example 
#example 
###### Statement
> A boat is being towed at the rate of $18 \mathrm{ft}$ sec. At the instant when the towing line is cast off, a man takes up the oars and begins to row with a force of 20 pounds in the direction of the moving boat. If man and boat together weigh 480 pounds, and the resistance is equal to $\frac{7}{4} v$ pounds, find the speed of the boat at the end of 30 seconds.

The net force acting on the boat at the instant $t=0$ when the towing line is cast off is the man's force of 20 pounds, less the resisting force of $\frac{7}{4} v$ pounds. The mass of man and boat is $480 / 32=15$. Hence the differential equation of motion is


$$
15 \frac{d v}{d t}=20-\frac{7}{4} v, \quad \frac{d v}{d t}+\frac{7}{60} v=\frac{4}{3} \tag{I}
$$

Its solution by Lesson $11 \mathrm{~B}$ is

$$
v=c e^{-7 t / 60}+\frac{80}{7} \tag{II}
$$

Substituting in ($II$) the initial conditions $t=0, v=18$, we find $c=46 / 7$. Hence ($II$) becomes

$$
v=\frac{46}{7} e^{-7 t / 60}+\frac{80}{7} \tag{III}
$$

When $t=30$,

$$
v=\frac{46}{7} e^{-7 / 2}+\frac{80}{7}=11.6 \mathrm{ft} / \mathrm{sec} \tag{IV}
$$


##### Example 
#example 
###### Statement
> A boy pulls a sled, on which a parcel has been placed, with a constant force of $F \mathrm{lb}$. The sled and parcel together weigh $\mathrm{mg} \mathrm{lb}$. The frictional force of the ice on the runners is negligible. However, the force of the air resistance is $k$ times the velocity of the sled. If the sled starts from rest, find:



###### (a)
> Its velocity as a function of time.


###### (b)
> Its distance as.a function of time.

###### (c)
> Its distance as a function of velocity.

###### (d)
> Its terminal velocity.

##### Example 
#example 

###### Statement
> In problem 1 , assume sled and parcel weigh $96 \mathrm{lb}$, that air resistance is $\frac{1}{9}$ times the velocity and that the boy is moving the sled at a constant rate of $4.5 \mathrm{ft} / \mathrm{sec}$ ) (i.e., the terminal velocity of the sled is $4.5 \mathrm{ft} / \mathrm{sec}$ ).



###### (a)
> Find the constant force which the boy is applying to the sled.




###### (b)
> Find the velocity and distance equations as functions of time.




###### (c)
> How far has the sled moved in $5 \mathrm{~min}$ ?




Solve independently and then check your answers with formulas found in problem 1.

##### Example 
#example 
###### Statement
> A boy weighing $m g \mathrm{lb}$ runs for a slide and reaches it with a velocity of $v_{0} \mathrm{ft} / \mathrm{sec}$. The coefficient of sliding friction between his shoes and the ice is $r$. (a) Find his velocity as a function of distance. Ignore air resistance. (b) How far will he slide?

##### Example 
#example 
###### Statement
> A boy weighing $80 \mathrm{lb}$ runs for a slide and reaches it with a velocity of $12 \mathrm{ft} / \mathrm{sec}$. The coefficient of sliding friction between his shoes and the ice is $1 / 20$ and the wind blows against him with a force equal to twice his velocity. (a) Find his distance as a function of his velocity. (b) How far will he slide?

##### Example 
#example 
###### Statement
> A 32,000-ton ship starting from rest begins to move because of the actions of its propellers that exert a forward force of $120,000 \mathrm{lb}$. The force of the water resistance is $5000 v$. Find the velocity of the ship as a function of time and its terminal velocity.


##### Example 
#example 
###### Statement
> The brakes are applied to a car, traveling on a slippery road, when it has slowed down to a speed of $6 \mathrm{mi} / \mathrm{hr}=8.8 \mathrm{ft} / \mathrm{sec}$. It slides $80 \mathrm{ft}$ before coming to a stop. Compute the coefficient of sliding friction between tires and street. Neglect the force of air resistance.

##### Example 
#example 
###### Statement
>  A body weighing $50 \mathrm{lb}$ rests on a table whose coefficient of sliding friction is $1 / 25$. The body is attached by a string to a weight of $14 \mathrm{lb}$ that hangs vertically over the table. At the moment the system is released, the 50 -1b body is $15 \mathrm{ft}$ from the edge of the table. Assume no other forces are operating. When and with what velocity does the body leave the table? Hint. The total mass of the system is $64 / 32$.

##### Example 
#example 
###### Statement
> The forward thrust of an airplane due to its propellers is $F$ lb. The air resistance is $k v^{2}$. Find the terminal velocity of the plane.

##### Example 
#example 
###### Statement
> An 8-lb body starting from rest is being pulled along a surface, whose coefficient of sliding friction is 1, by a force that is equal to twice the distance of the body from its starting point $x=0$. Air resistance is $v^{2} / 8$. Find the velocity of the body as a function of its distance. Hint. The resulting differential equation is linear in $v^{2}$. 10. A man and his boat weigh $320 \mathrm{lb}$. The man exerts a force of $16 \mathrm{lb}$ on the oars. The resistance of the water is twice the speed. Find:

###### (a)
> The velocity of the boat as a function of time.



###### (b)
> Its speed after 5 sec.


###### (c)
> Its terminal velocity.

##### Example 
#example 
###### Statement
> A man and his boat weigh $400 \mathrm{lb}$. At the moment the man picks up his oars to row, the boat is moving at the rate of $22 \mathrm{ft} / \mathrm{sec}$. If the resistance of the water is $2 v \mathrm{lb}$ and the oars exert a constant force of $15 \mathrm{lb}$, find the velocity of the boat as a function of time; also find the terminal velocity of the boat.



## ANSWERS 16B

1. (a) $v=\frac{F}{k}\left(1-e^{-k t / m}\right)$.

(b) $x=\frac{F}{k}\left(t+\frac{m e^{-k t / m}}{k}-\frac{m}{k}\right)$.

(c) $x=\frac{m}{k}\left(-v-\frac{F}{k} \log \frac{F-k v}{F}\right)$.

(d) T.V. $=F / k \mathrm{ft} / \mathrm{sec}$.

2. (a) $\frac{1}{2} l b$.

(b) $v=\frac{g}{2}\left(1-e^{-t / 27}\right), x=\frac{\theta}{2}\left(t+27 e^{-t / 27}-27\right)$.

(c) $1228.5 \mathrm{ft}$.

3. (a) $v^{2}=v_{0}^{2}-2 r g x \quad$ (b) $x=v_{0}^{2} / 2 r g$.
4. (a) $v-2 \log \frac{2+v}{14}=12-\frac{3}{3} x$. (b) 10.1 .
5. $v=24\left(1-e^{-t / 400}\right), \mathrm{T} . \mathrm{V} .=24 \mathrm{ft} / \mathrm{sec}$.
6. 0.015 .
7. $\mathrm{T} . \mathrm{V} .=\sqrt{F / k} \mathrm{ft} / \mathrm{sec}$.
8. $t=2.2 \mathrm{sec}, v=13.4 \mathrm{ft} / \mathrm{sec}$.
9. $v^{2}=16\left(x-2+2 e^{-x}\right)$.
10. (a) $v=8\left(1-e^{-0.2 t}\right)$.
(b) $v=5.1 \mathrm{ft} / \mathrm{sec}$.
(c) T.V. $=8 \mathrm{ft} / \mathrm{sec}$.
11. $v=\frac{1}{2}\left(15+29 e^{-4 t / 25}\right) ;$ T.V. $=15 / 2 \mathrm{ft} / \mathrm{sec}$.

