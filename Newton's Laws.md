
# Second law


> [!Summary]+
> $$\boxed{F=ma}=m\frac{\mathrm{d}v}{\mathrm{d}t}=mv\frac{\mathrm{d}v}{\mathrm{d}s}$$
> - $F$ is the sum of all forces, 
> - $m$ is the mass of a body and 
> - $a$ is the acceleration of said body.
> 
> $F$, $a$, and $v$ are vector quantities, i.e., they not only have magnitude but also direction.  _Hence it is always essential in a problem to indicate the positive direction._

The rate of change (derivative) of the momentum of a body (momentum = mass X velocity) is proportional ($k$) to the resultant external force $F$ acting upon it. In mathematical symbols, the second law says
$$F=k\frac{\mathrm{d}}{\mathrm{d}t}\text{momentum}=k\frac{\mathrm{d} mv}{\mathrm{d}t}=km\frac{dv}{dt} = kma $$
where $m$ is the mass of the body, $v$ its velocity, and $k > 0$ is a proportionality constant whose value depends on the units used. If these are foot for distance, pound for force, slug for mass$\left(\displaystyle =\frac{1}{32}\text{ pound}\right)$, second for time, then $k = 1$ and (a) becomes
$$F=m\frac{dv}{dt}=ma=m\frac{d^2s}{dt^2} $$


> [!question]+ 
> No idea why the rate of change of the momentum is proportional to the resultant external force. Would be nice to understand where it comes from. Regardless, it's even less clear how $k=1$ given all the conditions mentionned above. It's probably always the case since the standard formulation of Newton's second law is $$F=ma$$


where $a$ is the rate of change in velocity, commonly called the acceleration of the particle, and $s$ is the distance the particle has moved from a fixed point.

Remember that $F$, $a$, and $v$ are vector quantities, i.e., they not only have magnitude but also direction.  _Hence it is always essential in a problem to indicate the positive direction._

By the [[Calculus/Chain Rule|chain rule]]
$$
\begin{align}
a = \frac{\mathrm{d}v}{\mathrm{d}t} &= \frac{\mathrm{d}v}{\mathrm{d}s}\frac{\mathrm{d}s}{\mathrm{d}t} \\
&= v \frac{\mathrm{d}v}{\mathrm{d}s}
\end{align}
$$

> [!Question]+ 
> Is $F$ a function of time? Nothing seems to indicate that the acceleration needs to be a constant. $\frac{d^2x}{dt^2}$ could be anything other than a constant. So is correct to say that $$F(t) = ma(t)$$
> Even the mass can change over time (e.g. mass of a rocket gets lower when burning fuel)

# Law of attraction

If $m_1$ and $m_2$ are the masses of two bodies whose centers of gravity are $r$ distance apart, the force of attraction between them is given by 
$$F=k\frac{m_1m_2}{r^2}$$
where $k>0$ is a proportionality constant.
