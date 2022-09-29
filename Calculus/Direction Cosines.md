---
aliases:
    - "direction numbers"
    - "direction cosines"
---
> [!summary]
> The direction cosines are defined, in 3 dimensions, as
> $$\cos \alpha = \frac{OA}{OP}=\frac x d,\hspace{1em}\cos \beta = \frac{OB}{OP}=\frac y d, \hspace{1em}\cos \gamma = \frac{OC}{OP}=\frac z d$$
> 
> $x$, $y$ and $z$ are called *direction numbers*. 
> 
> <span class='centerImg'> ![[direction cosines.svg]] </span>


# Properties
Using the pythagorean theorem we have this property that squaring and summing the direction cosines equal to 1
$$
\begin{align}
\cos^2 \alpha + \cos^2 \beta + \cos ^2 \gamma &= \frac {x^2+y^2+z^2}{d^2} \\
&= \frac {d^2}{d^2} \\
&= 1
\end{align}
$$
---------

Using the previous result
$$
\cos^2 \alpha + \cos^2 \beta + \cos ^2 \gamma = 1
$$

means that if we have $\alpha$ and $\beta$ then there are two possible lines witht the same $\alpha$ and $\beta$
$$
\cos \gamma = \pm\sqrt{1-\cos^2 \alpha - \cos^2 \beta  }
$$

Two different of values of $\gamma$ will give two different values that differ by the sign only. 
$$
\begin{align}
\cos \gamma_1 &= \sqrt{1-\cos^2 \alpha - \cos^2 \beta  } \\
\cos \gamma_2 &= -\sqrt{1-\cos^2 \alpha - \cos^2 \beta  } = -\cos \gamma_1
\end{align}
$$

By the [[Trigonometric identity#Supplementary Angles]] we know that these two values will be $\gamma_1$ and $\gamma_2=\pi - \gamma_1$ 

$$
\begin{align}
\cos (\gamma_2) = \cos \left(\pi - \gamma_1\right) = - \cos \gamma_1
\end{align}
$$

---------
Reworking 
$$\cos \alpha = \frac x d,\hspace{1em}\cos \beta = \frac y d, \hspace{1em}\cos \gamma = \frac z d$$
We can isolate $d$ and have this property

$$d = \frac x {\cos \alpha} = \frac y {\cos \beta}= \frac z {\cos \gamma}$$

---------

This is just a rewrite of what is above. With $d=\sqrt{x^2+y^2+z^2}$, then

$$\cos \alpha = \frac x {\sqrt{x^2+y^2+z^2}},\hspace{1em}\cos \beta =\frac y {\sqrt{x^2+y^2+z^2}}, \hspace{1em}\cos \gamma =\frac z {\sqrt{x^2+y^2+z^2}}$$

---------

The coordinates of $P$ are

$$
x=d\cos\alpha, \hspace{1em}y=d\cos\beta, \hspace{1em}z=d\cos\gamma
$$

If we ever have only the direction cosines and the length of the segment, then we can get the coordinates of $P$. 
