---
aliases:
    - "Constant Coefficient homogeneous differential equation"
    - "constant coefficient homogeneous differential equation"
tags: 
    - "homogenous" 
    - "second_order"
    - "solution"
---

With the general form of constant coefficient homogeneous differential equation
$$ay'' + by' + cy=0$$
We can make an educated guess

Guess: $y=e^{rt}$
Then $y''=r^2e^{rt},\hspace{0.2em}y'=re^{rt}$


$$
\begin{align}
ay'' + by' + cy=ar^2e^{rt} + bre^{rt} + ce^{rt}=0 \\
ar^2 + br + c=0
\end{align}
$$

> [[Characteristic equation]]: $ar^2 + br + c=0$

The unknown here is $r$ and it can be solved with the quadratic formula.
$$
r = \frac{-b \pm \sqrt{b^2-4ac}}{2a}
$$
Three cases arise from this.

> [!tip]-
> Very important, all of this is derived from the initial guess. Had we guessed something else a different analysis would have been required. 

### Case 1: Distinct roots
---
$n$ distinct roots
$$\boxed{y=c_1e^{r_1t}+c_2e^{r_2t}+\ldots+c_ne^{r_nt}}$$

---

If $b^2-4ac>0$ then we have two real and distinct roots, $r_1$ and $r_2$, so two different solutions
$$\textcolor{lime}{y_1}=e^{r_1t}\hspace{2em}\textcolor{cyan}{y_2}=e^{r_2t}$$
#### [[General solution]]
One is not a scalar multiple of the other, so they are independant and the [[general solution]] is

$$\boxed{y=c_1\textcolor{lime}{e^{r_1t}}+c_2\textcolor{cyan}{e^{r_2t}}}$$
#### Specific solution
If we are given two initial conditions $\left(y(t)=k,\hspace{0.2em}y'(t)=m\right)$ then we have a specific solutions. With those initial conditions in hand we can then find the values of $c_1$ and $c_2$. Calculate the result of these two initial conditions

$$
\begin{align}
y(t) = k &= c_1\textcolor{lime}{e^{r_1t}}+c_2\textcolor{cyan}{e^{r_2t}} \\
y'(t) = m &= c_1r_1\textcolor{lime}{e^{r_1t}}+c_2r_2\textcolor{cyan}{e^{r_2t}}
\end{align}
$$
And we've got ourselves two equations with two unknowns, $c_1$ and $c_2$. $r_1$ and $r_2$ are known, they are the roots above. $t$ is also known, it's part of the initial conditions. 
Replace $x_1 = e^{r_1t}$ and $x_2 = e^{r_2t}$ to get a more familiar feel for solving this with a matrix form.
$$
\begin{align}
k &= c_1\textcolor{lime}{x_1}&+&c_2\textcolor{cyan}{x_2} \\
m &= c_1r_1\textcolor{lime}{x_1}&+&c_2r_2\textcolor{cyan}{x_2}
\end{align}
$$

$$
\left[
\begin{array}{l}
 c_1 & c_2     \\
 c_1r_1 & c_2r_2
\end{array}
\middle\vert
\begin{array}{l}
 k      \\
 m 
\end{array}
\right]
$$
This ties back to [[General solution]]

### Case 2: Repeated root 
---
Root repeated $n$ times
$$\boxed{y=c_1e^{rt}+c_2\textcolor{cyan}{t}e^{rt} + c_3\textcolor{cyan}{t^2}e^{rt} + \ldots + c_n\textcolor{cyan}{t^{n-1}}e^{rt}}$$

---

If $b^2-4ac=0$ then we have repeated root. Unlike case 1, we cannot set $y_1=e^{r_1t}$ and $y_2=e^{r_2t}$ because $r_1=r_2=r$, same root, and $y_1=y_2$ are then, obviously, not independant.

Instead, we do this little trick $y_1=e^{rt}$ and $y_2=\textcolor{cyan}{t}e^{rt}$. $y_2$ may be surprising, but 

This works as: 
1. $te^{rt}$ is a solution. Pluging it into $ay'' + by' + cy=0$ will have the $\textcolor{cyan}{t}$ disappear and the rest is the same. See [[Lin. Ind. of Func#Wronskian|Wronskian]].
2. $te^{rt}$ is linearly independant from $e^{rt}$. The [[Existence and uniqueness#Theorem Linear independance|theorem of linear independance]] says that two solutions are dependant if one is equal to the other times a **constant**. Here $t$ is not a constant, it's the dependant variable.

This trick can be applied as many times as we need. Simply use $\textcolor{cyan}{t^2},\textcolor{cyan}{t^3},\ldots$

#### [[General solution]]
One is not a scalar multiple of the other, so they are independant and the [[general solution]] is

$$\boxed{y=c_1e^{rt}+c_2\textcolor{cyan}{t}e^{rt}}$$
#### Specific solution
Very similar to the case 1, just that we have an additional $\textcolor{cyan}{t}$ in there.

$$
\begin{align}
y(t) = k &= c_1e^{rt}+c_2\textcolor{cyan}{t}e^{rt} \\
y'(t) = m &= c_1re^{rt}+c_2r\textcolor{cyan}{t}e^{rt}
\end{align}
$$
And we solve with
$$
\left[
\begin{array}{l}
 c_1 & \textcolor{cyan}{t}c_2     \\
 c_1r & \textcolor{cyan}{t}c_2r
\end{array}
\middle\vert
\begin{array}{l}
 k      \\
 m 
\end{array}
\right]
$$

### Case 3: Complex pair of roots
$$
\boxed{y=e^{\alpha t}\left[c_1\cos \beta t + c_2\sin \beta t\right]}
$$

If $b^2-4ac<0$ then we have a complex pair of roots (square root of a negative number). Then the roots will be
$$r=\alpha \pm i\beta$$
and the two linearly independant solutions will be 
$$y_1=e^{\alpha + i\beta},\hspace{1em}y_2=e^{\alpha - i\beta}$$
These are perfectly fine and can work for the [[general solution]]
$$y=c_1e^{(\alpha + i\beta)t}+c_2e^{(\alpha - i\beta)t}$$
#### [[General solution]]

But we can modify it so that the solution deals with real numbers and not complex numbers.

By using [[Euler's formula]] 
$$
\begin{align}
y_1=e^{\left(\alpha + i\beta\right)t} &= e^{\alpha t}e^{i\beta t} \\
&= e^{\alpha t}\left(\cos \beta t + i \sin \beta t\right)
\end{align}
$$
$$
\begin{align}
y_2=e^{\left(\alpha - i\beta\right)t} &= e^{\alpha t}e^{-i\beta t} \\
&= e^{\alpha t}\left(\cos \beta t + i \sin (-\beta t)\right) \\
&= e^{\alpha t}\left(\cos \beta t - i \sin \beta t\right)
\end{align}
$$

These are still two linearly independant solutions. We can linearly combine them to create new solutions.

$$
\begin{align}
\frac{y_1+y_2}{2} &= \frac{e^{\alpha t}\left(\cos \beta t + i \sin \beta t\right) + e^{\alpha t}\left(\cos \beta t - i \sin \beta t\right) }{2} \\
&= \frac{e^{\alpha t}\left(\cos \beta t + \cancel{i \sin \beta t} + \cos \beta t - \cancel{i \sin \beta t}\right) }{2} \\
&= e^{\alpha t}\cos \beta t \\
\end{align}
$$

$$
\begin{align}
\frac{y_1-y_2}{2i} &= \frac{e^{\alpha t}\left(\cos \beta t + i \sin \beta t\right) - e^{\alpha t}\left(\cos \beta t - i \sin \beta t\right) }{2i} \\
&= \frac{e^{\alpha t}\left(\cancel{\cos \beta t} + i \sin \beta t - \cancel{\cos \beta t} + i \sin \beta t\right) }{2i} \\
&= e^{\alpha t}\sin \beta t \\
\end{align}
$$

Since these two new solutions are 
1. Solutions because they used a linear combination of linearly independant solutions 
2. Linearly independant (one is clearly not a scalar multiplication of the other)


$$
\boxed{y=c_1e^{\alpha t}\cos \beta t + c_2e^{\alpha t}\sin \beta t}
$$

> [!Example]-
> All of the above is to show a methodology and not a plug-and-play type of thing. Let's take this
> $$y'''+y=0$$
> Guess: $y=e^{rt}$
> [[Characteristic equation]]: $r^3 + r = 0$
> 
> Try to find the roots
> $$
> \begin{align}
> r^3+r&=r(r^2+1) \\
> &=r(r+i)(r-i) \\
> &= 0
> \end{align}
> $$
> $r_1=0, r_2=-i, r_3=i$
> 
> So a [[general solution]] could be 
> $$
> \begin{align}
> y&=c_1e^{r_1t}+c_2e^{r_2t}+c_3e^{r_3t} \\
> &= c_1e^{0t}+c_2e^{-it}+c_3e^{it} \\
> &= c_1+c_2e^{-it}+c_3e^{it} \\
> \end{align}
> $$
> But we would still like to deal with real values. So reusing the same thing as [[Const Coeff Homo DE#General solution|above]], we can rewrite it as
> $$
> \boxed{
> \begin{align}
> y&= c_1+c_2\cos t+c_3\sin t \\
> \end{align}
> }
> $$


> [!example]-
> $$y^{(4)} - 3y''' + 3y'' -y'=0$$
> 
> Guess:  $y=e^{rt}$
> [[Characteristic equation]]: $r^4 -3r^3 + 3r^2 - r = 0$
> 
> Find the roots. This is a case by case. Comes with experience
> $$
> \begin{align}
> r^4 -3r^3 + 3r^2 - r &= r(r^3 -3r^2 + 3r - 1) \\
> &= r(r-1)^3 \\
> &= 0
> \end{align}
> $$
> We have a root repeated 3 times. Look at [[Const Coeff Homo DE#Case 2 Repeated root|repeated roots]].
> $r_1=0$ and $r_2=1$ with multiplicity 3.
> 
> $$
> \boxed{
> y=c_1\underbrace{e^{r_1t}}_{y_1} + c_2\underbrace{te^{r_2t}}_{y_2} + c_3\underbrace{t^2e^{r_2t}}_{y_3} + c_4\underbrace{t^3e^{r_2t}}_{y_4}
> }
> $$


