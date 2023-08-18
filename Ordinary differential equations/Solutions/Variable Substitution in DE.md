If the ODE has this specific form, then we can do a separation of variable with a variable substitution 

$$
P(x,t) + Q(x,t)\dot{x} = 0 \tag{1}
$$
- $x$ is a function of $t$, $x(t)$ (dependent variable). This is important for the notes below as it changes what we derive.
- $t$ is an independent variable 
- $\dot{x}=\frac{dx(t)}{dt}$ (just in case you forget)
- $P(x,t)$ and $Q(x,t)$ are both [[../Definitions/Homogeneous Function|Homogeneous Function]]. They need not be of the same order, but it can help simplify a few things

#### When to look for this?
- Put everything to one side (i.e. something equals 0)
- Isolate $\dot{x}$ or $t^\prime$
- $P(x,t)$ and $Q(x,t)$ could potentially be [[../Definitions/Homogeneous Function|homogeneous function]], so try that out.
- If they are, then you're a happy camper

# Substitutions
There is not just one possible substitution. Replacing a variable by something else is a general strategy. The next three should be fairly common and have some nice properties that will <mark style="background: #ADCCFFA6;">lead to a separation of variables between the dependent and independent variables</mark>. 

### 1. Substitution $u(t) = \frac{x(t)}{t}$
Using the substitution in [[../Definitions/Homogeneous Function#Alternative 1|homogeneous function]] 

$$u(t) = \frac{x(t)}{t} \tag{2}$$^fb3a05

and it’s derivative (using the [[../../Calculus/Derivative/Quotient Rule|Quotient Rule]]) 

$$
\begin{align}
\frac{du(t)}{dt} &= \frac{\frac{dx}{dt}t - x\frac{dt}{dt}}{t^2} \\
\dot{u} &= \frac{\dot{x}t - x}{t^2} \\
\implies \dot{x} &= \dot{u}t+\frac x t \\
\textcolor{violet}{\dot{x}}&= \textcolor{violet}{\dot{u}t + u} \tag{3}
\end{align}
$$

^2432ab

With $P(x,t)=t^ng_1(u)$ and $Q(x,t)=t^ng_2(u)$

> [!note]-
> Don’t forget that $u$ is a function of $t$, $u(t)$. You know you tend to forget that. 

$$
\begin{align}
\textcolor{orange}{P(x,t)}+\textcolor{olive}{Q(x,t)}\textcolor{violet}{\dot{x}}&=0 \tag{4.1}\\
\textcolor{orange}{\cancel{t^n}g_1(u)}+\textcolor{olive}{\cancel{t^n}g_2(u)}(\textcolor{violet}{\dot{u}t+u})&=0 \tag{4.2}\\
g_1(u)+g_2(u)\dot{u}t+g_2(u)u&=0 \tag{4.3}\\
\frac{g_2(u)\dot{u}\cancel{t}}{(g_1(u)+g_2(u)u)\cancel{t}}+\frac{\bcancel{g_1(u)+g_2(u)u}}{\bcancel{(g_1(u)+g_2(u)u)}t}&=0 \tag{4.4} \\
\frac{g_2(u)\dot{u}}{g_1(u)+g_2(u)u}+\frac{1}{t}&=0 \tag{4.5} \\
\frac{g_2(u)\dot{u}}{g_1(u)+g_2(u)u}&=\frac{-1}{t} \tag{4.6}
\end{align}
$$


> [!step]-
> - (4.1) Basic statement
> - (4.2) replace each terms (follow colors)
> - (4.3) Divide everything by $t^n$
> - (4.4) Reorder a little bit and divide everything by $(g_1(u)+g_2(u)u)t$. Requires a bit of foresight to see how that is going to help.
> - (4.5) Clean up
> - (4.6) Obvious


We now have a [[Separation of variables|separation of variables]] with $t$ on one side and $u$ on the other. Integrate both sides with respect to $t$ and hopefully you get something that is digestable (i.e. not too hard to integrate).

$$
\begin{align}
\int \frac{g_2(u)}{g_1(u)+g_2(u)u}\frac{du}{dt}dt&=\int \frac{-1}{t}dt \\
\int \frac{g_2(u)}{g_1(u)+g_2(u)u}du&=\int \frac{-1}{t}dt \\
\end{align}
$$

Once the integration is done, replace $u$ back by $\frac{x(t)}{t}$.

### 2. Substitution $u(t) = \frac{t}{x(t)}$

Using the substitution (see [[../Definitions/Homogeneous Function#Alternative 2|Homogeneous Function]])
$$u(t) = \frac{t}{x(t)} \tag{5}$$^f45a5d

and it’s derivative (using the [[../../Calculus/Derivative/Quotient Rule|Quotient Rule]]) 

$$
\begin{align}
\frac{du(t)}{dt} &= \frac{\frac{dt}{dt}x - t\frac{dx}{dt}}{x^2} \\
\dot{u} &= \frac{x - t\dot{x}}{x^2} \\
\implies \dot{x} &= \frac{x - \dot{u}x^2}{t} \\
&= \frac{x}{t} - \frac{t}{t}\frac{\dot{u}x^2}{t} \\
\textcolor{violet}{\dot{x}}&= \textcolor{violet}{\frac{1}{u} - \frac{\dot{u}t}{u^2}} \tag{6}
\end{align}
$$

^140314

With $P(x,t)=x^ng_1(u)$ and $Q(x,t)=x^ng_2(u)$

> [!note]-
> Don’t forget that $u$ is a function of $t$, $u(t)$. You know you tend to forget that. 

$$
\begin{align}
\textcolor{orange}{P(x,t)}+\textcolor{olive}{Q(x,t)}\textcolor{violet}{\dot{x}}&=0 \tag{7.1}\\
\textcolor{orange}{\cancel{x^n}g_1(u)}+\textcolor{olive}{\cancel{x^n}g_2(u)}\left(\textcolor{violet}{\frac{1}{u} - \frac{\dot{u}t}{u^2}}\right)&=0 \tag{7.2}\\
g_1(u)+\frac{g_2(u)}{u} - \frac{g_2(u)\dot{u}t}{u^2} &=0 \tag{7.3}\\
\frac{g_2(u)\dot{u}t}{u^2} &=g_1(u)+\frac{g_2(u)}{u}  \tag{7.4}\\
t &=\frac{g_1(u)u^2+g_2(u)u}{g_2(u)\dot{u}}  \tag{7.5}\\
\frac{1}{t} &=\frac{g_2(u)}{g_1(u)u^2+g_2(u)u}\dot{u}  \tag{7.6}\\
\end{align}
$$


> [!step]-
> - (7.1) Basic statement
> - (7.2) replace each terms (follow colors)
> - (7.3) Divide everything by $t^n$
> - (7.4) Reorder a little bit 
> - (7.5) Isolate $t$
> - (7.6) $\dot{u}$ is in the denominator, needs to be in the numerator. Inverse both sides.


We now have a [[Separation of variables|separation of variables]] with $t$ on one side and $u$ on the other. Integrate both sides with respect to $t$ and hopefully you get something that is digestable (i.e. not too hard to integrate).

$$
\begin{align}
\int \frac{1}{t} dt &=\int \frac{g_2(u)}{g_1(u)u^2+g_2(u)u}\frac{du}{dt}dt \\
\int \frac{1}{t} dt &=\int \frac{g_2(u)}{g_1(u)u^2+g_2(u)u}du

\end{align}
$$

Once the integration is done, replace $u$ back by $\frac{t}{x(t)}$.


### 3. Substitution $u(t) = \frac{t(x)}{x}$

This is similar to [[#2. Substitution $u(t) = frac{t}{x(t)}$|the second substitution]], but now $t$ is the dependant variable and $x$ is the independent one. Given that we changed the dependent and independent variable, we need to consider a small change to (1) (the derivative next to $Q$ is $t^\prime$ and not $x^\prime$)

$$P(t(x),x) + Q(t(x),x)t^\prime=0$$

$$u(t) = \frac{t(x)}{x} \tag{8}$$ ^8fb6fa

and it’s derivative (using the [[../../Calculus/Derivative/Quotient Rule|Quotient Rule]]) 

$$
\begin{align}
\frac{du(x)}{dx} &= \frac{\frac{dt}{dx}x - t\frac{dx}{dx}}{x^2} \\
u^\prime &= \frac{t^\prime x - t}{x^2} \\
\implies \textcolor{violet}{t^\prime} &= \frac{u^\prime x^2 + t}{x} = \textcolor{violet}{u^\prime x + u} \tag{9}
\end{align}
$$

^55a807

With $P(x,t(x))=t(x)^ng_1(u)$ and $Q(x,t(x))=t(x)^ng_2(u)$

> [!note]-
> Don’t forget that $u$ is a function of $x$, $u(x)$. You know you tend to forget that. 

$$
\begin{align}
\textcolor{orange}{P(x,t)}+\textcolor{olive}{Q(x,t)}\textcolor{violet}{t^\prime}&=0 \tag{10.1}\\
\textcolor{orange}{\cancel{t^n}g_1(u)}+\textcolor{olive}{\cancel{t^n}g_2(u)}(\textcolor{violet}{u^\prime x + u})&=0 \tag{10.2}\\
g_1(u)+g_2(u)u^\prime x+g_2(u)u&=0 \tag{10.3}\\
-\frac{g_2(u)u + g_1(u)}{g_2(u)u^\prime} &=  x  \tag{10.4} \\
\frac{g_2(u)u^\prime}{g_2(u)u + g_1(u)} &=  -\frac 1 x \tag{10.5} \\
\end{align}
$$


> [!step]-
> - (1.1) Basic statement
> - (1.2) replace each terms (follow colors)
> - (1.3) Divide everything by $t^n$
> - (1.4) Reorder a little bit and divide everything by $(g_1(u)+g_2(u)u)t$. Requires a bit of foresight to see how that is going to help.
> - (1.5) Clean up
> - (1.6) Obvious


We now have a [[Separation of variables|separation of variables]] with $x$ (the independent variable) on one side and $u$ on the other. Integrate both sides with respect to $x$ and hopefully you get something that is digestable (i.e. not too hard to integrate).

$$
\begin{align}
-\int \frac{1}{x} dx &=\int \frac{g_2(u)}{g_2(u)u + g_1(u)}\frac{du}{dt}dt \\
-\int \frac{1}{x} dx &=\int \frac{g_2(u)}{g_2(u)u + g_1(u)}du

\end{align}
$$

Once the integration is done, replace $u$ back by $\frac{t(x)}{x}$.


# Examples

## Example
#example 
This example will show how to solve it with all 3 substitutions. 


> [!Important] 
> Pay attention to the conditions we are adding along the way to make the simplifications. These conditions exclude certain set of values from the family of solutions and we should validate if these values could be part of another set of solutions.

### Statement
> Find a 1-parameter family of solutions of
> 
> $$
> \left(\sqrt{x^{2}-y^{2}}+y\right)-x\frac{dy}{dx}=0 \tag{a}
> $$
> 
> also any particular solution not obtainable from the family.

The notation is a departure from above. $x$ is, normally, the independent variable and $y(x)$ the dependent one (except in [[#Alternative 3]])

### Solution

We observe first that (a) makes sense only if $|y| \leq|x|$. Either $\left \lvert \frac y  x\right\lvert \leq 1$ or $\left \lvert \frac x  y\right\lvert \geq 1$. Depending of the substitution, either conditions will come into play.

Second we note that (a) is a differential equation with homogeneous coefficients of order one. 
$$
\underbrace{\left(\sqrt{x^{2}-y^{2}}+y\right)}_{P(x,y)}-\underbrace{x}_{Q(x,y)}\frac{dy}{dx}
$$


> [!step]- Homogeneous functions
> Using [[../Definitions/Homogeneous Function#Alternative 3|Alternative 3]]
> $$P(cx,cy) = \sqrt{(cx)^2 - (cy)^2} = c\sqrt{x^2-y^2} = cP(x,y)$$
> This is an homogeneous function of order 1
> 
> Similarly
> $$Q(cx,cy) = cx = cQ(x,y)$$
> 
> Which is also of order 1

#### Alternative 1
Using the substitution [[#^fb3a05|(2)]] we get the additional constraint that $x \neq 0$ (denominator) alongside $\left( \frac{y}{x}\right)^2 \leq 1$ due to the square root $\sqrt{x^2-y^2}=x\sqrt{1-\left(\frac y x \right)^2}$ and the choice of $u$
$$u(x) = \frac{y(x)}{x}, \qquad y(x) = u(x)x$$

Using the derivative [[#^2432ab|(3)]]


> [!Tip|right-small] 
> $$\frac{x^3}{|x|}=\frac{(-1)^3|x|^3}{|x|} = (-1)^3|x|^2=-|x|^2$$
> $$\frac{x^2}{|x|}=\frac{(-1)^2|x|^2}{|x|} = (-1)^2|x|=|x|$$


$$
\begin{align}
\sqrt{x^{2}-u^{2} x^{2}}+ux-x\left(\frac{du}{dx}x + u\right)&=0 \\
|x|\sqrt{1-u^{2}}+\cancel{ux}-\frac{du}{dx}x^2 + \cancel{ux}&=0 \\
x\left(\pm\sqrt{1-u^{2}}-\frac{du}{dx}x\right)&=0 \tag{b} \\
\end{align}
$$

Since $x \neq 0$, we can divide (b) by it to obtain after simplification

$$
\pm \sqrt{1-u^{2}}-x\frac{du}{dx}=0 \tag{c}
$$

where the + sign is to be used if $x>0$; the - sign if $x<0$.

Further if $u \neq \pm 1$, we may divide (c) by $\sqrt{1-u^{2}}$. Therefore (c) becomes

$$
\frac{1}{x}= \pm \frac{1}{\sqrt{1-u^{2}}} \frac{du}{dx} \tag{d}
$$

Since we are excluding $u=1$, $u^2=\left ( \frac{y}{x}\right )^2 \leq 1$ becomes $u=\left ( \frac{y}{x}\right )^2 \lt 1$ (or $u = \left \lvert \frac{y}{x}\right \lvert \lt 1$)

The variables are now separated. By using [[Separation of variables|separation of variables]] 


> [!step]-
> $$
> \begin{align}
> \frac{1}{x} &= \pm \frac{1}{\sqrt{1-u^{2}}} \frac{du}{dx} \\
> \int \frac{1}{x} dx &= \pm \int \frac{1}{\sqrt{1-u^{2}}} \frac{du}{dx} dx \\
> \int \frac{1}{x} dx &= \pm \int \frac{1}{\sqrt{1-u^{2}}} du \\
> \log{x} + C_1 &= \pm \operatorname{arcsin}{u} + C_2 \\
> \log{x} + C &= \pm \operatorname{arcsin}{u} \\
> \end{align}
> $$
> We made use of [[../../Calculus/Integration/Inverse trigonometric functions|Inverse arcsin function]]
> $+$ sign holding if $x>0$; the $-$ sign if $x<0$

$$
\begin{align}
\log x +c & =\operatorname{arcsin} u, \quad\lvert u\lvert <1, x>0 \tag{e}\\
-\log (-x) + c & =\operatorname{arcsin} u, \quad \lvert u\lvert <1, x<0
\end{align}
$$

Replacing $u$ in (e) by its value, we have

$$
\begin{array}{r}
\log x + c=\operatorname{arcsin} \frac{y}{x},\qquad \left|\frac{y}{x}\right|<1, x>0, \tag{f} \\
-\log (-x) + c =\operatorname{arcsin} \frac{y}{x},\qquad\left|\frac{y}{x}\right|<1, x<0 .
\end{array}
$$

##### Final answer
$$
\boxed{
\begin{array}{r}
y(x)= x\sin\left(\log x + c\right),\qquad\left|\frac{y}{x}\right|<1, x>0, \tag{g} \\
y(x)=x\sin\left(-\log (-x) + c\right) ,\qquad \left|\frac{y}{x}\right|<1, x<0
\end{array}
}
$$


#### Alternative 2

Using the substitution [[#^f45a5d|(5)]]  we get the additional constraint that $\textcolor{green}{y(x) \neq 0}$ (denominator) alongside $u^2=\left( \frac{x}{y(x)}\right)^2 \geq 1$ due to the square root $\sqrt{x^2-y^2}=\lvert y \lvert \sqrt{\left(\frac x {y(x)} \right)^2-1}$ and the choice of $u$

$$u(x) = \frac{x}{y(x)}, \qquad x = u(x)y(x)$$

Using the derivative [[#^140314|(6)]]
$$
\begin{align}
\sqrt{\left(\frac{x}{y}\right)^2-y^2}+y-uy\left(\frac{1}{u} - \frac{\dot{u}x}{u^2}\right) &= 0 \\
|y|\sqrt{u^2-1}+\cancel{y}-\cancel{y} +\frac{\dot{u}\bcancel{u^2}y^2}{\bcancel{u^2}} &= 0 \\
|y|\sqrt{u^2-1}+\dot{u}y^2 &= 0 \\
\pm\sqrt{u^2-1}+\dot{u}y &= 0 \\
\frac 1 x &=-\frac{\dot{u}}{\pm u\sqrt{u^2-1}}\qquad \textcolor{green}{\text{if } u^2 \neq 1} \\
\end{align}
$$

The + sign holding if $y\gt 0$ and - sign if $y\lt 0$. But because there is a negative sign on the right, it flips. 

$$
\begin{drcases}
    \frac 1 x =-\frac{\dot{u}}{\pm u\sqrt{u^2-1}}
\end{drcases} \qquad \begin{array}{}+\text{ if } y>0 \\-\text{ if } y<0\end{array} 
$$
becomes
$$
\begin{drcases}
    \frac 1 x =\frac{\dot{u}}{\mp u\sqrt{u^2-1}}
\end{drcases} \qquad \begin{array}{}-\text{ if } y>0 \\+\text{ if } y<0\end{array}
$$

Now, the - sign holding if $y\gt 0$ and + sign if $y\lt 0$. 

> [!important]
> The dependent and independent variables are now separated and we can proceed to do the integration. Keep in mind an important fact: the $\mp$ is based on $y$. We will have to do an analysis for when $u<-1$ and when $u>1$ because of [[../../Calculus/Integration/Inverse trigonometric functions#arccsc|arccsc]]

> [!Note]- $\operatorname{arc csc}$
> $$\int \mp\frac{1}{u\sqrt{u^2-1 }} \, \frac{\mathrm{d}u}{\mathrm{d}x}dx = \operatorname{arc csc} u+C$$
>  negative holds if $u \gt 1$, + if $u\lt-1$


If $\textcolor{green}{u \neq 1}$ (additional condition to keep in mind), $u(x)=\left\lvert \frac{x}{y}\right\lvert \geq 1$ becomes $u(x)=\left\lvert \frac{x}{y}\right\lvert \gt 1$ 


Using the [[../../Calculus/Integration/Inverse trigonometric functions|Inverse trigonometric functions]] for $\operatorname{arc csc}$, we have the constraint that $u<-1$ or $u>1$.

We could use the $\operatorname{arc sec}$ inverse trigonometric function instead, but everything simplifies quite nicely if we stick with $\operatorname{arc csc}$. 

There is more analysis to do compared to [[#Alternative 1]] because we are dealing with the condition of signs in $\operatorname{arc csc}$.

##### Case $y\gt 0, x \gt 0 \implies u \gt 1$
$$
\begin{align}
\int \frac{1}{x} dx =-\int \frac{1}{u\sqrt{u^{2}-1}}\frac{du}{dx} dx 
\end{align}
$$

Because $u \gt 1$, we should have a negative sign inside the integral. So we are simply going to use the negative sign from the $y\gt 0$ condition. 

$$
\begin{align}
\log{x} + C_1 &=\int - \frac{1}{u\sqrt{u^{2}-1}}du \\
\log{x} + C_1 &=\operatorname{arc csc} u +C_2 \\
\log{x} + C &=\operatorname{arc csc} u \\
\csc(\log{x} + C) &=\frac x y \\
\frac{1}{\sin(\log{x} + C)} &=\frac x y \\
y(x)&=\textcolor{#A3BE8C}{x\sin(\log{x} + C)} \\
\end{align}
$$

##### Case $y\gt 0, x \lt 0 \implies u \lt -1$

$$
\begin{align}
\int \frac{1}{x} dx =-\int \frac{1}{u\sqrt{u^{2}-1}}\frac{du}{dx} dx 
\end{align}
$$


Because $u \lt 1$, we leave the integral as-is. Meaning that the extra negative sign will end up on the other side.


> [!NOTE]-
> Because $x \lt 0$, the integral of $\frac 1 x$ is $\log(-x)$.


$$
\begin{align}
-\log(-x) + C_1 &=\int \frac{1}{u\sqrt{u^{2}-1}}du \\
-\log(-x) + C_1 &=\operatorname{arc csc} u +C_2 \\
-\log(-x) + C &=\operatorname{arc csc} u \\
\csc(-\log(-x) + C) &=\frac x y \\
\frac{1}{\sin(-\log(x) + C)} &=\frac x y \\
y(x)&=\textcolor{#D08770}{x\sin(-\log(-x) + C)} \\
\end{align}
$$

##### Case $y\lt 0, x \gt 0 \implies u \lt -1$
$$
\begin{align}
\int \frac{1}{x} dx =\int \frac{1}{u\sqrt{u^{2}-1}}\frac{du}{dx} dx 
\end{align}
$$

Because $u \lt -1$ and the integral has a positive sign (due to $y<0$), we proceed with the integration without worrying about anything

$$
\begin{align}
\log{x} + C_1 &=\int \frac{1}{u\sqrt{u^{2}-1}}du \\
\log{x} + C_1 &=\operatorname{arc csc} u +C_2 \\
\log{x} + C &=\operatorname{arc csc} u \\
\csc(\log{x} + C) &=\frac x y \\
\frac{1}{\sin(\log{x} + C)} &=\frac x y \\
y(x)&=\textcolor{#A3BE8C}{x\sin(\log{x} + C)} \\
\end{align}
$$

##### Case $y\lt 0, x \lt 0 \implies u \gt 1$
$$
\begin{align}
\int \frac{1}{x} dx =\int \frac{1}{u\sqrt{u^{2}-1}}\frac{du}{dx} dx 
\end{align}
$$


Because $u \gt 1$,  the integral should have a negative sign if we want to use $\operatorname{arc csc}$, so we multiply both sides by $-1$. 


> [!NOTE]-
> Because $x \lt 0$, the integral of $\frac 1 x$ is $\log(-x)$.

$$
\begin{align}
-\log(-x) + C_1 &=\int \frac{1}{u\sqrt{u^{2}-1}}du \\
-\log(-x) + C_1 &=\operatorname{arc csc} u +C_2 \\
-\log(-x) + C &=\operatorname{arc csc} u \\
\csc(-\log(-x) + C) &=\frac x y \\
\frac{1}{\sin(-\log(x) + C)} &=\frac x y \\
y(x)&=\textcolor{#D08770}{x\sin(-\log(-x) + C)}  \\
\end{align}
$$


##### Final answer
Looking at all 4 cases, we end up with the same thing as [[#Final answer|Alternative 1]]

$$
\boxed{
\begin{array}{r}
y(x)= \textcolor{#A3BE8C}{x\sin\left(\log{x} + C\right)},\qquad\left|\frac{x}{y}\right|\gt 1, x>0 \\
y(x)=\textcolor{#D08770}{x\sin\left(-\log (-x) + C\right)} ,\qquad \left|\frac x y\right|\gt 1, x<0
\end{array}
}
$$


#### Alternative 3

Using the substitution  [[#^8fb6fa|(8)]]   we get the additional constraint that $\textcolor{green}{y \neq 0}$ (denominator) alongside $u^2=\left( \frac{x(y)}{y}\right)^2 \geq 1$ due to the square root $\sqrt{x^2-y^2}=\lvert y \lvert \sqrt{\left(\frac {x(y)} y \right)^2-1}$ and the choice of $u$

$$u(y) = \frac{x(y)}{y}, \qquad x(y) = u(y)y$$

Using the derivative [[#^55a807|(9)]]


> [!Tip|right-small] 
> $$\frac{x^3}{|x|}=\frac{(-1)^3|x|^3}{|x|} = (-1)^3|x|^2=-|x|^2$$
> $$\frac{x^2}{|x|}=\frac{(-1)^2|x|^2}{|x|} = (-1)^2|x|=|x|$$


$$
\begin{align}
\left(\sqrt{u^{2} y^{2}-y^{2}}+y\right)\frac{dx}{dy}-uy&=0 \\
\left(|y|\sqrt{u^{2} -1}+y\right)\left(\frac{du}{dy}y + u\right)-uy&=0 \\
u^\prime|y|y\sqrt{u^{2}-1} + u|y|\sqrt{u^{2}-1} + u^\prime y^2 + \cancel{uy}-\cancel{uy}&=0 \\
\pm u^\prime y\sqrt{u^{2}-1} \pm u\sqrt{u^{2}-1} + u^\prime y &=0 \\
u^\prime y\left(\pm \sqrt{u^{2}-1} +1\right) \pm u\sqrt{u^{2}-1} &=0 \\
u^\prime y\left(\pm \sqrt{u^{2}-1} +1\right)  &=\mp u\sqrt{u^{2}-1} \\
y  &=\frac{\mp u\sqrt{u^{2}-1}}{u^\prime \left(\pm \sqrt{u^{2}-1} +1\right)} \\
\frac 1 y  &=\frac{ \pm \sqrt{u^{2}-1} +1}{\mp u\sqrt{u^{2}-1}}u^\prime \\
\frac 1 y  &=\left(\frac{ \pm \sqrt{u^{2}-1}}{\mp u\sqrt{u^{2}-1}} + \frac{1}{\mp u\sqrt{u^{2}-1}} \right)u^\prime \\
\end{align}
$$  

$$
\frac 1 y  =
\begin{drcases}
    \left(\frac{-1}{ u} + \frac{1}{\mp u\sqrt{u^{2}-1}} \right)u^\prime
\end{drcases} \qquad \begin{array}{}-&\text{ if }& y\gt 0 \\+&\text{ if }& y\lt 0\end{array} \tag{11} \\
$$  

^a27a6f

To divide by the square root we need the condition $\textcolor{green}{u \neq 1}$ (additional condition to keep in mind), $u(x)=\left\lvert \frac{x}{y}\right\lvert \geq 1$ becomes $u(x)=\left\lvert \frac{x}{y}\right\lvert \gt 1$ 

Looking ahead, we are going to use [[../../Calculus/Integration/Inverse trigonometric functions#arccsc|arccsc]], so the sign of $u$ will drive the part of this analysis, just like [[#Alternative 2]]. 

##### Case $y\gt 0, x \gt 0 \implies u \gt 1$

$$
\begin{align}
\int \frac{1}{y} dy &=-\int \frac 1 u \frac{du}{dy}dy - \int \frac{1}{u\sqrt{u^{2}-1}}\frac{du}{dy} dy \\
\log{y} &=-\log u  + \operatorname{arc csc} u + C \\
\log{y} &=-\log\left(\frac x y \right)  + \operatorname{arc csc} u + C \\
\cancel{\log{y}} &=-\left(\log x -\cancel{\log y} \right)  + \operatorname{arc csc} u + C \\
\log x +C&= \operatorname{arc csc} u\\
\csc \left(\log x +C \right) &= \frac x y\\
y &= \frac{x}{\csc \left(\log x +C \right)}\\
y &= \textcolor{#A3BE8C}{x\sin\left(\log{x} + C\right)}\\
\end{align}
$$

Because $u \gt 1$, we should have a negative sign inside the integral. So we are simply going to use the negative sign from the $y\gt 0$ condition. 


##### Case $y\gt 0, x \lt 0 \implies u \lt -1$

$$
\begin{align}
\int \frac{1}{y} dy &=-\int \frac 1 u \frac{du}{dy}dy - \int \frac{1}{u\sqrt{u^{2}-1}}\frac{du}{dy} dy \\
\int \frac{1}{y} dy &=-\int \frac 1 u du - \int \frac{1}{u\sqrt{u^{2}-1}} du \\
\log{y} &=-\log(-u)  - \operatorname{arc csc} u + C \\
\log{y} &=-\log\left(\frac {-x} y \right)  - \operatorname{arc csc} u + C \\
\cancel{\log{y}} &=-\log(-x) +\cancel{\log y}  - \operatorname{arc csc} u + C \\
-\log(-x) +C&= \operatorname{arc csc} u\\
\csc \left(-\log(-x) +C \right) &= \frac x y\\
y &= \frac{x}{\csc \left(-\log(-x) +C \right)}\\
y &=\textcolor{#D08770}{x\sin(-\log(-x) + C)}\\
\end{align}
$$



Because $u \lt 1$, we leave the integral as-is. Meaning that the extra negative sign will end up on the other side.


> [!NOTE]-
> Because $u \lt 0$, the integral of $\frac 1 u$ is $\log(-u)$.


##### Case $y\lt 0, x \gt 0 \implies u \lt -1$

Bottom sign (+) in [[#^a27a6f|(11)]] because $y \lt 0$. And because $u\lt -1$ the second part of the integral can be left as is

$$
\begin{align}
\int \frac{1}{y} dy &=-\int \frac 1 u \frac{du}{dy}dy + \int \frac{1}{u\sqrt{u^{2}-1}}\frac{du}{dy} dy \\
\log(-y) &=-\log(-u)  + \operatorname{arc csc} u + C \\
\log(-y) &=-\log\left(\frac x {-y} \right)  + \operatorname{arc csc} u + C \\
\cancel{\log(-y)} &=-\left(\log x -\cancel{\log(-y)} \right)  + \operatorname{arc csc} u + C \\
\log x +C&= \operatorname{arc csc} u\\
\csc \left(\log x +C \right) &= \frac x y\\
y &= \frac{x}{\csc \left(\log x +C \right)}\\
y &= \textcolor{#A3BE8C}{x\sin\left(\log{x} + C\right)}\\
\end{align}
$$

 
##### Case $y\lt 0, x \lt 0 \implies u \gt 1$

Bottom sign (+) in [[#^a27a6f|(11)]] because $y \lt 0$. And because $u\gt 1$ the second part of the integral needs a negative sign, so we multiply both sides by -1.

$$
\begin{align}
\int \frac{1}{y} dy &=-\int \frac 1 u \frac{du}{dy}dy + \int \frac{1}{u\sqrt{u^{2}-1}}\frac{du}{dy} dy \\
-\int \frac{1}{y} dy &=\int \frac 1 u du + \int \frac{-1}{u\sqrt{u^{2}-1}} du \\
-\log(-y) &=-\log(u)  - \operatorname{arc csc} u + C \\
-\log(-y) &=-\log\left(\frac {-x} {-y} \right)  + \operatorname{arc csc} u + C \\
\cancel{-\log(-y)} &=\log(-x) -\cancel{\log (-y)}  + \operatorname{arc csc} u + C \\
-\log(-x) +C&= \operatorname{arc csc} u\\
\csc \left(-\log(-x) +C \right) &= \frac x y\\
y &= \frac{x}{\csc \left(-\log(-x) +C \right)}\\
y &=\textcolor{#D08770}{x\sin(-\log(-x) + C)}\\
\end{align}
$$

##### Final answer
Looking at all 4 cases, we end up with the same thing as [[#Final answer|Alternative 1]] and [[#Final answer|Alternative 2]]

$$
\boxed{
\begin{array}{r}
y(x)= \textcolor{#A3BE8C}{x\sin\left(\log{x} + C\right)},\qquad\left|\frac{x}{y}\right|\gt 1, x>0 \\
y(x)=\textcolor{#D08770}{x\sin\left(-\log (-x) + C\right)} ,\qquad \left|\frac x y\right|\gt 1, x<0
\end{array}
}
$$


#### Validation
$$
\boxed{
\begin{array}{r}
y(x)= \textcolor{#A3BE8C}{x\sin\left(\log{x} + C\right)},\qquad\left|\frac{x}{y}\right|\gt 1, x>0 \\
y(x)=\textcolor{#D08770}{x\sin\left(-\log (-x) + C\right)} ,\qquad \left|\frac x y\right|\gt 1, x<0
\end{array}
}
$$

```tikz
\usepackage{pgfplots}
\pgfplotsset{compat=1.16,width=10cm}
\begin{document} 
    \begin{tikzpicture}
        \begin{axis}[
            axis lines=middle,
            xlabel=$x$,
            ylabel=$y(x)$,
            samples=200,
            enlargelimits,
            draw={rgb,255:red,209;green,209;blue,209},
            text={rgb,255:red,209;green,209;blue,209},
        ]
        \addplot[
            ultra thick,
            domain=0.001:2,
            draw={rgb,255:red,163;green,190;blue,140}
        ] {(x*sin(ln(x))};

        \addplot[
            ultra thick,
            domain=-0.001:-2,
            draw={rgb,255:red,208;green,135;blue,112}
        ] {(x*sin(-ln(-x))};
    \end{axis} 
    \end{tikzpicture} 
\end{document} 
```

First set of conditions
$$y(x)= x\sin\left(\log x + c\right),\qquad\left|\frac{y}{x}\right|<1, x>0$$
Find the derivative now that we know the function
$$
\begin{align}
\frac{dy}{dx} &= \frac{dx}{dx}\sin\left(\log x + c\right) + x \frac{d}{dx}\sin\left(\log x + c\right) \\
&= \sin\left(\log x + c\right) + x\left(\cos\left(\log x + c\right) \left(\frac{d}{dx} \log(x) + c \right)\right) \\
&= \sin\left(\log x + c\right) + x\left(\cos\left(\log x + c\right) \frac{1}{x}\right) \\
&= \sin\left(\log x + c\right) +\cos\left(\log x + c\right) \\
\end{align}
$$

Plug and play
$$
\begin{align}
&\sqrt{x^{2}-y^{2}}+y-x\frac{dy}{dx}  \\
&=\sqrt{x^{2}-x^2\sin^2\left(\log x + c\right)}+x\sin\left(\log x + c\right)-x\left(\sin\left(\log x + c\right) +\cos\left(\log x + c\right)\right)  \\
&=|x|\sqrt{1-\sin^2\left(\log x + c\right)}+\cancel{x\sin\left(\log x + c\right)}-\cancel{x\sin\left(\log x + c\right)} - x\cos\left(\log x + c\right)  \\
&=\cancel{x\cos^2\left(\log x + c\right)} - \cancel{x\cos\left(\log x + c\right)}w  \\
&= 0
\end{align}
$$

We dropped the absolute value because of the condition that $x>0$.

Second set of conditions
$$y(x)=x\sin\left(-\log (-x) + c\right) ,\qquad \left|\frac{y}{x}\right|<1, x<0$$

Find the derivative now that we know the function
$$
\begin{align}
\frac{dy}{dx} &= \frac{dx}{dx}\sin\left(-\log (-x) + c\right) + x \frac{d}{dx}\sin\left(-\log (-x) + c\right) \\
&= \sin\left(-\log (-x) + c\right) + x\left(\cos\left(-\log(-x) + c\right) \left(\frac{d}{dx} -\log(-x) + c \right)\right) \\
&= \sin\left(-\log (-x) + c\right) + x\left(\cos\left(-\log(-x) + c\right) \frac{-1}{x}\right) \\
&= \sin\left(-\log (-x) + c\right) -\cos(-\log(-x) + c) \\
\end{align}
$$

Plug and play
$$
\begin{align}
&\sqrt{x^{2}-y^{2}}+y-x\frac{dy}{dx}  \\
&=\sqrt{x^{2}-x^2\sin^2\left(-\log(-x) + c\right)}+x\sin\left(-\log(-x) + c\right)-x\left(\sin\left(-\log(-x) + c\right) -\cos\left(-\log(-x) + c\right)\right)  \\
&=|x|\sqrt{1-\sin^2\left(-\log(-x) + c\right)}+\cancel{x\sin\left(-\log(-x) + c\right)}-\cancel{x\sin\left(-\log(-x) + c\right)} +x\cos\left(-\log(-x) + c\right) \\
&=\cancel{|x|}\cos\left(-\log(-x) + c\right) + \cancel{x}\cos\left(-\log(-x) + c\right) \\
&=\pm\cos\left(-\log(-x) + c\right) + \cos\left(-\log(-x) + c\right) \\
&=-cos\left(-\log(-x) + c\right) + \cos\left(-\log(-x) + c\right)\quad \text{- because } x<0 \\
&= 0
\end{align}
$$


In obtaining the solution, we had to exclude the values $|u|=|y / x|=$ 1. This means we had to exclude the functions $y= \pm x$. 



## Example
#example 

### Statement
Find a 1-parameter family of solutions of each of the following equations. Assume in each case that the coefficient of $d y \neq 0$.

$2 x y +\left(x^{2}+y^{2}\right)\frac{dy}{dx}=0$.

### Solution

$$\underbrace{2 x y}_{P(x,y)} +\underbrace{\left(x^{2}+y^{2}\right)}_{Q(x,y)}\frac{dy}{dx}=0$$
Using substitution [[#^fb3a05|(2)]], derivative [[#^2432ab|(3)]] and replace them

$$
\begin{align}
2x^2u + (x^2+u^2x^2)(u^\prime x + u)&=0 \\
2x^2u + x^3u^\prime + x^2u + u^2u^\prime x^3 + u^3x^2&=0 \\
2u + xu^\prime + u + u^2u^\prime x + u^3 &= 0\\
3u+u^\prime x(1 + u^2)+u^3&= 0\\
x&= -\frac{u^3+3u}{u^\prime(1+u^2)} \\
\frac 1 x&= -\frac{1+u^2}{u^3+3u}u^\prime \\
\frac 1 x&= -\left(\frac{1}{u(u^2+3)}u^\prime + \frac{u^2}{u(u^2+3)}u^\prime\right) \\
\end{align}
$$


> [!step]- [[../../Partial Fraction Expansion|Partial Fraction Expansion]]
> $$
> \begin{align}
> \frac{1}{u(u^2+3)} &= \frac{A}{u} + \frac{B}{u^2+3} \\
> \frac{1}{\cancel{u(u^2+3)}} &= \frac{(u^2+3)A+ uB}{\cancel{u(u^2+3)}} \\
> 1 &= (u^2+3)A+ uB \\
> 1 &= \underbrace{Au^2 + uB}_{=0} + 3A  \\
> \end{align}
> $$
> The only power on the left is power of zero, so $3A=1$. The rest must be equal to zero.
> $$
> \begin{array}{rrr}
> 3A=1,  &Au^2 + uB &= 0 \\
> A=\frac{1}{3},  &\frac{1}{3}u^2 + uB &= 0 \\
> A=\frac{1}{3},  & B &= -\frac{u}{3} \\
> \end{array}
> $$
> Replace $A$ and $B$, validate them and you will find
> $$\frac{1}{u(u^2+3)} = \frac{A}{u} + \frac{B}{u^2+3} = \frac{1}{3u} - \frac{u}{3(u^2+3)}$$


$$
\begin{aligned}
\frac{1}{x} & =\frac{1+u^2}{-u^3-3 u} u^{\prime}=-\left(\frac{1}{u\left(3+u^2\right)}+\frac{u}{\left(3+u^2\right)}\right) u \\
& =-\left(\frac{1}{3 u}-\frac{u}{3\left(3+u^2\right)}+\frac{u}{\left(3+u^2\right)}\right) u^{\prime} \\
& =-\left(\frac{1}{3 u}+\textcolor{orange}{\frac{2 u}{3\left(3+u^2\right)}}\right) u^{\prime}
\end{aligned}
$$


Using [[../../Calculus/Integration/Substitution rule|Substitution rule]] for $u^2$
$$
\begin{aligned}
''\int \textcolor{orange}{\frac{2 u}{3\left(3+u^2\right)}} u^{\prime}& =\frac{1}{3} \int \frac{1}{3+w} d w,\qquad w=u^2, \frac{d w}{d u}=2 u \\
& =\frac{1}{3} \log (3+w)+C \\
& =\frac{1}{3} \log \left(3+u^2\right)+C
\end{aligned}
$$



$$
\begin{aligned}
\int \frac{1}{x} dx&=-\left(\int\frac{1}{3 u} d u+\int \textcolor{orange}{\frac{2 u}{3\left(3+u^2\right)}} du \right) \\
\int \frac{1}{x} dx&=-\left(\int \frac{1}{3u} d u+\frac{1}{3} \int \frac{1}{3+w} d w \right) \\
\log (x)+C_1&=-\left(\frac{1}{3} \log (u)+\frac{1}{3} \log \left(3+u^2\right)+C_2 \right)\\
\log (x)+C_1&=-\frac{1}{3} \left(\log \left(u(3+u^2)\right)+C_2\right) \\
\log (x)+C_1&=\log\left(\sqrt[-3]{u(3+u^2)}\right) + C_3\\
x&=\exp \left(\log \left(\sqrt[-3]{u(3+u^2)}\right) +C_4\right) \\
x&=\exp^{C_4}\sqrt[-3]{u(3+u^2)}\\
x&=C_5\sqrt[-3]{u(3+u^2)} \\
\left(\frac{x}{C_5}\right)^{-3}=\frac{C_5^3}{x^3}&=3u+u^3 \\ 
u^3+3u-\frac{C_6}{x^3}&=0 \\ 
\frac{y^3}{x^3}+\frac{3 y}{x}-\frac{C_6}{x^3}&=0 \\ 
y^3+3 x^2 y-C_6&=0 \\
y^3+3yx^2&=C
\end{aligned}
$$



### Validation
This is an [[../Definitions/Implicit function|Implicit function]], so it’s an [[../Definitions/Implicit function#Differentiation|Implicit derivative]]. Then we can isolate $y^\prime$

$$
\begin{aligned}
\frac{d}{dx}\left(y^3+3 x^2 y+C\right) &= 3 y^2 y^{\prime}+6 x y+3 x^2 y^{\prime}=0 \\
\Leftrightarrow y^{\prime}&=\frac{-2 x y}{x^2+y^2}
\end{aligned}
$$

Next, plug it in the initial problem
$$
\begin{aligned}
& 2 x y+\left(x^2+y^2\right) y^{\prime} \\
= & 2 x y+\left(x^2+y^2\right) \frac{-2 x y}{\left(x^2+y^2\right.} \\
= & 2 x y-2 x y \\
= & 0
\end{aligned}
$$


> [!Error] What went wrong
> Assuming my solution was not good because it’s an implicit function


## Example
#example  #substitution_rule 

### Statement
$\left(x+\sqrt{y^{2}-x y}\right)\frac{dy}{dx}-y =0$.

### Solution


$$
\begin{aligned}
\left(x+\sqrt{y^2-x y}\right)y^\prime-y&=0 \\
\left(x+\sqrt{u^2 x^2-u x^2}\right)\left(u^{\prime} x+u\right)-u x&=0 \\
u^2 x^2+u x+u^{\prime} x|x| \sqrt{u^2-u}+u|x| \sqrt{u^2-u}-u x&=0 \\
\begin{drcases}
   u^{\prime} x \pm u^{\prime} x \sqrt{u^2-u} \pm u \sqrt{u^2-u}
\end{drcases} &=0 \qquad \begin{array}{}\text{upper if } x\gt 0 \\ \text{lower if } x\lt 0\end{array} \\
u^{\prime} x\left(1 \pm \sqrt{u^2-u}\right)&=\mp u \sqrt{u^2-u} \\
x&=\mp \frac{u \sqrt{u^2-u}}{u^{\prime}\left(1 \pm \sqrt{u^2-u}\right)} \\
\frac{1}{x} &=\mp \frac{\left(1 \pm \sqrt{u^2-u}\right)}{u \sqrt{u^2-u}} \\
\frac{1}{x} & =\mp \left(\frac{1}{u \sqrt{u^2-u}} \pm \frac{\sqrt{u^2-u}}{u \sqrt{u^2-u}}\right) u^{\prime} \\
\frac{1}{x} &=\mp \frac{1}{u \sqrt{u^2-u}} u^{\prime}-\frac{1}{u} u^{\prime}
\end{aligned}
$$

To do the integration, we will need to use [[../../Calculus/Integration/Substitution rule|Substitution rule]] with $u=\sec ^2 \theta, du =2 \sec ^2 \theta \tan \theta d\theta$. We will have to isolate $\theta=\operatorname{arcsec}(\sqrt{u})$, so we have the condition that $u\ge 0$.
We are also keeping track of $\mp$, upper sign for when $x \gt 0$ and lower sign when $x\lt0$.

$$
\begin{aligned}
\int \frac{1}{x} dx &=\mp \int \frac{1}{u \sqrt{u^2-u}} \frac{du}{dx} dx-\int \frac{1}{u} \frac{du}{dx} dx\\
\log x + C&= \mp \int \frac{2 \sec ^2 \theta \tan \theta}{\sec ^2 \theta \sqrt{\sec ^2 \theta\left(\sec ^2 \theta-1\right)}} d \theta - \int \frac{1}{u} du \\
\log x&=\mp \int \frac{2 \sec ^2 \theta \tan \theta}{\sec ^2 \theta \sqrt{\sec ^2 \theta \tan ^2 \theta}} d \theta - \log u + C\\
\log x&= \mp \int \frac{2 \cancel{\sec ^2 \theta \tan \theta}}{\sec ^\cancelto{1}{3} \theta \cancel{\tan \theta}} d \theta -\log u + C\\
\log x&= \mp \int \frac{2}{\sec \theta} d\theta - \log u + C \\
\log x&= \mp2 \int \cos(\theta) d\theta - \log u + C \\
\log x&= \mp2 \sin(\theta) - \log u + C \\
\log x&= \mp2 \sin(\operatorname{arcsec(\sqrt{u})}) - \log u + C \\
\cancel{\log x}&= \mp2 \sin(\operatorname{arcsec(\sqrt{u})}) - \log y + \cancel{\log x} + C \\
\log y&= \mp2 \sin(\operatorname{arcsec(\sqrt{u})}) + C \\
\end{aligned}
$$

$\sin(\operatorname{arcsec}(x)) = \frac{\sqrt{x^2 -1}}{x}$ is apparently a known thing. Had to use Wolfram Alpha to learn this

$$
\begin{aligned}
\log y&= \mp2 \frac{\sqrt{\left(\sqrt{u}\right)^2 - 1}}{\sqrt{u}} + C \\
\log y&= \mp2 \frac{\sqrt{u - 1}}{\sqrt{u}} + C \\
\log y&= \mp2 \frac{\sqrt{\frac{y}{x} - 1}}{\sqrt{\frac{y}{x}}} + C \\
\log y&= \mp2 \frac{\cancel{\sqrt{\frac y x}}\sqrt{1-\frac{x}{y}}}{\cancel{\sqrt{\frac{y}{x}}}} + C \\
\end{aligned}
$$
We are adding the condition that $y \neq 0$ (division by zero otherwise). Additionally, $\displaystyle \left \lvert \frac{x}{y} \right \lvert < 1$ because of the square root
$$
\begin{aligned}
\log y&= \mp2 \sqrt{1-\frac{x}{y}} + C \\
y&= e^{\mp2 \sqrt{1-\frac{x}{y}} + C} \\
y&= Ce^{\mp2 \sqrt{1-\frac{x}{y}}} \\
\end{aligned}
$$

Given our main conditions:
- $u \ge 0$
- $x \neq 0$
- $y \neq 0$
- $\displaystyle \left \lvert \frac{x}{y} \right \lvert < 1$
And the two cases for $x$, we have two solutions


$$
\boxed{
\begin{array}{r}
y(x)= \textcolor{#A3BE8C}{Ce^{-2 \sqrt{1-\frac{x}{y}}}},\qquad 0 \lt x \lt y, y \gt 0 \\
y(x)=\textcolor{#D08770}{Ce^{2 \sqrt{1-\frac{x}{y}}}} ,\qquad y \lt x \lt 0, y \lt 0
\end{array}
}
$$

```python
from sympy import symbols, Eq, plot_implicit, exp, sqrt, Le
x, y = symbols('x y')
plot_implicit(
    expr=Eq(y/exp(-2*sqrt(1-x/y)), 5),  
    y_var=(y,0, 5),  
    adaptive=False
)
```

## Example 
#example #substitution_rule 

### Statement
$x+y -(x-y) \frac{dy}{dx}=0$.

### Solution

$$
\begin{aligned}
x+y-(x-y) y^{\prime}&=0 \\
x+u x-(x-u x)\left(u^{\prime} x+u\right)&=0 \\
x+u x-\left(u^{\prime} x^2+u x-u^{\prime} u x^2-u^2 x\right)&=0 \\
x-u^{\prime} x^2+u^{\prime} u x^2+u^2 x&=0 \\
1-u^{\prime} x+u^{\prime} u x+u^2&=0 \\
x u^{\prime}(u-1)&=-\left(1+u^2\right) \\
x &=\frac{-(u-1)}{u^2+1} u^{\prime} \\
\frac{1}{x}&=\left(\frac{-u}{u^2+1}+\frac{1}{u^2+1}\right) u^{\prime}
\end{aligned}
$$
Integrate on both sides with respect to $x$
$$
\begin{aligned}
\int \frac{1}{x} dx=-\textcolor{orange}{\int \frac{u}{u^2+1} \frac{du}{\cancel{dx}} \cancel{dx}}+\textcolor{violet}{\int \frac{1}{u^2+1} \frac{d u}{\cancel{d x}} \cancel{d x}} \\
\end{aligned}
$$

In orange we have to do a variable subsitution $\omega=u^2, d \omega=2 u du$
$$
\begin{aligned}
\textcolor{orange}{\int \frac{u}{u^2+1} d u}=\frac{1}{2} \int \frac{1}{\omega+1} d \omega=\frac{1}{2} \log (\omega+1)+C = \frac{1}{2} \log (u^2 + 1)+C \\
\end{aligned}
$$

$$
\begin{aligned}
\log (x)=-\textcolor{orange}{\frac{1}{2} \log \left(u^2+1\right)}+\textcolor{violet}{\arctan (u)+C} \\
\log (x)=-\frac{1}{2} \log \left(\frac{y^2+x^2}{x^2}\right)+\arctan \left(\frac{y}{x}\right)+C \\
\log (x)=-\frac{1}{2}\left(\log \left(y^2+x^2\right)-\log \left(x^2\right)\right)+\arctan \left(\frac y x \right)+C \\
\cancel{\log (x)}=-\frac{1}{2} \log \left(y^2+x^2\right)+\cancel{\log (x)}+\arctan \left(\frac{y}{x}\right)+C \\
C=\arctan \left(\frac{y}{x}\right)-\frac{1}{2} \log \left(y^2+x^2\right)
\end{aligned}
$$




## Example
#example 

### Statement
$x y^{\prime}-y-x \sin \left(\frac y x \right)=0$.

### Solution

$$
\begin{gathered}
x y^{\prime}-y-x \sin \left(\frac{y}{x}\right)=0 \\
x\left(u^{\prime} x+u\right)-u x-x \sin (u)=0 \\
u^{\prime} x^2+u x-u x-x \sin (u)=0 \\
u^{\prime} x-\sin (u)=0 \\
x=\frac{\sin (u)}{u^{\prime}} \\
\frac{1}{x}=\frac{1}{\sin (u)} u^{\prime} \\
=\csc (u) u^{\prime}
\end{gathered}
$$


$$
\begin{aligned}
\int \frac{1}{x} d x & =\int \csc (u) \frac{d u}{d x} d x \\
\log (x) & =\log \left(\sin \left(\frac{u}{2}\right)\right)-\log \left(\cos \left(\frac{u}{2}\right)\right)+C \\
x & =\exp \left(\log \left(\sin \left(\frac{u}{2}\right)\right)-\log \left(\cos \left(\frac{u}{2}\right)\right)+C\right) \\
x & =\frac{C \sin \left(\frac{u}{2}\right)}{\cos \left(\frac{u}{2}\right)} \\
x & =C \tan \left(\frac{u}{2}\right) \\
\arctan \left(\frac{x}{C}\right) & =\frac{u}{2}=\frac{y}{2 x} \\
y & =2x\arctan \left(Cx\right)
\end{aligned}
$$

## Example
#example #partial_fraction 

### Statement
$\left(2 x^{2} y+y^{3}\right)+\left(x y^{2}-2 x^{3}\right)y^\prime=0$

### Solution

$$
\begin{align}
\left(2 x^2 y+y^3\right)+\left(x y^2-2 x^3\right) y^{\prime}&=0 \\
2 x^2(u x)+(u x)^3+\left(x(u x)^2-2 x^3\right)\left(u^{\prime} x+u\right)&=0 \\
2 ux^3+u^3x^3+\left(u^2 x^3-2 x^3\right)\left(u^{\prime} x+u\right)&=0 \\
\cancel{2 x^3u}+u^3 x^3+u^2 u^{\prime} x^4+u^3 x^3-2 u^{\prime} x^4-\cancel{2 u x^3}&=0 \\
x^3\left(2u^3+u^2 u^{\prime} x-2 u^{\prime} x\right)&=0 \\
u^{\prime} x\left(u^2-2 \right)&=-2u^3 \\
x&=-\frac{2u^3}{u^{\prime}(u^2-2)} \\
\frac{2}{x}&=\frac{2-u^2}{u^3} u^{\prime}
\end{align}
$$

From there we can take the integral on both sides with respect to $x$

$$
\begin{aligned}
\int \frac{2}{x} d x&=\int \frac{2}{u^3} \frac{du}{dx}dx-\int \frac{u^2}{u^3} \frac{du}{dx}dx \\
\int \frac{2}{x} d x&=\int \frac{2}{u^3} \frac{du}{dx}dx-\int \frac{u^2}{u^3} \frac{du}{dx}dx \\
2 \log (x)&=-u^{-2}-\log (u) \\
2 \log (x)&=-\frac{x^2}{y^2}-(\log (y)-\log (x))+C \\
\log (x)+\frac{x^2}{y^2}+\log (y)&=C \\
\log (x y)+\frac{x^2}{y^2}&=C
\end{aligned}
$$

## Example
#example 

### Statement
$y^{2}+\left(x \sqrt{y^{2}-x^{2}}-x y\right)y^\prime=0$

## Example
#example 

### Statement
$\frac{y}{x} \cos \frac{y}{x} d x-\left(\frac{x}{y} \sin \frac{y}{x}+\cos \frac{y}{x}\right) d y=0$

## Example
#example 

### Statement
$y d x+x \log \frac{y}{x} d y-2 x d y=0$

## Example
#example 

### Statement
$2 y e^{x / y} d x+\left(y-2 x e^{x / y}\right) d y=0$

## Example
#example 

### Statement
$\left(x e^{y / x}-y \sin \frac{y}{x}\right) d x+x \sin \frac{y}{x} d y=0$

## Example
#example 

### Statement

Find a particular solution, satisfying the initial condition, of each of the following differential equations.

$\left(x^{2}+y^{2}\right) d x=2 x y d y, \quad y(-1)=0$

## Example
#example 

### Statement
$\left(x e^{y / x}+y\right) d x=x d y, \quad y(1)=0$

## Example
#example 

### Statement
$y^{\prime}-\frac{y}{x}+\csc \frac{y}{x}=0, \quad y(1)=0$

## Example
#example 

### Statement
$\left(x y-y^{2}\right) d x-x^{2} d y=0, y(1)=1$

## 2. ANSWERS 7


6. $\frac{x^{2}}{y^{2}}+\log x y=c, \quad x \neq 0, \quad y \neq 0$.
7. $y^{2}-c x=y \sqrt{y^{2}-x^{2}}$, or equivalently, $c\left(y+\sqrt{y^{2}-x^{2}}\right)=x y, y^{2}>x^{2}$.
8. $y \sin \frac{y}{x}=c$.
9. $y=c(1+\log x / y)$.
10. $2 e^{x / y}+\log y=c$.
11. $\log x^{2}-e^{-y / x}\left(\sin \frac{y}{x}+\cos \frac{y}{x}\right)=c$.
12. $y^{2}=x^{2}+x$.
13. $\log x+e^{-y / x}=1$.
14. $\log x-\cos \frac{y}{x}+1=0$.
15. $x=e^{(x / y)-1}$.