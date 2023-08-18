
> [!Note]
> 
> $$
> \lim_{ x \to 0 } \frac{\sin x}{x} = 1
> $$
>
> ^46d97d

Use this image to prove it. We are looking to squeeze $ODA$ between two other values. This is a bit twisted because we start from somewhere else entirely to end up on that limit. It must have been found by doing something else and this conclusion came along.

<span class='centerImg'>![[../Images/lim_sinx_over_x.svg|400]]</span>

$$
\text{area } OBA \lt \text{area of sector } ODA \lt \text{area } ODE
$$
But $OBA$ is a right triangle with known hypothenus equal 1 (this is for convenience, it could have been equal to any know value)
$$
\text{area } OBA = \frac 1 2 OB \cdot OA = \frac 1 2 \cos x \sin x
$$

Further, the area  $ODA$ of the sector of the circle is that part of the entire area of the circle which the central angle $x$ is of $2\pi$. That is

$$
\text{area } ODA = \frac x {2\pi} \pi(1)^2 = \frac x 2
$$

Finally,

$$
\text{area } ODE = \frac 1 2 ED \cdot OD = \frac 1 2 \tan x = \frac 1 2 \frac {\sin x}{\cos x}
$$

Combine
$$
\frac 1 2 \cos x \sin x \lt \frac x 2 \lt \frac 1 2 \frac {\sin x}{\cos x}
$$
Divide by $\frac {\sin x} 2$  so that the left and right hand side are easier to analyse

$$
\cos x  \lt \frac x {\sin x} \lt \frac {1}{\cos x}
$$

As $x \to 0$, $\cos x \to 1$. So both left and right-hand side will be one. The quantity in the middle is squeezed between two things that converges to 1. Hence

$$
\lim_{ x \to 0 } \frac{x}{\sin x} = 1
$$
Because the limit is 1, it is also possible to write this as

$$
\lim_{ x \to 0 } \frac{\sin x}{x} = 1
$$


> [!NOTE]
> $$
> \lim_{ x \to 0 } \frac{1-\cos x}{x} = 0
> $$
> 
> ^abe062


Using the [[Trigonometric identity#Half Angle|half angle]] identity

$$
\sin \frac x 2 = \sqrt{  \frac{1-\cos x}{2}}
$$
So that
$$
1-\cos x = 2\sin^2 \left(\frac x 2\right)
$$
Substitute this in the limit
$$
\lim_{ x \to 0 } \frac{2\sin^2 \left(\frac x 2\right)}{x} = \lim_{ x \to 0 } \sin \frac x 2 \frac{\sin \frac x 2}{\frac x 2}
$$

This is getting close to the limit above. As $x \to 0$, so does $\frac x 2$. So, for clarity, let's replace $\frac x 2$ by $z$
$$
\lim_{ z \to 0 } \sin z \frac{\sin z}{z}
$$
Because of the [[Properties#Product rule|product rule]] of limits,

$$
\lim_{ z \to 0 } \sin z  \cdot \frac{\sin z}{z} =  \underbrace{ \lim_{ z \to 0 } \sin z }_{0}   \cdot \underbrace{\lim_{ z \to 0 } \frac{\sin z}{z}}_{1} = 0
$$
We conclude that
$$
\lim_{ x \to 0 } \frac{1-\cos x}{x} = 0
$$