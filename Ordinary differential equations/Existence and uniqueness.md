
> [!summary] 
> If $f_{i}(x)$ for $i \in (0,n)$ and $Q(x)$ are each continuous functions of $x$ on a common internval $I$, and $f_i(x) \neq 0$ when $x$ is in I, then the linear differential equation
> $$f_{n}(x)y^{(n)}+f_{n-1}(x)y^{(n-1)}+\ldots+f_{1}(x)y^{(1)}+f_{0}(x)y=Q(x)$$ 
> has one (existence) and only one (uniqueness) solution
> $$y=y(x)$$
> satisfying the set of initial conditions
> $$y(x_{0}) = y_0, y'(x_{0}) = y_1, \ldots, y^{(n-1)}(x_{0}) = y_{n-1}$$
> where $x_0$ is in $I$, and $y_i$ are constants


# Second order linear ODE

$$y''+p(x)y'+q(x)y=f(x)$$
- All $y$ and its derivates are all power of 1, hence the *linear* part.
- If $f(x)=0$ then the equation is *homogeneous*

##### Theorem: Existence & Uniqueness
For the IVP
$$y''+p(x)y'+q(x)y=f(x),\hspace{2em}y(a)=b_0,\hspace{0.5em}y'(a)=b_1$$

If *p, q, f* continuous on an interval *I* about *a*, then there exists a unique solution on *I*. This can be generalized to $n^{th}$ derivative and require *n* initial conditions.

##### Theorem: Superposition
The differential equation must be homogeneous. Suppose $\textcolor{yellow}{y_1}$ and $\textcolor{cyan}{y_2}$ solve 
$$y''+p(x)y'+q(x)y=0$$
Then $y=c_1\textcolor{yellow}{y_1} + c_2\textcolor{cyan}{y_2}$ also solves.

$$
\begin{align}
&(c_1y_1 + c_2y_2)'' + p(x)(c_1y_1 + c_2y_2)' + q(x)(c_1y_1 + c_2y_2) \\
= &(c_1y_1'' + c_2y_2'') + p(x)(c_1y_1' + c_2y_2') + q(x)(c_1y_1' + c_2y_2') \\
= &c_1(\underbrace{y_1'' + p(x)y_1'+q(x)y_1}_{\textcolor{yellow}{y_1}\text{ solves}}) + c_2(\underbrace{y_2'' + p(x)y_2'+q(x)y_2}_{\textcolor{cyan}{y_2}\text{ solves}}) \\
=&0+0=0
\end{align}
$$

##### Theorem: Linear independance
$y_1$ and $y_2$ are linearly independant on an interval if they are not a scalar multiple of each other.

Suppose there are two __linearly independant__ solutions $\textcolor{yellow}{y_1}$ and $\textcolor{cyan}{y_2}$ to 
$$y''+p(x)y'+q(x)y=0$$
then the [[../Calculus/General solution]] is 
$$y=c_1\textcolor{yellow}{y_1} + c_2\textcolor{cyan}{y_2}$$
That means that if you find more than 2 solutions then you have some that are linear combinations of 2 linearly independant solutions. There cannot be more than 2 solutions.

# $n^{th}$ order linear ODE
With the general form of a higher order linear ODE
$$y(n) + p_{n-1}(x)y^{(n-1)} + \ldots + p_{0}(x)y = f(x)$$

##### Theorem: Existence & Uniqueness
For the IVP

$$
\begin{align}
&y(n) + p_{n-1}(x)y^{(n-1)} + \ldots + p_{0}(x)y = f(x), \\
&y(a) = b_0, y'(a) = b_1, \ldots, y^{(n-1)}(a) = b_{n-1}
\end{align}
$$
If all $p_{i}$ and f continuous on an interval *I* about *a*, then there exists a unique solution on *I*.

##### Theorem: Superposition

The differential equation must be homogeneous. Suppose $y_1, \ldots, y_n$ solve 
$$y(n) + p_{n-1}(x)y^{(n-1)} + \ldots + p_{0}(x)y =0$$
Then $y=c_1y_1 + \ldots + c_ny_n$ also solves.


##### Theorem: [[../Calculus/General solution]]
Suppose $y_1, \ldots, y_n$ solve 
$$y(n) + p_{n-1}(x)y^{(n-1)} + \ldots + p_{0}(x)y =0$$
Then $y=c_1y_1 + \ldots + c_ny_n$ is the *[[../Calculus/General solution]]* if and only if the [[../Calculus/Lin. Ind. of Func#Wronskian|Wronskian]] $W(t) \neq 0$ for some $t_0$.

This tells us that we can stop searching for new solutions once you have found *n* independant ones. Anything else will be a combination of these.