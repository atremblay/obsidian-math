
> [!Summary]
> $$
> \boxed{
> \frac{\delta u}{\delta s} =
> \left[\begin{matrix} 
> f_x(x, y, z) & f_y(x, y, z) & \cdots & f_z(x, y, z)
> \end{matrix}\right] 
> \left[\begin{matrix} 
> \frac{\delta x}{\delta s} \\ 
> \frac{\delta y}{\delta s} \\
> \cdots \\
> \frac{\delta z}{\delta s}
> \end{matrix}\right] 
> }
> $$

This shows how to extend the concept of [[Derivative/Chain Rule]] to multiple variables.

With function compositions like $u=f(x,y)$ where $x$ and $y$ are functions of other variables $x=g(s,t)$, $y=h(s,t)$, changing $s$ or $t$ will result in changes in $x$ and $y$ and those will affect $u$.

A small modification in $s$:

$$x + \Delta x = g(s + \Delta s, t)$$

$$y + \Delta y = h(s + \Delta s, t)$$

$$\implies u + \Delta u = f(x + \Delta x, y + \Delta y)$$

The standard method of increment is still valid

$$\frac{\Delta u}{\Delta s}=\frac{f(x + \Delta x, y + \Delta y) - f(x,y)}{\Delta s}$$

Now the idea is to add a substract terms to fall back on [[Partial derivative|partial derivates]]

$$\frac{\Delta u}{\Delta s}=\frac{f(x + \Delta x, y + \Delta y) - f(x, y + \Delta y) + f(x, y + \Delta y) - f(x,y)}{\Delta s}$$

$$=\frac{f(x + \Delta x, y + \Delta y) - f(x, y + \Delta y) }{\Delta s} + \frac{f(x, y + \Delta y) - f(x,y)}{\Delta s}$$





##### Method 1

Not entirely certain if this method is alright. It uses the fact that $\Delta s \to 0$ in different places at different times. I think it's okay, but look at method 2 for something that came from the book.

$$\frac{\Delta u}{\Delta s}=\frac{f(x + \Delta x, y + \Delta y) - f(x, y + \Delta y) }{\Delta s}\frac{\Delta x}{\Delta x} + \frac{f(x, y + \Delta y) - f(x,y)}{\Delta s}\frac{\Delta y}{\Delta y}$$

$$=\frac{f(x + \Delta x, y + \Delta y) - f(x, y + \Delta y) }{\Delta x}\frac{\Delta x}{\Delta s} + \frac{f(x, y + \Delta y) - f(x,y)}{\Delta y}\frac{\Delta y}{\Delta s}$$

We have to look at this considering that $\lim\limits_{\Delta s \to 0}$. As $\Delta s \to 0$, so does $\Delta x \to 0$. Same thing for $\Delta y$.

- $\frac{f(x + \Delta x, y + \Delta y) - f(x, y + \Delta y) }{\Delta x}$ is the definition of the partial derivate of $f$ w.r.t. $x$ at the point $(x, y + \Delta y)$

- $\frac{f(x, y + \Delta y) - f(x,y)}{\Delta y}$ is the definition of the partial derivate of $f$ w.r.t. $y$ at the point $(x, y)$

$$\frac{\Delta u}{\Delta s}=f_x(x, y + \Delta y)\frac{\Delta x}{\Delta s} + f_y(x, y)\frac{\Delta y}{\Delta s}$$

Again, as $\Delta s \to 0$ so does $\Delta x$ and $\Delta y$. That means that $f_x(x, y + \Delta y) \to f_x(x, y)$

$$\frac{\delta u}{\delta s}=f_x(x, y)\frac{\delta x}{\delta s} + f_y(x, y)\frac{\delta y}{\delta s}$$

##### Method 2

This uses the [[Mean value theorem|mean value theorem]].

$$\frac{\Delta u}{\Delta s}=\frac{f(x + \Delta x, y + \Delta y) - f(x, y + \Delta y) }{\Delta s} + \frac{f(x, y + \Delta y) - f(x,y)}{\Delta s}$$
$$=f_x(x_1, y + \Delta y)\frac{\Delta x}{\Delta s} + f_y(x, y_1)\frac{\Delta y}{\Delta s}$$

Where $x<x_1<x+\Delta x$ and  $y<y_1<y+\Delta y$ 

$$\lim\limits_{\Delta s \to 0}\frac{\Delta u}{\Delta s}=\frac{\delta u}{\delta s}=f_x(x, y)\frac{\delta x}{\delta s} + f_y(x, y)\frac{\delta y}{\delta s}$$

Because as $\Delta s \to 0$ so does $\Delta x$ and $\Delta y$. That means that $x_1 \to x$ because $x_1$ gets squished more and more between the lower and upper bounds. Same thing for $y_1$.

It also means that $f_x(x, y + \Delta y) \to f_x(x, y)$

$$
\frac{\delta u}{\delta s} =
\left[\begin{matrix} 
f_x(x, y) & f_y(x, y)
\end{matrix}\right] 
\left[\begin{matrix} 
\frac{\delta x}{\delta s} \\ \frac{\delta y}{\delta s}
\end{matrix}\right] 
$$

### More than 2 variables


$u=f(x,y,z)$, $x=g(s,t)$, $y=h(s,t)$, $z=k(s,t)$

The process is the same, it's just about finding the right stuff to add and subtract so we can fall back on partial derivatives.

$$\frac{\Delta u}{\Delta s}=\frac{f(x + \Delta x, y + \Delta y, z + \Delta z) - f(x,y,z)}{\Delta s}$$

$$\frac{\Delta u}{\Delta s}=\frac{f(x + \Delta x, y + \Delta y, z + \Delta z) - f(x, y + \Delta y, z + \Delta z) + f(x, y + \Delta y, z + \Delta z) - f(x, y, z + \Delta z) + f(x, y, z + \Delta z) - f(x,y)}{\Delta s}$$

$$=\frac{f(x + \Delta x, y + \Delta y, z + \Delta z) - f(x, y + \Delta y, z + \Delta z)}{\Delta s} + \frac{f(x, y + \Delta y, z + \Delta z) - f(x, y, z + \Delta z)}{\Delta s} + \frac{f(x , y , z + \Delta z) - f(x, y, z)}{\Delta s}$$

$$=f_x(x, y, z)\frac{\delta x}{\delta s} + f_y(x, y, z)\frac{\delta y}{\delta s} + f_z(x, y, z)\frac{\delta z}{\delta s}$$