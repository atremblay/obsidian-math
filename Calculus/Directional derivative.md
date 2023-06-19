
> [!summary]
> $$
> \boxed{
> \frac{d u}{d s} = f_x(x_0,y_0,z_0)\cos \alpha +f_y(x_0,y_0,z_0)\cos \beta+f_z(x_0,y_0,z_0) \cos\gamma
> }
> $$
> > [!blank-container|right-small]
> > ![[images/directional_derivative_2D.svg]]
> > Figure 1
>  
> where $\alpha, \beta, \gamma$ are the direction angles that the directed line $PQ$ makes with the positive x-, y- and z-axes, is the directional derivative of $u$ at $P$  and in the direction determined by $\alpha, \beta,\gamma$
> 
> or 
> $$
> \boxed{
> \frac{d u}{d s} = \frac{\vec{s}}{\lVert \vec{s} \lVert}\nabla f(x_0,y_0,z_0)
> }
> $$
> where $\vec{s}$ is the direction we are taking a step towards
> > [!important]- Steepest direction
> > The steepest direction is
> > $$
> > \nabla u = \begin{bmatrix}
> > 
> >     f_x \\
> >    f_y \\
> >    f_z
> > \end{bmatrix}$$
>
>
> > [!Important]- Rate of change is zero
> > If the gradient $\nabla f$ and $\vec{s}$ are perpendicular, the rate of change is zero. It means that taking a step in the direction $\vec{s}$ will yield the same value in $f$
>
> See also [[Gradient Operator]]


Given a function $u=f(x,y)$, what is the rate of change when more than one variable changes at the same time i.e. not only a partial derivative?

Starting with the basic definition of a derivative
$$
\frac{\mathrm{d} u}{\mathrm{d} s} = \lim_{\Delta s \rightarrow 0}\frac{f(x,y) - f(x_0,y_0)}{\Delta s} \tag{1}
$$
Notice the denominator, $\Delta s$. It's not the typical $\Delta x$ or $\Delta y$. It moves in both directions. That's why we take the derivative with respect to $s$. Let's replace $\Delta u = f(x,y) - f(x_0,y_0)$ 
$$
\frac{\Delta u}{\Delta s} = \frac{f(x,y) - f(x_0,y_0)}{\Delta s} \tag{2}
$$
Just like the [[Multivariate chain rule]], we introduce new terms that add up to 0
$$
\frac{f(x,y) - f(x_0,y_0)}{\Delta s} = \frac{\textcolor{orange}{f(x,y) - f(x_0,y)} + \textcolor{cyan}{f(x_0,y) - f(x_0,y_0)}}{\Delta s} \tag{3}
$$
By the [[Mean value theorem|mean value theorem]], 
$$
\textcolor{orange}{f(x,y) - f(x_0,y)} = f_x(x_1,y)\Delta x, \qquad \textcolor{cyan}{f(x_0,y) - f(x_0,y_0)} = f_y(x_0,y_1)\Delta y
$$
where $x_0 < x_1 < x$ and $y_0 < y_1 < y$. Replace these in (3)
$$
\begin{align}
\frac{f(x,y) - f(x_0,y_0)}{\Delta s} &= \frac{f_x(x_1,y)\Delta x +  f_y(x_0,y_1)\Delta y}{\Delta s} \tag{3.1} \\
&= f_x(x_1,y)\textcolor{lightgreen}{\frac{\Delta x}{\Delta s}} + f_y(x_0,y_1)\textcolor{lightblue}{\frac{\Delta y}{\Delta s}} \tag{3.2}
\end{align}
$$

Simple trigonometry tells us that $\frac{\Delta x}{\Delta s} = \cos \alpha$ and $\frac{\Delta y}{\Delta s} = \cos \beta$ (see Figure 1)

$$
\begin{align}
\frac{f(x,y) - f(x_0,y_0)}{\Delta s} 
&= f_x(x_1,y)\textcolor{lightgreen}{\cos \alpha} + f_y(x_0,y_1) \textcolor{lightblue}{\cos \beta} \tag{4}
\end{align}
$$
Following on the [[Mean value theorem|mean value theorem]], as $\Delta s \rightarrow 0$, $x_1 \rightarrow x_0$ and $y_1 \rightarrow y_0$ because $x \rightarrow x_0$ and $y \rightarrow y_0$. $x_1$ and $y_1$ are being squeezed in 

$$
\begin{align}
\frac{\mathrm{d}u}{\mathrm{d} s} &= \lim_{\Delta s \rightarrow 0} \frac{f(x,y) - f(x_0,y_0)}{\Delta s} \\
&= \lim_{\Delta s \rightarrow 0} f_x(x_1,y)\cos \alpha + f_y(x_0,y_1)\cos \beta \\
&= f_x(x_0,y_0)\cos \alpha + f_y(x_0,y_0)\cos \beta \tag{5}
\end{align}
$$
We can generalize from this
$$\frac{\mathrm{d}u}{\mathrm{d}s} = \mathbf{d}^T\nabla f$$
Where $\mathbf{d}$ is a vector of the [[Direction Cosines|direction cosines]] in the direction of $s$.

## Alternative notation
Because the direction of $s$ is linear, it does not matter how long it is, the direction cosines will stay the same. Considering that $s$ is a vector with an $x$ and $y$ component, let's change the notation a little bit

> [!blank-container|right-small]
> ![[images/directional_derivative_2D_vec.svg]]
> > Figure 2
> 

$$\vec{s} = \begin{bmatrix} s_x \\ s_y \end{bmatrix}$$
- What was denoted as $\Delta s$ is the length of the vector $\vec{s}$
- What was denoted as $\Delta x$ is the component $s_x$
- What was denoted as $\Delta y$ is the component $s_y$

Rewriting (5)

$$
\begin{align}
f_x(x_0,y_0)\cos \alpha + f_y(x_0,y_0)\cos \beta &= f_x(x_0,y_0)\frac{\Delta x}{\Delta s } + f_y(x_0,y_0)\frac{\Delta y}{\Delta s} \\
&= f_x(x_0,y_0)\frac{s_x}{\lVert \vec{s} \lVert} + f_y(x_0,y_0)\frac{s_y}{\lVert \vec{s} \lVert} \\
&= \frac{1}{\lVert \vec{s} \lVert}\left(f_x(x_0,y_0) s_x + f_y(x_0,y_0) s_y \right) \\
&= \frac{1}{\lVert \vec{s} \lVert}
    \begin{bmatrix}s_x  & s_y \end{bmatrix}
    \begin{bmatrix}f_x(x_0,y_0)   \\  f_y(x_0,y_0) \end{bmatrix} \\
&= \frac{\vec{s}^T}{\lVert \vec{s} \lVert}
\nabla f
\end{align}
$$

## Maximum direction

> [!blank-container|left-medium]
> <span class='centerImg'>![[directional_derivative.svg|400]]</span>

An interesting manipulation is to find what is the steepest direction. We are using the case where $u=f(x,y,z)$, three variables (6) instead of two like the above (5).
$$
\begin{align}
\frac{d u}{d s} &= f_x(x_0,y_0,z_0)\cos \alpha +f_y(x_0,y_0,z_0)\cos \beta+f_z(x_0,y_0,z_0) \cos\gamma \tag{6} \\
&= \sqrt{f_x^2 + f_y^2+f_z^2} \left\{\frac{ f_x}{\sqrt{f_x^2 + f_y^2+f_z^2}}\cos \alpha +\frac{f_y}{{\sqrt{f_x^2 + f_y^2+f_z^2}}}\cos \beta+\frac{f_z}{\sqrt{f_x^2 + f_y^2+f_z^2}} \cos\gamma \right\}
\end{align}
$$

The coefficients of the [[Direction Cosines|direction cosines]] $\cos\alpha, \cos\beta,\cos\gamma$ can be seen as the direction cosine of another line, the one defined by the angles
$$\frac{ f_x}{\sqrt{f_x^2 + f_y^2+f_z^2}}, \frac{ f_y}{\sqrt{f_x^2 + f_y^2+f_z^2}}, \frac{ f_z}{\sqrt{f_x^2 + f_y^2+f_z^2}}$$

This is the case because of the [[Direction Cosines#Properties|properties]]
$$
\cos^2 \alpha + \cos^2 \beta + \cos ^2 \gamma = 1
$$
and that
$$
\left( 
    \frac{ f_x}{\sqrt{f_x^2 + f_y^2+f_z^2}}
\right)^2+ 
\left(
    \frac{ f_y}{\sqrt{f_x^2 + f_y^2+f_z^2}}
\right)^2 + 
\left(
    \frac{ f_z}{\sqrt{f_x^2 + f_y^2+f_z^2}}
\right)^2 = 1
$$

Now, based on the [[Angle between two lines#Parallel|parallel]] property,$\frac{\partial u }{\partial s}$ is maximal when both lines point in the same direction.   

$$
\begin{align}
\frac{d u}{d s} &= f_x\cos \alpha +f_y\cos \beta+f_z \cos\gamma \\
&= \sqrt{f_x^2 + f_y^2+f_z^2} \left\{\frac{ f_x}{\sqrt{f_x^2 + f_y^2+f_z^2}}\cos \alpha +\frac{f_y}{{\sqrt{f_x^2 + f_y^2+f_z^2}}}\cos \beta+\frac{f_z}{\sqrt{f_x^2 + f_y^2+f_z^2}} \cos\gamma \right\} \\
&=\sqrt{f_x^2 + f_y^2+f_z^2}
\end{align}
$$

In other words, when the derivative is in the direction of the partial derivative 
$$
\begin{align}
\cos\alpha &= \frac{ f_x}{\sqrt{f_x^2 + f_y^2+f_z^2}} \propto f_x \\
\cos\beta &= \frac{ f_y}{\sqrt{f_x^2 + f_y^2+f_z^2}} \propto f_y \\
\cos\gamma &= \frac{ f_z}{\sqrt{f_x^2 + f_y^2+f_z^2}} \propto f_z
\end{align}
$$
we get the maximum value of $\sqrt{f_x^2 + f_y^2+f_z^2}$. That's why doing the a gradient descent in the direction of $\nabla u$ is the step that will bring us the fastest to a minimum.


