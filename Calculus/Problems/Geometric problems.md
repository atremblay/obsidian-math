
Find the family of functions such that $A$ ($\Delta PQR$)  is constant. 

![[Images/1.svg]]

No matter where we take $P(x,y)$, $A$ needs to be the same. So, this point is essentially a constant in this problem. What will vary is the intersection of the slope and the x-axis at $Q(a,0)$. 

Obviously, start with the area of the triangle
$$
\begin{align}
    A&=\frac {(x-a)y} 2 \\
    \iff a&= x-\frac{2A} y \\
    a&= \frac{xy - 2A} y \\
\end{align}
$$
$a$ is now a function of $x, y, A$, three independant variables. What we want is the
family of function $y$. We need a second equation that uses $a$ so that we can eliminate
it. It is after all just a by-product of the family of functions.

We have the slope
$$
\begin{align}
    y'&=\frac{y}{x-a} \\
    \iff y' &= \frac{y}{x-\frac{xy - 2A} y} \\
    y' &= \frac{y^2}{xy-xy - 2A} \\
    y' &= \frac{-y^2}{2A} \\
\end{align}
$$

We now have a differential equation, without $a$. We can use [[Separation of variables]] to solve this (as usual the
constant $C$ absorbs every other constants)

$$
\begin{align}
    \frac{y'}{y^2} &= \frac{-1}{2A} \\
    \frac{1}{y^2}\frac{d y}{d x} &= \frac{-1}{2A} \\
    \int \frac{1}{y^2}\frac{d y}{d x} dx &= \int \frac{-1}{2A} dx \\
    \int \frac{1}{y^2}dy &= \int \frac{-1}{2A} dx \\
    \frac{-1} y &= \frac{-x}{2A} + C  \\
    \frac{1} y &= \frac{x+2AC}{2A}  \\
    y &= \frac{2A}{x+2AC}  \\
\end{align}
$$

This would be our family of functions. A specific solution is obtained when we set a value for $C$.

We now have to test this family of functions against an arbitrary point $P(x_0,y_0)$
![[Images/1.1.svg]]

We use $Q(b,0)$ instead of $Q(a,0)$ to avoid confusion on the notation. This is not the same point as above. $A$ is
still the same constant as before. It's the central point of this whole problem.

So, using our family of functions we walk backwards by first finding the first derivative $y_0'$.

$$
\begin{align}
    y_0 &= \frac{2A}{x_0+2AC}  \\
    y_0'&= \frac{2A}{(x_0+2AC)^2}  \\
\end{align}
$$

$$
\begin{align}
     y_0' &= \frac{y_0-0}{x_0-b} \\
      &= \frac{y_0}{x_0-b} \\
    \iff b &= x_0 - \frac{y_0}{y_0'}
\end{align}
$$

Using this value of $b$, the area $A^*$ is
$$
\begin{align}
    A^*&=\frac {(x_0-b)y_0} 2 \\
    &=\frac {(x_0-x_0 + \frac{y_0}{y_0'})y_0} 2 \\
    &=\frac {y_0^2}{2y_0'} \\
    &=\frac 1 2 \frac {\left(\frac{2A}{x_0+2AC}\right) ^2}{\frac{2A}{(x_0+2AC)^2}} \\
    &=\frac 1 2 \frac {\frac{(2A)^2}{\cancel{(x_0+2AC)^2}}}{\frac{2A}{\cancel{(x_0+2AC)^2}}} \\
    &=\frac 1 2 \frac{(2A)^\cancel{2}}{\cancel{2A}} \\
    &= A
\end{align}
$$

For a generic point $P(x_0, y_0)$, the area $A^*$ is always equal to $A$, the initial area.

# Wrong direction
Very important here. We could very easily go down the wrong path

$$
\begin{align}
    y'&=\frac{y}{x-a} \\
    \int \frac 1 y \frac{d y}{d x} &= \frac 1 {x-a} \\
    \log y &= \log (x-a) + C \\
    y &= e^{\log (x-a) + C} \\
    y &= (x-a)e^C \\
    y &= (x-a)C \\
\end{align}
$$

and think that this is the family of functions we are looking for. The reason why this is wrong is that $a$ is a function of $x$ and $y$. We could, though, remplace by its actual function
$$
\begin{align}
    A&=\frac {(x-a)y} 2 \\
    \iff a&= x- \frac{2A} y
\end{align}
$$

And replace it above
$$
\begin{align}
    y'&=\frac{y}{x-x- \frac{2A} y } \\
    y'&=\frac{-y^2}{2A} \\
    \frac{y'}{y^2}&=\frac{-1}{2A} \\
\end{align}
$$
And proceed with the [[Separation of variables]] like above.
