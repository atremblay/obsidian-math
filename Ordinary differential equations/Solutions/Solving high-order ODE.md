
Given a constant coefficients [[../Definitions/Linear ODE#Homogeneous|homogeneous DE]]
$$a_nx^{(n)}+a_{n-1}x^{(n-1)}+\ldots+a_2\ddot{x}+a_1\dot{x}+a_0x=0$$

Try $x(t)=e^{\lambda t}$ as potential solution, just like when solving [[Const Coeff Homo DE]]. This time it has a higher order 
$$
\begin{align}
x(t)&=e^{\lambda t} \\
\dot{x}(t)&=\lambda e^{\lambda t} \\
\ddot{x}(t)&=\lambda^2 e^{\lambda t} \\
&\ldots \\
x^{(n-1)}(t)&=\lambda^{n-1} e^{\lambda t} \\
x^{(n)}(t)&=\lambda^{n} e^{\lambda t}
\end{align}
$$
Plug these derivatives in the initial ODE
$$e^{\lambda t}\underbrace{\left(a_n\lambda^{n}+a_{n-1}\lambda^{n-1}+\ldots+a_2\lambda^2+a_1\lambda+a_0\right)}_{\text{characteristic equation}}=0$$
This will be zero only when the quantity in parentheses, the [[../../Calculus/Characteristic equation]], is zero. That polynomial has $n$ roots. Some of them might be repeated ([[Const Coeff Homo DE#Case 2: Repeated root|repeated roots]]), some of them might be complex ([[Const Coeff Homo DE#Case 3: Complex pair of roots|complex roots]]). 

Each one of these roots will be a solution
$$x_i=e^{\lambda_i t}$$

And by the [[../Superposition]] principle, we can linearly combine all of them to have another solution
$$
\begin{align}
x&=c_1x_1 + c_2x_2 + \ldots + c_{n-1}x_{n-1}+c_nx_n \\
&=c_1e^{\lambda_1 t}+ c_2e^{\lambda_2 t} + \ldots + c_{n-1}e^{\lambda_{n-1} t}+c_ne^{\lambda_n t}
\end{align}
$$

> [!note]- Repeated roots
> Careful with the [[Const Coeff Homo DE#Case 2: Repeated root|repeated roots]]. The solutions will require something of the form $t^re^{\lambda_i t}$

### Solving for the initial conditions

If we have the intial conditions (i.e. we know the values of $x^{(i)}$ at $t=0$), then we can find the values for the coefficients $c_i$

If we take all the derivative of the more general solution
$$
\begin{align}
x(t)&=c_1e^{\lambda_1 t}+ c_2e^{\lambda_2 t} + \ldots + c_{n-1}e^{\lambda_{n-1} t}+c_ne^{\lambda_n t} \\
\dot{x}(t)&=c_1\lambda_1e^{\lambda_1 t}+ c_2\lambda_2e^{\lambda_2 t} + \ldots + c_{n-1}\lambda_{n-1}e^{\lambda_{n-1} t}+c_n\lambda_ne^{\lambda_n t} \\
\ddot{x}(t)&=c_1\lambda_1^2e^{\lambda_1 t}+ c_2\lambda_2^2e^{\lambda_2 t} + \ldots + c_{n-1}\lambda_{n-1}^2e^{\lambda_{n-1} t}+c_n\lambda_n^2e^{\lambda_n t} \\ 
&\ldots \\
x^{(n-1)}(t)&=c_1\lambda_1^{n-1}e^{\lambda_1 t}+ c_2\lambda_{n-1}^2e^{\lambda_2 t} + \ldots + c_{n-1}\lambda_{n-1}^{n-1}e^{\lambda_{n-1} t}+c_n\lambda_n^{n-1}e^{\lambda_n t}  \\
x^{(n)}(t)&=c_1\lambda_1^{n}e^{\lambda_1 t}+ c_2\lambda_{n}^2e^{\lambda_2 t} + \ldots + c_{n-1}\lambda_{n}^{n-1}e^{\lambda_{n} t}+c_n\lambda_n^{n-1}e^{\lambda_n t}
\end{align}
$$
write it in a matrix form
$$
\begin{bmatrix}
x(t) \\
\dot{x}(t) \\
\ddot{x}(t) \\ 
\vdots \\
x^{(n-1)}(t) \\
x^{(n)}(t)
\end{bmatrix} =

\begin{bmatrix}
1& 1 & \ldots & 1 & 1 \\
\lambda_1 & \lambda_2 & \ldots & \lambda_{n-1} & \lambda_n \\
\lambda_1^2 & \lambda_2^2 & \ldots & \lambda_{n-1}^2 & \lambda_n^2 \\ 
\vdots&\vdots&\ddots&\vdots&\vdots \\
\lambda_1^{n-1} & \lambda_{n-1}^2 & \ldots & \lambda_{n-1}^{n-1} & \lambda_n^{n-1} \\
\lambda_1^n & \lambda_2^n & \ldots & \lambda_{n-1}^n & \lambda_n^n \\ 
\end{bmatrix}

\begin{bmatrix}
c_1e^{\lambda_1 t} \\
c_2e^{\lambda_2 t} \\
\vdots \\
c_{n-1}e^{\lambda_{n-1} t} \\
c_ne^{\lambda_n t} 
\end{bmatrix}
$$
And set $t=0$
$$
\underbrace{\begin{bmatrix}
x(0) \\
\dot{x}(0) \\
\ddot{x}(0) \\ 
\vdots \\
x^{(n-1)}(0) \\
x^{(n)}(0)
\end{bmatrix}}_{\mathbf{D}} =

\underbrace{\begin{bmatrix}
1& 1 & \ldots & 1 & 1 \\
\lambda_1 & \lambda_2 & \ldots & \lambda_{n-1} & \lambda_n \\
\lambda_1^2 & \lambda_2^2 & \ldots & \lambda_{n-1}^2 & \lambda_n^2 \\ 
\vdots&\vdots&\ddots&\vdots&\vdots \\
\lambda_1^{n-1} & \lambda_{n-1}^2 & \ldots & \lambda_{n-1}^{n-1} & \lambda_n^{n-1} \\
\lambda_1^n & \lambda_2^n & \ldots & \lambda_{n-1}^n & \lambda_n^n \\ 
\end{bmatrix}}_{\mathbf{M}}

\underbrace{\begin{bmatrix}
c_1 \\
c_2 \\
\vdots \\
c_{n-1} \\
c_n
\end{bmatrix}}_{C}
$$
then we can solve for $\mathbf{C}=\mathbf{M}^{-1}\mathbf{D}$. All the $\lambda_i$ in $\mathbf{M}$ are known from the roots of the [[../../Calculus/Characteristic equation]] as well as the initial conditions $\mathbf{D}$.

The matrix $\mathbf{M}$ is called the [Vandermonde](https://en.wikipedia.org/wiki/Vandermonde_matrix)
