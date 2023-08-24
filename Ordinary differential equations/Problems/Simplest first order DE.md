
$$
\frac{dx}{dt} = \dot{x} = kx
$$

Using [[../Solutions/Separation of variables]]

$$
\begin{align}
&\int \frac{1}{x} \frac{dx}{dt}dt = \int \frac{1}{x} dt = \int kdt \\
\iff & log |x| = kt + C_1 \\
\iff & x = e^{kt + C_1} = e^{C_1}e^{kt} = C e^{kt}
\end{align}
$$

A constant is just a constant. We have to keep track of it, but we don't need to know it exactly unless we are looking for a specific solution.

So **a** family of solution is $x = Ce^{kt}$. Not to be confused with [[../../Calculus/General solution|general solution]].

