> [!summary]
>  $$e^{i\theta} = \cos \theta + i \sin \theta$$

Based on the [[Maclaurin Series]] for $e^x$

$$
\begin{align}
e^x &= 1+ \theta + \frac{\theta^2}{2!} + \frac{\theta^3}{3!} + \ldots
\end{align}
$$

We can expand $e^{i\theta}$ and use the Maclaurin expansion of $\cos(x)$ and $\sin(x)$
$$
\begin{align}
e^{i\theta} &= 1+ i\theta + \frac{(i\theta)^2}{2!} + \frac{(i\theta)^3}{3!} + \frac{(i\theta)^4}{4!} + \ldots \\ 
 &= \textcolor{orange}{1}+ \textcolor{olive}{i\theta} \textcolor{orange}{-\frac{\theta^2}{2!}} \textcolor{olive}{- \frac{i\theta^3}{3!}} \textcolor{orange}{+\frac{\theta^4}{4!}} \textcolor{olive}{+ \frac{i\theta^5}{5!}} + \ldots \\
 &= \left(\textcolor{orange}{1-\frac{\theta^2}{2!}+\frac{\theta^4}{4!}+ \ldots} \right)+ i\left(\textcolor{olive}{x- \frac{i\theta^3}{3!}+ \frac{i\theta^5}{5!}+ \ldots} \right)  \\
&= \textcolor{orange}{\cos(\theta)} + i\textcolor{olive}{\sin(\theta)}
\end{align}
$$

## Euler's identity

Just a special case of Euler's formula where $\theta=\pi$.

> [!summary]
>  $$e^{i\pi} = \cos \pi + i \sin \pi=1$$