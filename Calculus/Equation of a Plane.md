---
aliases:
  - equation of a plane
---
  





$$
D = Ax + By+ Cz
$$

The point $(x, y, z)$ is an arbitrary point on the plane. All points that can satisfy this equation are part of the plane.

The perpendicular distance from the origin to the plane is $\sqrt{A^2+B^2+C^2}$. That is also the closest distance from the origin.



## Method 1: Normal to the plane

Any plane has a normal vector $N=\langle a,b,c \rangle$, with length $l=\sqrt{a^2+b^2+c^2}$,  which is perpendicular to it. The normal can be positioned anywhere, the important point is the size of the vector.


> [!blank|right-medium]
> ![[Images/plane_equation2.svg]]


If we have a known point $P(x_0, y_0, z_0)$ and any other point $R(x, y, z)$, both on the plane, their difference will also lie on the plane and it will be perpendicular to the normal vector. That line is defined by $R-P= \langle x,y,z \rangle - \langle x_0,y_0,z_0 \rangle =\langle x-x_0,y-y_0,z-z_0 \rangle$ 

The dot product between $N$ and $P-R$ is
$$
N \cdot (R-P) = \langle a,b,c \rangle \langle x-x_0,y-y_0,z-z_0 \rangle = a(x-x_0) + b(y-y_0)+c(z-z_0) = 0
$$

Or often written
$$
\begin{align}
a(x-x_0) + b(y-y_0)+c(z-z_0)&=0  \\
\iff  ax + by + cz &= d
\end{align}
$$

where $d=ax_0+by_0+cz_0$ 

- So the normal to the plane is $\langle a,b,c \rangle$ 
- If $d=0$, then we have a plane going through the origin 
- Think linear regression $w^Tx$ where $w$ is the normal to a plane 
- Changing the length of the normal vector will change $d$, which will move the plane up and down


## Method 2: [[Direction Cosines#Angle between two lines|Angle between two lines]]

- $P$ is an arbitrary point on the plane with coordinate $x,y,z$
- $Q$ is a point on the plane with coordinate $a,b,c$
- With $OQ$ being perpendicular to the plane, $OQ \perp QP, \forall P \in \text{plane}$
- [[Direction Cosines]] of $OQ$ are $\cos\alpha$,  $\cos\beta$ and $\cos\gamma$
- [[Direction Cosines]] of $OP$ are $\cos\alpha'$,  $\cos\beta'$ and $\cos\gamma'$
- The angle between $OQ$ and $OP$ is $\theta$
- By the [[Direction Cosines#Angle between two lines|angles between two lines]], $\cos \theta = \cos \alpha \cos\alpha' + \cos\beta\cos\beta'+\cos\gamma\cos\gamma'$ 

> [!blank|right-medium]
> ![[Images/plane_equation.svg]]

Because the triangle $OQP$ is a right triangle
$$
\cos \theta = \frac{OQ}{OP}
$$

Moreover
$$
\cos\alpha = \frac{a}{OQ}  \hspace{1.5em}\cos\beta = \frac{b}{OQ} \hspace{1.5em}\cos\gamma = \frac{c}{OQ}
$$
$$
\cos\alpha' = \frac{x}{OP}  \hspace{1.5em}\cos\beta' = \frac{y}{OP} \hspace{1.5em}\cos\gamma' = \frac{z}{OP}
$$

Use these values in
$$
\begin{align}
\cos \theta &= \cos \alpha \cos\alpha' + \cos\beta\cos\beta'+\cos\gamma\cos\gamma' \\
\frac{OQ}{OP} &= \frac{x}{OP} \cos \alpha + \frac{y}{OP}\cos\beta + \frac{z}{OP}\cos\gamma \\
OQ &= x\cos \alpha  + y\cos\beta+z\cos\gamma \\
OQ &= x \frac{a}{OQ}  + y\frac{b}{OQ} + z\frac{c}{OQ} \\
(OQ)^2 &= ax  + by + cz \\
&\boxed{D = ax  + by + cz} 
\end{align}
$$

where $(OQ)^2=D$. Important, if $Q$ is the origin this all breaks down because $OQ=0$ and we can't divide by zero. Refer to [[#Method 1 Normal to the plane]]

The point $(x, y, z)$ was an arbitrary point on the plane. So this is the equation of a plane; all points that can satisfy this equation are part of the plane.

> $(OQ)^2 = D=A^2+B^2+C^2$. Don't get confused by this if you compare to [[#Method 1 Normal to the plane]]. Here $D$ is the distance from the origin to the point $Q$. If the plane goes through the origin then $D=0$ and $A,B,C$ must also be zero. It does not mean that the normal to the plane does not exists. $Q$ would be at the origin, but there is still a normal. That line going from the  origin through $Q$ is the normal to the plane.


> $A$, $B$ and $C$ are not values between -1 and 1, but if we divide them by $OQ =\sqrt{D}$ then the range will be between -1 and 1 as we would get back $\cos\alpha,\cos\beta,\cos\gamma$.


> The perpendicular distance from the origin to the plane is $\sqrt{A^2+B^2+C^2}$