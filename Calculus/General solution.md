

> [!Summary]
> A general solution will involve one or more undefined constant factors.
> 
> > [!example]
> > The solution of $y'''=e^x$ is $y=e^x + c_1x^2 + c_2x + c_3$ (need to integrate the DE three times). As soon as we set values for $c_1, c_2, \ldots, c_n$ then we have a specific solution.


#### Definition

The *functions* defined by 
$$y=f(x, c_1, c_2, \ldots, c_n)$$

of the $(n+1)$ variables $x, c_1, c_2, \ldots, c_n$ will be called an **n-parameter family of solutions** of the *nth* order differential equation
$$F(x, y, y', y'', ..., y^{(n)})=0,$$
if for each choice of a set of values $c_1, c_2, \ldots, c_n$ the resulting *function f(x)* (now only a function of *x*) satisfies
$$F(x, f(x), f'(x), f''(x), ..., f^{(n)}(x))=0$$

In other words, set the constants to something, plug it in the DE and if it equals zero then you have a solution. This needs to be valid for any choice of constants.

> $f(x)$ is one solution among many. This is NOT a general solution

Need to be careful with the [[Function#Domain and Range|domain]] on which the function is defined. $xy'=1$ has *no* solution in the interval $-1 < x < 1$. Formally, $y=log|x| + x$.

$$
y= 
\begin{cases}
log(-x) + c,&  x<0\\
log(x) + c,&  x>0
\end{cases}
$$

This solution only exists in one of the domain and neither includes $x=0$. In other words, we need to reduce the domain of the DE if we want the solution to be valid.

### Finding a DE if its *n*-parameter family of solutions is known

If we are given a general solution with *n* constants, then it is most likely an *nth* order DE. Derive the solution *n* times then use a bit of imagination to find a DE where all the constants are no longer there.

e.g. solution $y=c \cos x+x$. What is the DE?

$y'=-c \sin x + 1$. Since we have only one constant, need only the first derivative. This is not the DE because we still have the constant *c* in there. Multiply the solution by $\sin x$ and the first derivative by $\cos x$ and add everything

$$
\begin{align}
y\sin x + y'\cos x &= c\sin x \cos x + x\sin x -c \sin x \cos x + \cos x \\
&= x \sin x + \cos x \\
\iff y' &=  (x-y) \tan x + 1, x \ne \pm \frac{\pi}{2}, \pm \frac{3\pi}{2}, \ldots  
\end{align}
$$

There is no magical solutions. Some problems will required quite a bit of fucking around.