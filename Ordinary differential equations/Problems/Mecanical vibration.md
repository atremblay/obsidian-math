![[../Images/spring.svg|400]]

The position of the mass with respect to time can be described by a second order ODE. Setting the problem is the difficult part.

By Newton's Second Law, the sum of all forces is
$$F=ma=mx''$$

By [[Hooke's law]], the restoring force on the mass by the spring is proportional to the displacement $x$.
$$F_s = -kx$$
Friction Force. The damping force (e.g. friction and/or a device that dampens the movement) is proportional to the speed $x'=\dot{v}$
$$F_f=-cx'$$
The **sum of all forces** applied to an object equals its mass times acceleration

$$F=mx'' = F_s + F_f = -kx -cx'$$
This is a direct application of [[Const Coeff Homo DE|constant coefficient homogeneous differential equation]]. 
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



# Higher order DE

Instead of having one mass and one spring, let's look at two springs and two masses and no damping.

![[two_springs.svg]]

The forces acting on $m_1$ are the first and second spring. So the sum of forces will have two terms.

> [!tip]- 
> The main difficulty here is to realize that $x_1$ and $x_2$ are displacement from their equilibrium and NOT position on the x-axis.

### Mass 1
The two springs apply forces on the first mass.

#### From spring 1

Same as above: $F=-k_1x_1$

#### From spring 2

This is a bit more complicated because have to take into consideration the displacement of the second mass. The spring can have to states (ignoring the equilibrium): 
- Stretched. It means that difference in displacement $(x_2-x_1)$ is positive. The displacement of $x_2$ is greater than the displacement of $x_1$. It doesn't matter if the displacements are positive or negative, as long as the difference is positive.
- Compressed. It means that the difference in displacement $(x_2-x_1)$ is negative. Reasoning is the inverse of the stretched state.

Now, if the second spring is stretched, then the restoring force will pull the first mass in the positive direction (right). If the second spring is compressed, the restoring force will push the first mass in the negative direction (left) This implies that the restoring force is proportional to the difference in displacements between the two masses.
$$F=k_2(x_2-x_1)=-k_2(x_1-x_2)$$
#### Sum of forces
We can now use Newton's second law $F=ma$
$$F = -k_1x_1 + k_2(x_2-x_1) = m_1a_1=m_1\ddot{x_1}$$
### Mass 2

Only the second spring acts on the second mass, but in a way similar to the force applied by spring 2 on mass 1. The force applied on mass 2 is simply the inverse of the one applied on mass 1 by the second spring
$$F=-k_2(x_2-x_1)=m_2a_2=m_2\ddot{x_2}$$

## System of equations

We have all the information to build the systems of equations describing the position of both masses with respect to time.
$$
\begin{align}
m_1\ddot{x}_1 + k_1x_1 - k_2(x_2-x_1)&=0 \hspace{4em}(1)\\
m_2\ddot{x}_2 + k_2(x_2-x_1) &= 0 \hspace{4em}(2)
\end{align}
$$

### Four 1st linear ODE
We can describe the whole system with four equations of first order DE. The variables are set such that taking the first derivative can be expressed in terms of the other variables.
$$
\begin{align}
\frac{d}{dt}\begin{bmatrix}
    x_1 \\
    \dot{x}_1 \\
    x_2 \\
    \dot{x}_2 
\end{bmatrix} = 
\frac{d}{dt}\begin{bmatrix}
    x_1 \\
    v_1 \\
    x_2 \\
    v_2 
\end{bmatrix} = 
\begin{bmatrix}
    \dot{x}_1 \\
    \ddot{x}_1 \\
    \dot{x}_2 \\
    \ddot{x}_2 
\end{bmatrix} = 
\begin{bmatrix}
    0 & 1 & 0 & 0 \\
    \frac{-(k_1+k_2)}{m_1} & 0 & \frac{k_2}{m_2} & 0 \\
    0 & 0 & 0 & 1 \\
    \frac{k_2}{m_2} & 0 & \frac{-k_2}{m_2} & 0
\end{bmatrix}
\begin{bmatrix}
    x_1 \\
    v_1 \\
    x_2 \\
    v_2 
\end{bmatrix}
\end{align}
$$
The first and third are the easiest to see. $\dot{x_1} = v_1$ and $\dot{x_2} = v_2$.
To get the second and fourth row we need to isolate $\ddot{x_1}$ in $(1)$ and isolate $\ddot{x_2}$ in $(2)$.


### One 4th linear ODE

This requires a bit of elbow grease. 
1. Express $(1)$ in terms of $x_2$ 
2. Calculate $\ddot{x}_2$
3. Replace $x_2$ and $\ddot{x}_2$ in $(2)$

$$
\begin{align}
x_2 &= \frac{m_1}{k_2}\ddot{x}_1+\frac{k_1}{k_2}x_1 + x_1 \\
\ddot{x_2} &= \frac{m_1}{k_2}x_1^{(4)}+\frac{k_1}{k_2}\ddot{x}_1 + \ddot{x}_1
\end{align}
$$
Replace in $(2)$
$$
\begin{align}
m_2\ddot{x_2} + k_2(x_2-x_1) &= 0 \\
m_2\left(\frac{m_1}{k_2}x_1^{(4)}+\frac{k_1}{k_2}\ddot{x}_1 + \ddot{x}_1\right) + \cancel{k_2}\left(\frac{m_1}{\cancel{k_2}}\ddot{x}_1+\frac{k_1}{\cancel{k_2}}x_1 + \cancel{x_1}-\cancel{x_1}\right) &= 0 \\
\frac{m_1m_2}{k_2}x_1^{(4)}+\textcolor{orange}{\frac{m_2k_1}{k_2}\ddot{x_1} + m_2\ddot{x}_1 + m_1\ddot{x}_1}+k_1x_1 &= 0 \\
\frac{m_1m_2}{k_2}x_1^{(4)}+\textcolor{orange}{\frac{m_2(k_1+k_2)+m_1k_2}{k_2}\ddot{x}_1} +k_1x_1  &= 0
\end{align}
$$
To give it a more familiar look ([[Const Coeff Homo DE]])
$$a_4x_1^{(4)}+a_2\ddot{x}_1 +a_0x_1  = 0$$
with 
$$a_4=\frac{m_1m_2}{k_2}, \hspace{1em}a_2=\frac{m_2(k_1+k_2)+m_1k_2}{k_2}, \hspace{1em}a_0=k_1$$
We now have one fourth order differential equation in terms of $x_1$. The same steps could be done to get a 4th ODE in terms of $x_2$.


# Two masses, three springs
At this point it's just annoying to build the high order ODEs, so I'm not going to.

> [!tip]-
> At a future point in time it will probably be confusing to read these equations. Just go through them one by one and add more masses. First read through [[Mecanical vibration#Higher order DE]] where the logic of the masses displacement are explained.

$$\begin{align}
F_1&=-k_1x_1+k_2(x_2-x_1) = m_1a_1=m_1\ddot{x}_1 \\
\iff  \ddot{x}_1 &= \frac{1}{m_1}\left(-k_1x_1+k_2(x_2-x_1)\right) \\
&= -x_1\textcolor{orange}{\frac{(k_1+k_2)}{m_1}}+x_2\textcolor{violet}{\frac{k_2}{m_1}}
\\ \\
F_2&=-k_2(x_2-x_1)-k_3x_2 = m_2a_2=m_2\ddot{x}_2 \\
\iff  \ddot{x}_2 &= \frac{1}{m_2}\left(-k_2(x_2-x_1)-k_3x_2\right) \\
&= x_1\textcolor{lightgreen}{\frac{k_2}{m_2}}-x_2\textcolor{lightblue}{\frac{(k_2+k_3)}{m_2}}
\end{align}
$$

State vector
$$\mathbf{x}=\begin{bmatrix}
x_1 \\ \dot{x}_1 \\ x_2 \\ \dot{x}_2
\end{bmatrix}$$
System of ODE
$$
\begin{align}
\mathbf{\dot{x}} &= \mathbf{Ax} \\
\frac{d}{dt}\begin{bmatrix}
x_1 \\ \dot{x}_1 \\ x_2 \\ \dot{x}_2
\end{bmatrix}
&=\begin{bmatrix}
0 & 1 & 0 & 0 \\
\textcolor{orange}{-\frac{(k_1+k_2)}{m_1}} & 0 & \textcolor{violet}{\frac{k_2}{m_1}} & 0\\
0 & 0 & 0 & 1 \\
\textcolor{lightgreen}{\frac{k_2}{m_2}} & 0 & \textcolor{lightblue}{-\frac{(k_2+k_3)}{m_2}} & 0
\\
\end{bmatrix}
\begin{bmatrix}
x_1 \\ \dot{x}_1 \\ x_2 \\ \dot{x}_2
\end{bmatrix}
\end{align}
$$

# Three masses, four springs
The first mass is affected by the springs on the left ($k_1$) and right ($k_2$). There is no displacement on the left as it's attached to a wall, so the displacement $x_1$ is affected only by both springs and not some other moving mass on the left. 
The right spring is affected by the displacement $x_2$.
$$\begin{align}
F_1&=-k_1x_1+k_2(x_2-x_1) = m_1a_1=m_1\ddot{x}_1 \\
\iff  \ddot{x}_1 &= \frac{1}{m_1}\left(-k_1x_1+k_2(x_2-x_1)\right) \\
&= -x_1\textcolor{orange}{\frac{(k_1+k_2)}{m_1}}+x_2\textcolor{lightgreen}{\frac{k_2}{m_1}} \\
\end{align}
$$
The middle mass, $m_2$, is affected by the springs on the left ($k_2$) and the right ($k_3$) and those springs are affected by the displacements $x_1$ of mass $m_1$ and $x_3$ of mass $m_3$.
$$
\begin{align}
F_2&=-k_2(x_2-x_1)-k_3(x_3-x_2) = m_2a_2=m_2\ddot{x}_2 \\
\iff  \ddot{x}_2 &= \frac{1}{m_2}\left(-k_2(x_2-x_1)+k_3(x_3-x_2)\right) \\
&= x_1\textcolor{magenta}{\frac{k_2}{m_2}} - x_2\textcolor{pink}{\frac{(k_2+k_3)}{m_2}}-x_3\textcolor{violet}{\frac{k_3}{m_2}} 
\end{align}
$$
The last mass on the left is similar to the first mass on the left.
$$
\begin{align}
F_3&=-k_3(x_3-x_2)-k_4x_3 = m_3a_3=m_3\ddot{x}_3 \\
\iff  \ddot{x}_3 &= \frac{1}{m_3}\left(-k_3(x_3-x_2)-k_4x_3\right) \\
&= x_2\textcolor{lightblue}{\frac{k_3}{m_3}}-x_3\textcolor{teal}{\frac{(k_3+k_4)}{m_3}}
\end{align}
$$

State vector
$$\mathbf{x}=\begin{bmatrix}
x_1 \\ \dot{x}_1 \\ x_2 \\ \dot{x}_2 \\ x_3 \\ \dot{x}_3
\end{bmatrix}$$
System of ODE
$$
\begin{align}
\mathbf{\dot{x}} &= \mathbf{Ax} \\
\frac{d}{dt}\begin{bmatrix}
x_1 \\ \dot{x}_1 \\ x_2 \\ \dot{x}_2 \\ x_3 \\ \dot{x}_3
\end{bmatrix}
&=\begin{bmatrix}
0 & 1 & 0 & 0 & 0 & 0\\
\textcolor{orange}{-\frac{(k_1+k_2)}{m_1}} & 0 & \textcolor{lightgreen}{\frac{k_2}{m_1}} & 0 & 0 & 0\\
0 & 0 & 0 & 1 & 0 & 0\\
\textcolor{magenta}{\frac{k_2}{m_2}} & 0 & \textcolor{pink}{-\frac{(k_2+k_3)}{m_2}} & 0 & \textcolor{violet}{\frac{k_3}{m_2}} & 0
\\
0 & 0 & 0 & 0 & 0 & 1\\
0 & 0 & \textcolor{lightblue}{-\frac{(k_2+k_3)}{m_3}} & 0 & \textcolor{teal}{\frac{k_4}{m_3}} & 0
\end{bmatrix}
\begin{bmatrix}
x_1 \\ \dot{x}_1 \\ x_2 \\ \dot{x}_2 \\ x_3 \\ \dot{x}_3
\end{bmatrix}
\end{align}
$$

# $n$ masses, $n-1$ springs
At this point it's just the same thing as 3 masses and 4 springs. Left and right will have the same equations and all the middle masses will be the same as the second mass in the previous section.

Left mass
$$\begin{align}
F_1&=-k_1x_1+k_2(x_2-x_1) = m_1a_1=m_1\ddot{x}_1 \\
\iff  \ddot{x}_1 &= \frac{1}{m_1}\left(-k_1x_1+k_2(x_2-x_1)\right) \\
&= -x_1\textcolor{orange}{\frac{(k_1+k_2)}{m_1}}+x_2\textcolor{lightgreen}{\frac{k_2}{m_1}} \\
\end{align}
$$
Middle mass, for $1<i<n$
$$
\begin{align}
F_i&=-k_i(x_i-x_{i-1})-k_{i+1}(x_{i+1}-x_i) = m_ia_i=m_i\ddot{x}_i \\
\iff  \ddot{x}_i &= \frac{1}{m_i}\left(-k_i(x_i-x_{i-1})+k_{i+1}(x_{i+1}-x_i)\right) \\
&= x_{i-1}\textcolor{magenta}{\frac{k_i}{m_i}} - x_i\textcolor{pink}{\frac{(k_i+k_{i+1})}{m_i}}-x_{i+1}\textcolor{violet}{\frac{k_{i+1}}{m_i}} 
\end{align}
$$

Right mass
$$
\begin{align}
F_3&=-k_3(x_3-x_2)-k_4x_3 = m_3a_3=m_3\ddot{x}_3 \\
\iff  \ddot{x}_3 &= \frac{1}{m_3}\left(-k_3(x_3-x_2)-k_4x_3\right) \\
&= x_2\textcolor{lightblue}{\frac{k_3}{m_3}}-x_3\textcolor{teal}{\frac{(k_3+k_4)}{m_3}}
\end{align}
$$
