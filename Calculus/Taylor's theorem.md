---
aliases:
    - "Taylor series"
    - "taylor series"
---

> [!Summary]
> **Single variable**
> $$
> f(x)=\sum_{i=0}^n \frac{(x-a)^nf^{(n)}(a)}{n!} + R
> $$
> Where the function is evaluated at point $a$ and estimated at point $x$.
> If $x=a + \Delta a$, then another way to write it is
> $$
> f(a+\Delta a)=\sum_{i=0}^n \frac{(\Delta a)^nf^{(n)}(a)}{n!} + R
> $$
> 
> **Multi variables**
> It's typical to stop at the Hessian
> $$
> f(\textbf{x})= f(\textbf{a}) + \nabla f(\textbf{a})[\textbf{x}-\textbf{a}] + \frac 1 2 [\textbf{x}-\textbf{a}]^T H [\textbf{x}-\textbf{a}] + R
> $$
> 
> Where $\bf{x}$ is a vector of independant variables, $\bf{a}$ is the vector where the derivatives are evaluated and $H$ is the Hessian of $f(\bf{a})$.
> 

^627630


# Single variable
## Approximation

We may need to approximate a function $f(x)$ by another, simpler, one $g(x)$. Oftentimes to get an easier derivative or integral. This other function can be a polynomial of degree $n$
$$g(x)=\textcolor{orange}{c_0}+\textcolor{cyan}{c_1}x+\textcolor{lime}{c_2}x^2+\textcolor{violet}{c_3}x^3+\ldots+c_nx^n$$

The goal is to get $g(x)$ as close as possible to $f(x)$, $g(x) \approx f(x)$ by finding good values for $c_i$. This polynomial is a convenient one, it could be another function. It just has nice proporties. 

## Coefficients

We need to find values for $c_i$. The walkthrough always starts by estimating the functions at $x=0$ (aka [[Maclaurin Series]]. It's convenient because it will cancel many terms. We would like to have

$$
g(0) = c_0 + c_10 + c_20^2+c_30^3+\ldots+c_n0^n = \textcolor{orange}{c_0 = f(0)}
$$

This is the first $\textcolor{orange}{\text{coefficient}}$ 

$$g(x)=\textcolor{orange}{f(0)}+c_1x+c_2x^2+\ldots+c_nx^n$$

Secondly, we would like the first derivatives of the two functions to be equal at $x=0$ 

$$
\begin{align}
g'(x)&=\textcolor{cyan}{c_1}+2c_2x+3c_3x^2+\ldots+nc_nx^{n-1} \\
g'(0)&=\textcolor{cyan}{c_1}+2c_20+3c_30^2+\ldots+nc_n0^{n-1} = \textcolor{cyan}{c_1=f'(0)}
\end{align}
$$

We get the value of $\textcolor{cyan}{c_1}$ and fill it in.
$$g(x)=\textcolor{orange}{f(0)}+\textcolor{cyan}{f'(0)}x+c_2x^2+c_3x^3+\ldots+c_nx^n$$

One more time

$$
\begin{align}
g''(x)&=\textcolor{lime}{2c_2}+3 \cdot 2c_3x+\ldots+n(n-1)c_nx^{n-2} \\
g''(0)&=\textcolor{lime}{2c_2}+2c_20+\ldots+nc_n0^{n-1} = \textcolor{lime}{2c_2=f''(0)} \\
\iff \textcolor{lime}{c_2} &= \textcolor{lime}{\frac{f''(0)}{2}}

\end{align}
$$

Fill it in
$$g(x)=\textcolor{orange}{f(0)}+\textcolor{cyan}{f'(0)}x+\textcolor{lime}{\frac{f''(0)}{2}}x^2+c_3x^3+\ldots+c_nx^n$$

And again to drill this in

$$
\begin{align}
g^{(3)}(x)&=\textcolor{violet}{3 \cdot 2 c_3}+\ldots+n(n-1)(n-2)c_nx^{n-3} \\
g^{(3)}(0)&=\textcolor{violet}{3 \cdot 2 c_3}+\ldots+nc_n0^{n-2} = \textcolor{violet}{3\cdot2c_2=f^{(3)}(0)} \\
\iff \textcolor{violet}{c_3} &= \textcolor{violet}{\frac{f^{(3)}(0)}{3\cdot2}}

\end{align}
$$

Fill it in
$$g(x)=\textcolor{orange}{f(0)}+\textcolor{cyan}{f'(0)}x+\textcolor{lime}{\frac{f''(0)}{2!}}x^2+\textcolor{violet}{\frac{f^{(3)}(0)}{3!}}x^3+\ldots+c_nx^n$$

We start noticing that the denominator is a factorial.

Do this $n$ times and you get


$$g(x)=\sum_{i=0}^n \frac{f^{(i)}(0)x^i}{i!}$$


Repeat after me, $g(x)$ is **not** a replacement for $f(x)$, it's an approximation. We found coefficients $c_i$ such that when evaluated around $x=0$ then the approximation is, hopefully, decent. We can stil evaluate for any values of $x$, but there is a good chance that the error $\left(f(x)-g(x)\right)$ is large.

We evaluated at $x=0$ because it was convenient, it would turn many terms to 0. We are not limited to that, we could have evaluated at $x=a$, but it would have turned ugly fast.

$$
\begin{align}
g(a) &= c_0 + c_1a + c_2a^2+c_3a^3+\ldots+c_na^n = f(a) \\
g'(a) &= c_1 + 2c_2a+3c_3a^2+\ldots+nc_na^{n-1} = f'(a) \\
&\ldots \\
g^{(n)}(a) &= (n!)c_n = f^{(n)}(a) \\
\end{align}
$$

Try to find the value of the coefficients, it's a nightmare. The very last coefficient is available, but try to work this backwards and it becomes an absolute mess.

## Generalization

Let's change $g(x)$ to be a different approximation function

$$g(x)=c_0+c_1(x-a)+c_2(x-a)^2+c_3(x-a)^3+\ldots+c_n(x-a)^n$$

It's still possible to do the same thing as above with the derivatives

$$
\begin{align}
g(x)&=c_0+c_1(x-a)+c_2(x-a)^2+c_3(x-a)^3+\ldots+c_n(x-a)^n \\
g'(x)&=c_1+2c_2(x-a)+3c_3(x-a)^2+\ldots+nc_n(x-a)^{n-1} \\
&\ldots \\
g^{(n)}(x) &= (n!)c_n  \\
\end{align}
$$

evaluating all of the above at $x=a$ and matching the derivatives with the derivatives of the original function

$$
\begin{align}
g(a)&=c_0+c_1(a-a)+c_2(a-a)^2+c_3(a-a)^3+\ldots+c_n(a-a)^n &= f(a)\\
g'(a)&=c_1+2c_2(a-a)+3c_3(a-a)^2+\ldots+nc_n(a-a)^{n-1}&= f'(a) \\
g''(a)&=2c_2+3\cdot2c_3(a-a)+\ldots+n(n-1)c_n(a-a)^{n-2}&= f''(a) \\
&\ldots \\
g^{(n)}(a) &= (n!)c_n &= f^{(n)}(a)  \\
\end{align}
$$

We find that the coeffcients are $c_i=\frac{f^{(i)}(a)}{i!}$, so the approximation function can be written as

$$g(x)=\sum_{i=0}^n \frac{f^{(i)}(a)(x-a)^i}{i!}$$

Same expectations apply. Around $a$ we expect that $g(x)$ will be a good approximator. The more terms you add to the polynomial, the better the approximation will be, generally, the further away from $a$ you can go.


# Multivariable 
This is surprisingly easy to do the same thing for a multivariate function $f: \mathbb{R}^n \rightarrow \mathbb{R}$

## Approximation
Let's do it for 2 dimensions only and stop at the second degree polynomial 
$$
g(x,y) = c_0 + c_1x + c_2y + c_3xy +c_4x^2 + c_5y^2 
$$

We need to find the values of the coefficients $c_i$. The strategy is still to match the $n^{th}$ derivatives of $g(x,y)$

$$
\begin{align}
g(x,y)      &= &c_0 &+ & c_1x &+ & c_2y &+ & c_3xy  &+ & c_4x^2 &+ & c_5y^2 \\
g_x(x,y)    &= &    &  & c_1  &+ &      &  & c_3y   &+ & 2c_4x  &  &        \\
g_y(x,y)    &= &    &  &      &  & c_2  &+ & c_3x   &+ &        &  & 2c_5y  \\
g_{xy}(x,y) &= &    &  &      &  &      &  & c_3    &  &        &  &        \\
g_{xx}(x,y) &= &    &  &      &  &      &  &        &  & 2c_4   &  &        \\
g_{yy}(x,y) &= &    &  &      &  &      &  &        &  &        &  & 2c_5   \\
\end{align}
$$

at the origin with the $n^{th}$ derivatives of the original function $f(x,y)$

$$
\begin{align}
g(0,0)      &= &c_0 &  &      &  &      &  &        &  &        &  &       &= f(0,0)      \\
g_x(0,0)    &= &    &  & c_1  &  &      &  &        &  &        &  &       &= f_x(0,0)    \\
g_y(0,0)    &= &    &  &      &  & c_2  &  &        &  &        &  &       &= f_y(0,0)    \\
g_{xy}(0,0) &= &    &  &      &  &      &  & c_3    &  &        &  &       &= f_{xy}(0,0) \\
g_{xx}(0,0) &= &    &  &      &  &      &  &        &  & 2c_4   &  &       &= f_{xx}(0,0) \\
g_{yy}(0,0) &= &    &  &      &  &      &  &        &  &        &  & 2c_5  &= f_{yy}(0,0) \\
\end{align}
$$

which gives us the coefficients 
$$
\begin{align}
c_0 &= f(0,0)      \\
c_1 &= f_x(0,0)    \\
c_2 &= f_y(0,0)    \\
c_3 &= f_{xy}(0,0) \\
c_4 &= \frac{f_{xx}(0,0)}{2} \\
c_5 &= \frac{f_{yy}(0,0)}{2} \\
\end{align}
$$

Plug them in the approximation function
$$
\begin{align}
g(x,y) &= f(0,0) + f_x(0,0)x + f_y(0,0)y + f_{xy}(0,0)xy + \frac{f_{xx}(0,0)}{2}x^2 + \frac{f_{yy}(0,0)}{2}y^2  \\
&= f(0,0) + \begin{bmatrix} x \\ y \\ \end{bmatrix}^T \begin{bmatrix} f_x \\ f_y \\ \end{bmatrix}
          + \frac 1 2 \begin{bmatrix} x \\ y \\ \end{bmatrix}^T 
                      \begin{bmatrix} f_{xx} & f_{xy} \\ f_{xy} & f_{yy} \\ \end{bmatrix}
                      \begin{bmatrix} x \\ y \\ \end{bmatrix} \\

&= f(0,0) + \textbf{x}\nabla f(0,0) + \textbf{x}^T H \textbf{x}
\end{align}
$$

where $H$ is the Hessian.

## Approximation at another point

It's easy to redo those steps but with the derivatives evaluated at $(a,b)$ 

$$
\begin{align}
g(x,y)      &= &c_0 &+ & c_1(x-a) &+ & c_2(y-b) &+ & c_3(x-a)(y-b)  &+ & c_4(x-a)^2 &+ & c_5(y-b)^2 \\
g_x(x,y)    &= &    &  & c_1      &+ &      &  & c_3(y-b)       &+ & 2c_4(x-a)  &  &            \\
g_y(x,y)    &= &    &  &          &  & c_2  &+ & c_3(x-a)       &+ &            &  & 2c_5(y-b)  \\
g_{xy}(x,y) &= &    &  &          &  &      &  & c_3            &  &            &  &            \\
g_{xx}(x,y) &= &    &  &          &  &      &  &                &  & 2c_4       &  &            \\
g_{yy}(x,y) &= &    &  &          &  &      &  &                &  &            &  & 2c_5       \\
\end{align}
$$

at the point $(a,b)$ with the $n^{th}$ derivatives of the original function $f(x,y)$

$$
\begin{align}
g(a,b)      &= &c_0 &  &      &  &      &  &        &  &        &  &       &= f(a,b)      \\
g_x(a,b)    &= &    &  & c_1  &  &      &  &        &  &        &  &       &= f_x(a,b)    \\
g_y(a,b)    &= &    &  &      &  & c_2  &  &        &  &        &  &       &= f_y(a,b)    \\
g_{xy}(a,b) &= &    &  &      &  &      &  & c_3    &  &        &  &       &= f_{xy}(a,b) \\
g_{xx}(a,b) &= &    &  &      &  &      &  &        &  & 2c_4   &  &       &= f_{xx}(a,b) \\
g_{yy}(a,b) &= &    &  &      &  &      &  &        &  &        &  & 2c_5  &= f_{yy}(a,b) \\
\end{align}
$$

which gives us the coefficients 
$$
\begin{align}
c_0 &= f(a,b)      \\
c_1 &= f_x(a,b)    \\
c_2 &= f_y(a,b)    \\
c_3 &= f_{xy}(a,b) \\
c_4 &= \frac{f_{xx}(a,b)}{2} \\
c_5 &= \frac{f_{yy}(a,b)}{2} \\
\end{align}
$$

Plug them in the approximation function
$$
\begin{align}
g(x,y) &= f(a,b) + f_x(a,b)x + f_y(a,b)y + f_{xy}(a,b)xy + \frac{f_{xx}(a,b)}{2}x^2 + \frac{f_{yy}(a,b)}{2}y^2  \\
&= f(a,b) + \begin{bmatrix} x \\ y \\ \end{bmatrix}^T \begin{bmatrix} f_x \\ f_y \\ \end{bmatrix}
          + \frac 1 2 \begin{bmatrix} x \\ y \\ \end{bmatrix}^T 
                      \begin{bmatrix} f_{xx} & f_{xy} \\ f_{xy} & f_{yy} \\ \end{bmatrix}
                      \begin{bmatrix} x \\ y \\ \end{bmatrix} \\

&= f(a,b) + \textbf{x}\nabla f(a,b) + \textbf{x}^T H \textbf{x}
\end{align}
$$

## Multivariate Generalization

Make it more general with vector notation for the independant variables $\textbf{x}$ and the point at which the derivative is evaluated $\textbf{a}$
$$
f(\textbf{x})\approx g(\textbf{x})= f(\textbf{a}) + [\textbf{x}-\textbf{a}]^T\nabla f(\textbf{a}) + \frac 1 2 [\textbf{x}-\textbf{a}]^T H [\textbf{x}-\textbf{a}]
$$
A notation often encountered is writing $\mathbf{x}=\mathbf{a}+\mathbf{\Delta x}$. $\mathbf{x}$ is just $\mathbf{a}$ plus a delta.

$$
\begin{align}
f(\textbf{x})\approx g(\textbf{x})&= f(\textbf{a}) + \nabla f(\textbf{a})[\textbf{x}-\textbf{a}] + \frac 1 2 [\textbf{x}-\textbf{a}]^T H [\textbf{x}-\textbf{a}] \\
&=f(\mathbf{a}) + \nabla f(\textbf{a})[\mathbf{x-(x - \Delta x)}] + \frac 1 2 [\mathbf{x-(x - \Delta x)}]^T H [\mathbf{x-(x - \Delta x)}] \\
&=f(\mathbf{a}) + \nabla f(\textbf{a})\mathbf{ \Delta x} + \frac 1 2 \mathbf{\Delta x}^T H \mathbf{\Delta x}
\end{align}
$$

