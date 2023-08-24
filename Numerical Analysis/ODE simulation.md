[[../Ordinary differential equations/System of ODE]]
$$\mathbf{\dot{x}}(t)=\mathbf{A}\mathbf{x}(t) \implies\mathbf{\dot{x}}(t)=\mathbf{f(x(t))}$$

$\mathbf{x}$ is a vector representing the state of a system 
$\mathbf{f(x)}$ may be a non-linear system. This is literally the set of instructions to make $\mathbf{x}$ move forward in time. 

Take a vector field, every point in it has a direction of movement and taking a small step in that direction will bring us somewhere else. Could be useful to predict where a lifeboat lost as sea would be. Or how an oil spill will evolve and where we should deploy our ressources to clean the mess.


![[../Calculus/Images/Pasted image 20211121100624.png|400]]

To be more thourough, we could add a time variable as the system can change over time (non-autonomous). The system need not be the same foreever and ever.
$$\mathbf{\dot{x}}=\mathbf{f(x,t)}$$
Doing these small step is also called **integrating**, not to be confused with the anti-derivative **integration**. Why do we want to do that? Because most real world problems are not solvable, they are non-linear and impossible to solve analytically. We are left with numerical schemes.

## Forward Euler Integration
$$\mathbf{x}_{k+1}=F(\mathbf{x}_k)$$
$F(\mathbf{x}_k)$ is a generic function that takes the current position and returns a new position at the next time step. This is very similar to the [[Numerical differentiation#^458dc5|update rule]] for differentiation.
If we can calculate $\mathbf{\dot{x}}_k$, or have it by any other means, then we have an update rule for $\mathbf{x}(t+\Delta t)$ as
$$\mathbf{x}(t+\Delta t) \approx \mathbf{x}(t) + \Delta t\mathbf{\dot{x}}(t)$$
Or a bit cleaner notation
$$\mathbf{x}(t+\Delta t) \approx \mathbf{x}_{k+1} = \mathbf{x}_k + \Delta t\mathbf{\dot{x}}_k=\mathbf{x}_k + \Delta t\mathbf{f(x_k)}$$
In the case of an idealized version where (aka linear) $\mathbf{f(x_k)}=\mathbf{A}\mathbf{x_k}$

$$
\begin{align}
\mathbf{x}_{k+1} &= \mathbf{x}_k + \Delta t\mathbf{A}\mathbf{x_k} \\
&= (\mathbb{I} + \Delta t\mathbf{A})\mathbf{x_k}
\end{align}$$

The interesting thing is that $\mathbb{I} + \Delta t\mathbf{A}$ are the first two terms of the [[../Linear Algebra/Matrix exponential|Taylor expansion]]
$\mathbf{x}_{k+1}=e^{\mathbf{A}\Delta t}\mathbf{x}_{k}$


> [!note] 
> At this point you are more used to
> $$\mathbf{x}(t)=e^{\mathbf{A}t}\mathbf{x}(0)$$
> but squint a bit harder and notice that in the exponent there is a $\Delta t$. So it means that the system moved forward in time by $\Delta t$, which corresponds to the state $\mathbf{x}_{k+1}$

### Error of Forward Euler

> [!summary]
> - In the case of non-chaotic systems, we can compute the local one step forward, and global error
> - In a chaotic system the small divergence will have a butterfly effect. We cannot estimate the total error of approximation, only locally. That small delta effect will accumulate and diverge exponentially due to the inherent dynamic of the system.
>  
> Locally the error is in the order of $\mathcal{O}(t)$.


The true next value is $\mathbf{x}(t+\Delta t)$, but we can only approximate it by taking a small step forward. 

Taylor expand
$$
\begin{align}
\mathbf{x}(t+\Delta t) &= \mathbf{x}(t) + \dot{\mathbf{x}}(t)\Delta t + \frac{\ddot{\mathbf{x}}(t)\Delta t^2}{2}+ \frac{\dddot{\mathbf{x}}(t)\Delta t^3}{3!} + \ldots \\
&= \underbrace{\mathbf{x}(t) + \mathbf{f}(\mathbf{x}(t))\Delta t}_{\text{Approximation from above}} + \underbrace{\frac{\ddot{\mathbf{x}}(t)\Delta t^2}{2}+ \frac{\dddot{\mathbf{x}}(t)\Delta t^3}{3!} + \ldots}_{\text{Error of approximation}}
\end{align}
$$
Part of the Taylor Expansion is our approximation above. The rest of it is the error by our approximation, so order $\mathcal{O}(\Delta t^2)$. That error is at **every step**, so it's a local error. Globally the number of steps scale by $\frac 1 {\Delta t}$ (smaller step = more steps), so the global error is in the order of $\mathcal{O}(\Delta t)$.



## Backward Euler Integration

$$
\begin{align}
\frac{\mathbf{x_{k+1}} - \mathbf{x_{k}}}{\Delta t} &= \mathbf{f(x_{k+1})} \\
\iff \mathbf{x_{k+1}} &= \mathbf{x_{k}} + \Delta t\mathbf{f(x_{k+1})}
\end{align}
$$
The difference with the Forward Integration is subtle, check the subscript of $\mathbf{f}$. This is an implicit equation because $\mathbf{x_{k+1}}$ appears on both sides of the equation and cannot be easily isolated.

Again, in the idealized case where the vector field is linear
$$
\begin{align}
\mathbf{x}_{k+1} &\approx \mathbf{x}_k + \Delta t\mathbf{A}\mathbf{x_{k+1}} \\
\iff \mathbf{x}_{k+1} - \Delta t\mathbf{A}\mathbf{x_{k+1}} &\approx \mathbf{x}_k  \\
\iff \mathbf{x}_{k+1}&\approx (\mathbb{I} - \Delta t\mathbf{A})^{-1}\mathbf{x_k}
\end{align}$$



> [!Example]- Mass on a spring
> [[../Ordinary differential equations/Problems/Mecanical vibration]]
> 
> $$
> \begin{align}
> m\ddot{x} &=  -kx -c\dot{x} \\
> \implies m\ddot{x}+c\dot{x} + kx  &= 0 \\
> \implies \ddot{x}+ \frac c m \dot{x} + \frac k m x  &= 0 \\
> \end{align}
> $$
> 
> To make a more readable [[../Calculus/Characteristic equation]] we do a change of variable (that seems out of nowhere, but it comes from other people's experience)
> 
> $$\omega_0 = \sqrt \frac k m, \zeta = \frac c {2 \sqrt{km}}$$
> $$
> \begin{align}
> \ddot{x}+ 2\zeta \omega_0 \dot{x} + \omega_0^2 x  &= 0 \\
> \implies \ddot{x}  &= - 2\zeta \omega_0 \dot{x} - \omega_0^2 x \\
> \end{align}
> $$
> 
> Finding the eigenvalues of this, we'll find that
> $$
> \begin{align}
> \zeta &< 1\hspace{2em}\text{Under damped, will oscillate} \\
> \zeta &> 1\hspace{2em}\text{Over damped, will directly go back to rest position} \\
> \zeta &= 1\hspace{2em}\text{Critically damped} \\
> \end{align}
> $$
> 
> As a [[../Ordinary differential equations/System of ODE]]
> 
> $$
> \begin{align}
> \frac{d}{dt}\begin{bmatrix}
>     x \\ \dot{x} 
> \end{bmatrix} = 
> \frac{d}{dt}\begin{bmatrix}
>     x \\ v
> \end{bmatrix} = 
> \begin{bmatrix}
>     \dot{x} \\ \ddot{x} 
> \end{bmatrix} = 
> \begin{bmatrix}
>     0 & 1  \\
>     -\omega_0^2 & -2\zeta \omega_0
> \end{bmatrix}
> \begin{bmatrix}
>     x \\ v
> \end{bmatrix}
> \end{align}
> $$
> 
> ```python
> from phaseportrait import PhasePortrait2D
> import matplotlib.pyplot as plt
> import numpy as np
> 
> omega = 2 * np.pi
> zeta = 0.25
> 
> def dF_1(x, y, omega, zeta):
>     return y, -(omega**2) * x - 2 * zeta * omega * y
> 
> example1 = PhasePortrait2D(
>     dF=dF_1,
>     Range=[-3, 3],
>     dF_args={"omega": omega, "zeta": zeta},
>     density=1.2,
>     color="cool",
>     Title="Mass on a spring with dampener",
>     xlabel="x",
>     ylabel=r"$\dot{x}$",
> )
> fig, ax = example1.plot()
> plt.show()
> ```
> 
> ![[../Images/mass_spring_damp.svg]]


# Stability

When the system is linear, the Forward/Backward Euler simplifies a bit. Simulating a system is a discrete process, so we have to look at the scalar in front of $x_k$.

**Forward**
$$
\begin{align}
\mathbf{x}_{k+1} &= \mathbf{x}_k + \Delta t\mathbf{A}\mathbf{x_k} \\
&= (\mathbb{I} + \Delta t\mathbf{A})\mathbf{x_k}
\end{align}$$
**Backward**
$$
\begin{align}
\mathbf{x}_{k+1} &= \mathbf{x}_k + \Delta t\mathbf{A}\mathbf{x_{k+1}} \\
&= (\mathbb{I} - \Delta t\mathbf{A})^{-1}\mathbf{x_k}
\end{align}$$
Intuition building, look at scalar version
$$
\begin{align}
x_{k+1} &= \beta x_k \\
&= \beta^2 x_{k-1} \\
&= \beta^3 x_{k-2} \\
&= \ldots \\
&= \beta^{k+1} x_{0} \\
\end{align}
$$
So if $\lvert \beta\rvert > 1$, the system will blow up to infinity and is unstable.
If $|\beta| < 1$, the system will shrink to zero and is stable.

If $\beta=a+bi$ is complex, then $R = \sqrt {a^2+b^2}$ and using [[../Polar coordinate in complexe plane]]
$$
\begin{align}
\beta &= Re^{i\theta} \\
\beta^2 &= R^2e^{i2\theta} \\
&\ldots \\
\beta^{k} &= R^ke^{ik\theta} \\
\end{align}
$$

$$e^{ik\theta} = \cos{k\theta} + i\sin{k\theta}$$
and this will simply rotate. If $R<1$, then it will rotate and shrink to 0. If $R>1$ then it will rotate to infinity. If $R=1$ the it will rotate forever.

**Forward**
$$x_{k+1} = (1+\lambda \Delta t)x_k = (1+\lambda \Delta t)^{k+1}x_0$$
is unstable if $|1+\lambda \Delta t| > 1$
is stable if $|1+\lambda \Delta t| < 1$

To have a stable system then we need $-2<\lambda \Delta t$ < 0. In the complex plane that would be a circle of radius 1 centered at -1.

The equivalent for multivariate systems is that the absolute values of the eigenvalues of $\mathbb{I} + \Delta t\mathbf{A}$ are smaller or bigger than one.

**Backward**

$$x_{k+1} = (1-\lambda \Delta t)^{-1}x_k = (1-\lambda \Delta t)^{-(k+1)}x_0$$
- is unstable if $|1-\lambda \Delta t| < 1$ (this is at the denominator, so if it's smaller than 1 then $x_0$ gets multiplied by something larger than 1)
- is stable if $|1-\lambda \Delta t| > 1$
Which is the opposite of the forward method.

To have a stable system then we need $\lambda \Delta t$ to be anywhere BUT in $-2<\lambda \Delta t$ < 0. In the complex plane that would be a circle of radius 1 centered at -1.

The equivalent for multivariate systems is that the absolute values of the eigenvalues of $\mathbb{I} - \Delta t\mathbf{A}$ are smaller or bigger than one.

Backward Euler is more stable than Forward Euler because there is a larger set of $\Delta t$ that will make the system stable.
