An operator is a mathematical device that converts one function into another such as the differential and the integration. 

The differential operator is written as $D$. So if $y$ is an $n^{th}$ order differentiable function then
$$D^0y=y,\hspace{1em}D^1=y',\hspace{0.5em}\ldots\hspace{0.5em} ,D^ny=y^{(n)}$$
Forming a linear combination of differential operators gives us a **polynomial operator of order n**
$$P(D)=a_0+a_1D+a_2D^2+\ldots+a_nD^n,\hspace{1em}a_n\neq0$$
Apply this polynomial operation to $y$
$$
\begin{align}
P(D)y&=(a_nD^n+\ldots+a_1D+a_0)y \\
&=a_nD^ny+\ldots+a_1Dy+a_0y \\
&= a_ny^{(n)}+\ldots+a_1y'+a_0y, \hspace{4em}a_n\neq0 \\
&= Q(x)
\end{align}
$$
Different notation for the same thing like in [[Undetermined Coefficient]].

# Properties
## Linear 
### Linear operator
$$
\begin{align}
P(D)(c_1y_1+c_2y_2+\ldots+c_ny_n)&=c_1P(D)y_1+c_2P(D)y_2+\ldots+c_nP(D)y_n \\
&=\sum_{i=1}^n c_iP(D)y_i
\end{align}$$

Insert example here


With the linear operator 
1. If $y_1,y_2,\ldots,y_n$ are $n$ solutions of the [[Linear ODE#Homogeneous|homogenous linear equation]] $P(D)y=0$, then $y_c=\sum_i^nc_iy_i$ is also a solution
2. If $y_c$ is a solution of the [[Linear ODE#Homogeneous|homogeneous linear equation]] $P(D)y=0$ and $y_p$ is a particular solution of the  [[Linear ODE#Nonhomogeneous|non homogeneous linear equation]] $P(D)y=Q(x)$, then $y=y_h+y_p$ is a solution of $P(D)y=Q(x)$.


### Principle of superposition
If we have a sum of different [[Linear ODE#Nonhomogeneous|non homogeneous linear equation]] $Q(x)$ and make it equal to a polynomial operator
$$P(D)y=Q_1+Q_2+\ldots+Q_n$$

Then we assume that the same polynomial operator can be equivalent to the individual $Q_i(x)$ 

$$P(D)y=Q_1,\hspace{0.5em}P(D)y=Q_2,\ldots,P(D)y=Q_n$$
given
$$y_{1p},y_{2p},\ldots,y_{np}$$

Therefore
$$P(D)y_{1p}=Q_1,\hspace{0.5em}P(D)y_{2p}=Q_2,\ldots,P(D)y_{np}=Q_n$$

Adding all these equation and making use of the [[#Linear operator|linear operator property]] results in
$$P(D)(y_{1p}+y_{2p}+\ldots+y_{np})=Q_1+Q_2+\ldots+Q_n$$

which implies that
$$y=y_{1p}+y_{2p}+\ldots+y_{np}$$
is a solution of $P(D)y=\sum_{i=1}^n Q_i$


