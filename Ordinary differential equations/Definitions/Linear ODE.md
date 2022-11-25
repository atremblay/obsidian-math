An ODE is linear if

$$
\sum_{i=0}^{n} f_{(n-i)}(x)y^{(n-i)} = Q(x)
$$
Sum of linear terms $y^{(k)}$, where $k$ is the $k^{th}$ derivative. It is of order $n$ if $f_n \not\equiv 0$ on interval I.

#### Nonhomogeneous
If $Q(x)\not\equiv0$ then 
$$
\sum_{i=0}^{n} f_{(n-i)}(x)y^{(n-i)} = Q(x)
$$
is non-homogeneous

#### Homogeneous
If $Q(x)\equiv0$ then 
$$
\sum_{i=0}^{n} f_{(n-i)}(x)y^{(n-i)} = 0
$$
is called homogeneous


>Check how that fits with [[Homogeneous Function]]

#### Principle of Superposition

If we have $n$ (non) homogeneous equations
$$
f_n(x)y^{(n)} + \ldots + f_1(x)y^{(1)} + f_0(x)y = Q_i(x) 
$$

Where $i = 1 \ldots n$ and we have the particular solutions $y_{p_1}$, $y_{p_2}$, ..., $y_{p_n}$
$$
\begin{align}
f_n(x)y_{p_1}^{(n)} + \ldots + f_1(x)y_{p_1}^{(1)} + f_0(x)y_{p_1} &= Q_1(x) \\
f_n(x)y_{p_2}^{(n)} + \ldots + f_1(x)y_{p_2}^{(1)} + f_0(x)y_{p_2} &= Q_2(x) \\
&\ldots \\
f_n(x)y_{p_n}^{(n)} + \ldots + f_1(x)y_{p_n}^{(1)} + f_0(x)y_{p_n} &= Q_n(x) \\
\end{align}
$$
then
$$y_p = y_{p_1} + y_{p_2} +  \ldots + y_{p_n}$$ is a solution of 

$$
f_n(x)y_p^{(n)} + \ldots + f_1(x)y_p^{(1)} + f_0(x)y_p = Q(x) 
$$
Where 
$$Q(x) = Q_1(x) + Q_2(x) + \ldots + Q_n(x)$$




#### Distributive property

If $y_p$  is a solution of 
$$
f_n(x)y^{(n)} + \ldots + f_1(x)y' + f_0(x)y = Q(x)
$$

Then $Ay_p$ is a solution of $AQ(x)$ 

$$
\begin{align}
&f_n(x)\left(Ay^{(n)}\right)  + \ldots + f_1(x)\left(Ay'\right) + f_0(x)\left(Ay\right) \\
&= f_n(x)\left(Ay^{(n)}\right)  + \ldots + f_1(x)\left(Ay'\right) + f_0(x)\left(Ay\right) \\
&=A \left( f_n(x)y^{(n)}  + \ldots + f_1(x)y' + f_0(x)y\right) \\
&= AQ(x)
\end{align}
$$

#### Real and imaginary solutions

Based on the superposition distributive properties, if $y_{p_1}$ is a solution to 
$$
f_n(x)y^{(n)} + \ldots + f_1(x)y' + f_0(x)y = Q_1(x)
$$
and $y_{p_2}$ is a solution to 
$$
f_n(x)y^{(n)} + \ldots + f_1(x)y' + f_0(x)y = Q_2(x)
$$
then $y_p=y_{p_1}+iy_{p_2}$ is a solution to 
$$
f_n(x)y^{(n)} + \ldots + f_1(x)y' + f_0(x)y = Q_1(x) + iQ_2(x)
$$
where $i$ is the $A$ in the distributive property above
