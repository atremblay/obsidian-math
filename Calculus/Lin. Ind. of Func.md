---
alias: 
    - Linear independance of functions
    - linear independance of functions
---

A set of functions $f_1(x),f_2(x), \ldots, f_n(x)$, each defined on a common interval $I$, is called **linearly dependant** on $I$, if there exsists a set of constants $c_1, c_2, \ldots, c_n$, not *all zeroes* such that 

$$
\sum_{i=1}^n c_if_i(x) = 0 
$$
for *every* x in $I$.

> Could be a trick question, all functions must be defined on the interval $I$. This is something that we often assume, but it's not always the case.

If no such set of constants exists, then the set of functions is called **linearly independant**.

$y_1$ and $y_2$ are *linearly independant* on an interval if they are not a scalar multiple of each other. Pretty similar to linear independance in linear algebra

Ex: $\sin x$ and $\cos x$ are linearly independant
Ex: $x^2$ and $2x^2$ are linearly dependant

More generally, $y_1,\ldots,y_n$ are *linearly independant* on an interval if $\sum a_iy_i = 0$ implies $a_1=\ldots=a_n=0$.


# Wronskian
The Wronskian is another way to figure out if a set of functions are linearly dependant or not. We need to find the determinant of the matrix composed of all the functions and their derivative up to $n-1$. Here is the case with 3 functions
$$
W(t)=\left|
\begin{array}{l}
 y_1 & y_2 & y_3     \\
 y_1' & y_2' & y_3' \\
 y_1'' & y_2'' & y_3''
\end{array}

\right|
$$

- If the determinant is zero, then the functions are linearly dependant. 
- If the determinant is not zero, then the functions are linearly independant.

Example

$y_1=e^t$ and $y_2=te^t$

$$
\begin{align}
W(t)=\left|
\begin{array}{l}
 y_1 & y_2      \\
 y_1' & y_2'  \\
\end{array}
\right| &= 
\left|
\begin{array}{l}
 e^t & te^t      \\
 e^t & te^t+e^t  \\
\end{array}
\right| \\ 
&= e^t(te^t+e^t) - te^{2t} \\
&= \cancel{te^{2t}}+e^{2t} - \cancel{te^{2t}} \\
&= e^{2t}
\end{align}
$$
So linearly independant. Notice that $t$ is not a constant but the independant variable. Had we use a constant then the determinant would have been zero.
