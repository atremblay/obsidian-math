
$$
a = \dot{v} = \ddot{x} = -kx
$$

At $t=0$ we have an initial position $x(0)=x_0$ and an initial velocity $v(0)=\dot{x}(0) = v_0$   

# Method 1 - [[Const Coeff Homo DE]]

In the constant coefficient homogeneous differential equation 
$$ax'' + bx' + cx=0$$
we pretty much always guess $x=e^{rt}$ where the unknown $r$s are the roots
$$ r = \frac{-b \pm \sqrt{b^2-4ac}}{2a} $$
Here the DE is
$$ \ddot{x} + kx = 0 $$

With $a=1$, $b=0$ and $c=k$, we have a [[Const Coeff Homo DE#Case 3: Complex pair of roots|complex pair of roots]].

$$
r = \frac{ \pm \sqrt{-4k}}{2} = \pm i\sqrt{k}
$$
The real part $\alpha=0$ and the imaginary part $\beta=\sqrt{k}$ . The [[Const Coeff Homo DE#General solution|the two general solutions]] are

$$
x^{(1)}=e^{\alpha t}\cos{\beta t}=e^{0}\cos{t\sqrt{k}} = \cos{\left(t\sqrt{k}\right)}
$$
and
$$
x^{(2)}=e^{\alpha t}\sin{\beta t}=e^{0}\sin{t\sqrt{k}} = \sin{\left(t\sqrt{k}\right)}
$$
The solution is  the combination of both by the [[Superposition]] principle

$$
x = c_1\cos{\left(t\sqrt{k}\right)}+c_2\sin{\left(t\sqrt{k}\right)}
$$

If we have initial conditions for the initial position $x(0)=x_0$ 
$$
\begin{align}
x(0) &= c_1\cos{\left(0\sqrt{k}\right)}+c_2\sin{\left(0\sqrt{k}\right)}\\
&= c_1\cos{0}+c_2\sin{0}\\
&= c_1 \\
&= x_0
\end{align}
$$

and velocity $v(0)=\dot{x}(0)=v_0$ (don't forget the [[Chain Rule]])
$$
\begin{align}
v(0)=\dot{x}(0) &= -c_1\sin{\left(0\sqrt{k}\right)}\sqrt{k}+c_2\cos{\left(0\sqrt{k}\right)}\sqrt{k}\\
&= -c_1\sin{0}\sqrt{k}+c_2\cos{0}\sqrt{k}\\
&= c_2\sqrt{k}\\
&= v_0\\
\end{align}
$$
$$
\boxed{x = x_0\cos{\left(t\sqrt{k}\right)}+\frac{v_0}{\sqrt{k}}\sin{\left(t\sqrt{k}\right)}}
$$

# Method 2 - Power Series

Use polynomial expansion like in the [[Taylor's theorem|taylor series]]. 

$$
\begin{align}
x(t) &= \textcolor{orange}{c_0} + &\textcolor{cyan}{c_1t}& + &\textcolor{violet}{c_2t^2}& + &\textcolor{olive}{c_3t^3}& + &c_4t^4& + &c_5 t^5& + \ldots \\
\dot{x}(t) &= &c_1&+ &2c_2t& + &3c_3t^2& + &4c_4t^3& + &5 c_5 t^4& + \ldots \\
\ddot{x}(t) &=&& &\textcolor{orange}{2c_2}& + &\textcolor{cyan}{3\cdot2\cdot c_3t}& + &\textcolor{violet}{4\cdot3\cdot c_4t^2}& + &\textcolor{olive}{5\cdot4\cdot c_5 t^3}&+ \ldots
\end{align}
$$

With the help of the initial values we can find $c_0$ and $c_1$ 

$$
\begin{align}
x(0) &= c_0 = x_0 \\
\dot{x}(0) &= c_1 = v_0 \\
\end{align}
$$

Match the coefficients of the powers for $\ddot{x} = -kx$. Two polynomials are equal only if their coefficients are the same. Match a few to figure out the pattern. 
$$
\begin{align}
\textcolor{orange}{2c_2} &=  \textcolor{orange}{-kc_0}= -kx_0 \\
\iff c_2 &= \frac{-kx_0}{2}
\end{align}
$$

$$
\begin{align}
\textcolor{cyan}{3 \cdot 2 \cdot c_3} &=  \textcolor{cyan}{-kc_1}= -kv_0 \\
\iff c_3 &= \frac{-kv_0}{3!}
\end{align}
$$

$$
\begin{align}
\textcolor{violet}{4 \cdot 3 \cdot c_4} &=  \textcolor{violet}{-kc_2}= \frac{k^2x_0}{2} \\
\iff c_4 &= \frac{k^2x_0}{4!}
\end{align}
$$
$$
\begin{align}
\textcolor{olive}{5 \cdot 4 \cdot c_5} &=  \textcolor{olive}{-kc_3}= \frac{k^2v_0}{3!} \\
\iff c_5 &= \frac{k^2x_0}{5!}
\end{align}
$$

Plug these coefficients into the power series
$$
\begin{align}
x(t) &= c_0 + c_1t + c_2t^2 + c_3t^3 + c_4t^4 + c_5 t^5 + \ldots \\
&= x_0 + v_0t -\frac{kx_0}{2!}t^2 -\frac{kv_0}{3!}t^3 + \frac{k^2x_0}{4!}t^4 + \frac{k^2v_0}{5!}t^5 + \ldots \\
&= x_0\left(1-\frac{k}{2!}t^2+ \frac{k^2}{4!}t^4 + \ldots\right) + v_0\left(t  -\frac{k}{3!}t^3  + \frac{k^2}{5!}t^5 + \ldots\right) \\
&= x_0\left(1-\frac{k}{2!}t^2+ \frac{k^2}{4!}t^4 + \ldots\right) + \frac{v_0}{\sqrt{k}}\left(t\sqrt{k}  -\frac{k^{\frac 3 2}}{3!}t^3  + \frac{k^{\frac 5 2}}{5!}t^5 + \ldots\right) \\
&= x_0\left(1-\frac{k}{2!}t^2+ \frac{k^2}{4!}t^4 + \ldots\right) + \frac{v_0}{\sqrt{k}}\left(t\sqrt{k}  -\frac{(t\sqrt{k})^3 }{3!}  + \frac{(t\sqrt{k})^5}{5!} + \ldots\right) \\
&= x_0\cos\left(t\sqrt{k}\right)+\frac{v_0}{\sqrt{k}}\sin\left(t\sqrt{k}\right)
\end{align}
$$


# Method 3 - System of first order DE
Turn this into a system of first order differential equation

$$
\begin{bmatrix}
\dot{x} \\
\dot{v}
\end{bmatrix} = 
\begin{bmatrix}
v \\
-kx
\end{bmatrix}= 
\begin{bmatrix}
0 & 1\\
-k & 0
\end{bmatrix}
\begin{bmatrix}
x\\
v
\end{bmatrix}
$$

Change this to a vector notation with
$$
\textbf{x}= \begin{bmatrix}
x\\
v
\end{bmatrix}, \hspace{2em}
\dot{\textbf{x}}= \begin{bmatrix}
\dot{x}\\
\dot{v}
\end{bmatrix}
$$
$$
\dot{\textbf{x}}= A \textbf{x}
$$
