
A mass is attached to a spring on an horizontal surface. Where the mass sits at rest is $x=0$. The mass is then pulled and release. What is the solution describing the motion of the mass over time?

This is a direct application of [[Const Coeff Homo DE|constant coefficient homogeneous differential equation]]. Setting the problem is the difficult part.

Newton's Second Law
$$F=ma=mx''$$

Hooke's Law
$$F_s = -kx$$
Friction Force
$$F_f=-cx'$$
The sum of all forces applied to an object equals its mass times acceleration

$$F=mx'' = F_s + F_f = -kx -cx'$$
Let's turn this into an [[Const Coeff Homo DE|constant coefficient homogenous differential equation]]
$$mx''+cx'+kx=0$$
As often, guess: $x=e^{rt}$

The [[characteristic equation]] is $mr^2+cr+k=0$ and the roots are
$$
r = \frac{-c \pm \sqrt{c^2-4mk}}{2m}
$$
There will be 3 cases for the possible values of the roots

### [[Const Coeff Homo DE#Case 1 Distinct roots|Case 1]] Distinct roots
If $c^2-4mk>0$ then we have real, distinct roots, $r_1$ and $r_2$. So the solution will have the form
$$x=c_1e^{r_1t} + c_2e^{r_2t}$$
We can push the analysis a bit further. The numerator will necessarily be negative, so the roots will be negative as well. 
$$
\begin{align}
&c^2-4mk > 0 \\
\iff &c^2 > 4mk \\
\iff &1 > \frac{4mk}{c^2} \\
\end{align}
$$
So
$$
\begin{align}
\sqrt{c^2-4mk} = c\sqrt{1-\frac{4mk}{c^2}} < c
\end{align}
$$
Giving 
$$-c \pm \sqrt{c^2-4mk} = -c \pm c\sqrt{1-\frac{4mk}{c^2}} < -c \pm c \leq 0$$
The [[general solution]] $x=c_1e^{r_1t} + c_2e^{r_2t}$ decays to the neutral position ($x=0$) overtime. There will not be any back and forth. This is called **overdamped**.


### [[Const Coeff Homo DE#Case 2 Repeated root|Case 2]] Repeated root
If $c^2-4mk=0$ then we have one real repeated root $r=\frac{-c}{2m}$.

The [[Const Coeff Homo DE#General solution|general solution]] is
$$x=c_1e^{rt} + c_2te^{rt}$$


### [[Const Coeff Homo DE#Case 3 Complex pair of roots|Case 3]] Complex pair of root
If $c^2-4mk < 0$ then we have a pair of complex roots.
$$
r = \frac{-c \pm \sqrt{c^2-4mk}}{2m} = \underbrace{\frac{-c }{2m}}_{\alpha} \pm \underbrace{\frac{\sqrt{c^2-4mk}}{2m}}_{i\omega} = \alpha \pm i\omega
$$
The [[Const Coeff Homo DE#General solution|general solution]] is
$$
x=e^{\alpha t}[c_1\cos \omega t + c_2\sin \omega t]
$$
The $\sin$ and $\cos$ indicate a back and forth movement. With $\alpha$ being negative, $e^{\alpha t}$ indicates a decay towards $x=0$ as $t \rightarrow \infty$.

>This next section is incomplete. I do not understand what Trefor is doing in his video by introducing a C variable.


Using [[Trigonometric identity#Sum and difference of angles|trigonometric identity of cosine]] $\cos(\delta-\gamma)=\cos\delta\cos\gamma+\sin\delta\sin\gamma$, can we rearrange $c_1\textcolor{orange}{\cos \omega t} + c_2\textcolor{cyan}{\sin \omega t}$ To have the left-hand side instead? This is not required, it just shows a cleaner solution.

Let's set $\delta=\omega t$
$$\cos(\omega t-\gamma)=\textcolor{orange}{\cos\omega t} \cos\gamma+\textcolor{cyan}{\sin\omega t} \sin\gamma$$



