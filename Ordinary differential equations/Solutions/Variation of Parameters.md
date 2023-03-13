
---
tags:
    - "solution"
    - "second_order"
---

Can we drop the [[Undetermined Coefficient#Constraints|second constraint]]

$$y''-p(x)y'-q(x)y=f(x)$$

$f(x)$ is a continuous function of $x$ on an interval I and is $\not\equiv 0$ on I. 
This method can still be used if $f(x)$ has a finite number of linearly independant derivatives, but [[Undetermined Coefficient]] would still be easier. But with this method there is no need to concern ourselves with the different [[Undetermined Coefficient#Cases|cases]]. 

# Methodology
Easier to explain with a second order generic DE

$$f_2(x)y''+f_1(x)y'+f_0(x)y=Q(x)$$


#### Step 1: [[Complementary function]]
We need to have the [[Complementary function|complementary function]] 
of the equation above. If the $f_i(x)$ are not constants then it will often be very difficult to get the $n$ independant solutions.  If they are constants then we know a few ways (e.g. [[Undetermined Coefficient]]). For the rest we will do as if $f_i(x)$ are constants. 

First solve the homogeneous version
$$a_2y''+a_1y'+a_0y=0$$
With $a_2\neq 0$

And find the solutions $y_1$ and $y_2$, giving the [[Complementary function|complementary function]]
$$y_h(x)=c_1y_1(x)+c_2y_2(x)$$

#### Step 2: Calculate the derivatives
This solution is for the homogeneous version of the DE and we are looking for a particular solution the the non-homogenous. So we make the **variation of  parameters**; changing the constant $c_1$ and $c_2$ to be unknown functions of $x$ instead: $u_1(x)$ and $u_2(x)$ . These terms vary as opposed to being constants.

$$y_p=u_1(x)y_1(x) + u_2(x)y_2(x)$$

Successive derivatives

$$
\begin{align}
y_p'&=u_1y_1'+u_1'y_1+u_2y_2'+u_2'y_2 \\
&=(u_1y_1'+u_2y_2') + (u_1'y_1+u_2'y_2) \\
y_p''&=(u_1y_1''+u_2y_2'') + (u_1'y_1'+u_2'y_2') +(u_1'y_1+u_2'y_2)'
\end{align}
$$
Plugging these derivatives in the DE will give us conditions that we will have to respect in order have a particular solution. 

#### Step 3: Use everything

$$
\begin{align}
a_2y''+a_1y'+a_0y&=a_2\left((u_1y_1''+u_2y_2'') + (u_1'y_1'+u_2'y_2') +(u_1'y_1+u_2'y_2)'\right) + a_1\left((u_1y_1'+u_2y_2') + (u_1'y_1+u_2'y_2)\right)+a_0(u_1y_1+u_2y_2) \\
&=u_1(\cancelto{0}{a_2y_1''+a_1y_1'+a_0y_1})+u_2(\cancelto{0}{a_2y_2''+a_1y_2'+a_0y_2})+a_2\left((u_1'y_1'+u_2'y_2')+(u_1'y_1+u_2'y_2)'\right)+a_1(u_1'y_1+u_2'y_2)

\end{align}
$$
If we set the condition that $u_1'y_1+u_2'y_2=0$, then
$$
\begin{align}
a_2y''+a_1y'+a_0y&=a_2\left((u_1'y_1'+u_2'y_2')+(\cancelto{0}{u_1'y_1+u_2'y_2})'\right)+a_1(\cancelto{0}{u_1'y_1+u_2'y_2}) \\
&=a_2(u_1'y_1'+u_2'y_2') \\
&= Q(x)

\end{align}
$$
We now have two equations and two unknowns
$$
\begin{align}
u_1'y_1+u_2'y_2&=0 \\
u_1'y_1'+u_2'y_2'&=\frac{Q(x)}{a_2}
\end{align}
$$
[[Solve equations|Solutions]] are
$$
u_1'=\frac{
    \left| {
        \begin{array}{cc}
            0 & y_2 \\
            \frac{Q(x)}{a_2}  & y_2' \\
        \end{array} } 
        \right|
    }{
    \left| {
        \begin{array}{cc}
            y_1 & y_2 \\
            y_1'  & y_2' \\
        \end{array} } 
        \right|
    },\hspace{2em}

u_2'=\frac{
    \left| {
        \begin{array}{cc}
            y_1 & 0 \\
            y_1' & \frac{Q(x)}{a_2} \\
        \end{array} } 
        \right|
    }{
    \left| {
        \begin{array}{cc}
            y_1 & y_2 \\
            y_1'  & y_2' \\
        \end{array} } 
        \right|
    }
$$

The denominator in both is the [[Lin. Ind. of Func#Wronskian|Wronskian]].

$$u_1(x)=\int u_1'(x)dx, \hspace{2em}u_2(x)=\int u_2'(x)dx$$
Plug these in the particular solution $y_p$ and finally get the [[general solution]]
$$
\begin{align}
y&=y_h+y_p \\
&=c_1y_1(x)+c_2y_2(x)+u_1(x)y_1(x)+u_2(x)y_2(x)
\end{align}
$$
Up to our ability to integrate, this always works if we have $y_1$ and $y_2$.



> [!Example]-
> $$y''+y=\tan(x)$$
> 
> ##### Step 1: Solution to the homogeneous version
> First solve the homogeneous version of this equation. It has constant coefficient, so we can use the technique for [[Const Coeff Homo DE|constant coefficient homogeneous differential equation]] to find $y_1$ and $y_2$.
> 
> -----
> $$y''+y=0$$
> Guess: $y=e^{rt}$
> Then $y''=r^2e^{rt},\hspace{0.2em}y'=re^{rt}$
> $$
> \begin{align}
> 1y'' + 0y' + 1y=1r^2e^{rt} + 0re^{rt} + 1e^{rt}=0 \\
> r^2 + 1=0
> \end{align}
> $$
> 
> [[Characteristic equation]]: $r^2 + 1=0 \rightarrow r = \pm \sqrt{-1} = \pm i$
> 
> We have a [[Const Coeff Homo DE#Case 3 Complex pair of roots|complex pair of roots]] (with no real part, $\alpha = 0,\beta=1$), so the solution will have the form
> $$
> \begin{align}
> y_h&=c_1e^{\alpha x}\cos \beta x + c_2e^{\alpha x}\sin \beta x \\
> &=c_1\cos x + c_2\sin x 
> \end{align}
> $$
> 
> > The rest of the steps are due to this solution to the homogenous version of the DE. Had it been a different set of roots then the steps would have been different.
> 
> ##### Step 2: Calculate the derivatives
> This solution is for the homogeneous version of the DE and we are looking for a particular solution the the non-homogenous. So we make the **variation of  parameters**; changing the constant $c_1$ and $c_2$ to be unknown functions of $x$ instead: $u_1(x)$ and $u_2(x)$  
> $$y_p=u_1(x)y_1(x) + u_2(x)y_2(x)$$
> Successive derivatives
> $$
> \begin{align}
> y_p'&=u_1y_1'+u_1'y_1+u_2y_2'+u_2'y_2 \\
> &=(u_1y_1'+u_2y_2') + (u_1'y_1+u_2'y_2) \\
> y_p''&=(u_1y_1''+u_2y_2'') + (u_1'y_1'+u_2'y_2') +(u_1'y_1+u_2'y_2)'
> \end{align}
> $$
> 
> 
> 
> ---
> 
> $$
> \begin{align}
> y_p&=u_1(x)\cos x + u_2(x)\sin x
> \end{align}
> $$
> First derivative
> 
> 
> $$y_p'=\underbrace{u_1'\cos x - u_1\sin x}_{\textit{Product rule}} + \underbrace{u_2'\sin x + u_2\cos x}_{\textit{Product rule}} $$
> $u_1'$ and $u_2'$ are annoying to deal with, so it will make our lives simpler by making the assumption that
> $$u_1'\cos(x) + u_2'\sin(x)=0$$
> Leaving
> $$y'= u_2\cos x- u_1\sin x$$
> >Because the goal is to find a solution, it does not matter if we add additionnal constraints, as long as it helps me find a solution.
> 
> Second derivative
> $$y''= u_2'\cos x - u_2\sin x- u_1'\sin x - u_1\cos x$$
> >Here we are not adding additionnal contraints. This is a case by case thing. We have to notice that adding $y$ and $y''$ will get us closer to a solution and continue from there.
> 
> ##### Step 3: Use everything
> $$
> \begin{align}
> y''+y &= \tan(x)\\
> \iff u_2'\cos x - \cancel{u_2\sin x}- u_1'\sin x -\bcancel{u_1\cos x} + \bcancel{u_1\cos x} + \cancel{u_2\sin x} &= \tan(x) \\
> \iff u_2'\cos x  - u_1'\sin x  &= \tan(x)
> \end{align}
> $$
> Another way of looking at it is that we now have two sets of constraints
> $$
> \begin{align}
> u_2'\cos x  - u_1'\sin x  &= \tan(x) \\
> u_2'\sin(x) + u_1'\cos(x)  &=0
> \end{align}
> $$
> We can almost cancel two terms in both equation if we add them. Do this
> $$
> \begin{align}
> u_2'\cos x \textcolor{orange}{\cos x}  - u_1'\sin x\textcolor{orange}{\cos x}  &= \tan(x) \textcolor{orange}{\cos x}\\
> u_2'\sin x \textcolor{violet}{\sin x} + u_1'\cos x \textcolor{violet}{\sin x}  &=0
> \end{align}
> $$
> Add them
> $$
> \begin{align}
> u_2'\cos^2 x - \cancel{u_1'\sin x\cos x} + u_2'\sin^2 x + \cancel{u_1'\cos x \sin x}  &= \tan(x) \cos x \\
> \iff u_2'\cos^2 x  + u_2'\sin^2 x   &= \sin x\\
> \iff u_2'   &= \sin x\\
> \end{align}
> $$
> Substitute this in either constraint and we get
> $$u_1'=\tan(x) \sin(x)$$
> 
> ##### Step 4: Integrate
> $$u_2 = \int u_2' dx = \int \sin x dx = -\cos x + C_1$$
> 
> $$u_1 = \int u_1' dx = \int \tan(x) \sin(x) dx = \frac{1}{2}\ln\left|\frac{\sin x - 1}{\sin x + 1}\right| + \sin x + C_2$$
> 
> We made the variation of parameters (change the constants in the homogeneous version to functions of x) that our solution would look like 
> $$y=u_1(x)\cos x + u_2(x)\sin x $$
> We now have the coefficient $u_1$ and $u_2$, so we have a solution (not a [[general solution]], but **a** solution).


> [!Question] 
> could this work for higher order than 2?

# Additionnal ressources
[Steve Brunton](https://youtu.be/wCeDUbZQ_zA)
