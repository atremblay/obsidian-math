

> [!Summary]
> $$\int_0^T e^{\mathbf{X}t} dt=\left(e^{\mathbf{X}T}-\mathbf{I}\right)\mathbf{X}^{-1}$$
> If $\mathbf{X}$ is invertible

^68432f

If $\mathbf{X}$ is invertible, then we can use the property that $\mathbf{XX^{-1}=I}$
$$
\begin{align}
\int_0^T e^{\mathbf{X}t} dt &= \int_0^T \mathbf{I} + \frac{\mathbf{X}t}{1!} + \frac{\mathbf{X}^2t^2}{2!} + \frac{\mathbf{X}^3t^3 }{3!}+ \ldots dt \\
&= \mathbf{I}t + \frac{\mathbf{X}t^2}{2!} + \frac{\mathbf{X}^2t^3}{3!} + \frac{\mathbf{X}^3t^4 }{4!}+ \ldots \Biggm\lvert_0^T \\
&= \mathbf{I}T + \frac{\mathbf{X}T^2}{2!} + \frac{\mathbf{X}^2T^3}{3!} + \frac{\mathbf{X}^3T^4 }{4!}+ \ldots \\
&= \left(\left(\mathbf{I} + \mathbf{I}T + \frac{\mathbf{X}^2T^2}{2!} + \frac{\mathbf{X}^3T^3}{3!} + \frac{\mathbf{X}^4T^4 }{4!}+ \ldots\right)-\mathbf{I}\right)\mathbf{X}^{-1} \\
&= \left(e^{\mathbf{X}T}-\mathbf{I}\right)\mathbf{X}^{-1}
\end{align}
$$
