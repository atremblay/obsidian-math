---
alias:
    - "Undetermined coefficient for non-homogeneous ODE"
tags:
    - "non_homogeneous"
    - "solution"
    - "second_order"
---

> [!summary]
> The [[../../Calculus/General solution]] of
> $$
> \sum_{i=0}^{n}a_{i}x^{(i)}=Q(t)
> $$
> Where $a_i\neq 0$ and $Q(t) \not\equiv 0$ on an interval $I$ is
> $$x(t)=x_h(t)+x_p(t)$$
> where $x_p$ is the particular solution of $Q(t)$ and $x_h$ is the [[Const Coeff Homo DE|general solution of the related homogeneous version]]. 


> [!Caution]
> The method of undetermined coefficient can only be used if $Q(x)$ consists of a sum of terms each of which has a finite number of linearly independant derivatives. 

A differential equation that has something other than zero on the right-hand side. For example:
$$y''-2y'-3y=3e^{2t}$$
#### [[../../Calculus/General solution]]

Particular solution $\textcolor{yellow}{y_p}$ solves non-homogeneous. Because of [[../Existence and uniqueness#Theorem Existence Uniqueness|uniqueness]], $\textcolor{yellow}{y_p}$ is the only solution to the non-homogeneous equation. 
$\textcolor{cyan}{y_h}$ solves the homogeneous version of the same equation $y''-2y'-3y=0$

If we add both solutions together as a new solution, then $\textcolor{cyan}{y_h}+\textcolor{yellow}{y_p}$ solves the non-homogeneous equation.
$$
\begin{align}
&(\textcolor{cyan}{y_h}+\textcolor{yellow}{y_p})''-2(\textcolor{cyan}{y_h}+\textcolor{yellow}{y_p})'-3(\textcolor{cyan}{y_h}+\textcolor{yellow}{y_p}) \\
=&(\textcolor{cyan}{y_h''-2y_h'-3y_h}) + (\textcolor{yellow}{y_p''-2y_p'-3y_p})\\
=&0+3e^{2t}
\end{align}
$$

Suppose we have two different particular solutions to the non-homogeneous equation, $y_1$ and $y_2$. Then $y_1-y_2$ solves the homogenous equation

$$
\begin{align}
&(\textcolor{cyan}{y_1}-\textcolor{yellow}{y_2})''-2(\textcolor{cyan}{y_1}-\textcolor{yellow}{y_2})'-3(\textcolor{cyan}{y_1}-\textcolor{yellow}{y_2}) \\
=&(\textcolor{cyan}{y_1''-2y_1'-3y_1}) - (\textcolor{yellow}{y_2''-2y_2'-3y_2})\\
=&3e^{2t}-3e^{2t} \\
=&0
\end{align}
$$

# Methodology
Here is the methodology to use with the same example as above weaved-in

### Step 1: Solve the homogeneous
See [[Const Coeff Homo DE|constant coefficient homogeneous differential equation]]. The [[../../Calculus/General solution]] the the homogeneous DE is
$$\textcolor{cyan}{y_h=c_1e^{3t}+c_2e^{-t}}$$
With the guess $y=e^{rt}$

### Step 2: Solve a solution to non-homogeneous
It's similar to the homogeneous version, but the guess is slightly different. We are guessing something that looks like the right-hand side
$$\textcolor{yellow}{y_p}=\textcolor{orange}{A}e^{2t}$$
Must be careful with the guess to make sure it does not overlap with on of the homogeneous solutions (see below). Plug this in
$$
\begin{align}
&\cancel{A4e^{2t}} - \cancel{A4e^{2t}}-A3e^{2t}=3e^{2t} \\
\iff&-A3e^{2t}=3e^{2t} \\
\iff&\textcolor{orange}{A}=-1
\end{align}
$$
This is **not** the [[../../Calculus/General solution]], but a particular solution.

### Step 3: [[../../Calculus/General solution]]
The [[../../Calculus/General solution]] is 
$$
\begin{align}
y_g&=\textcolor{cyan}{y_h}+\textcolor{yellow}{y_p} \\
&=\textcolor{cyan}{c_1e^{3t}+c_2e^{-t}} \textcolor{yellow}{-e^{2t}}
\end{align}$$
If we had initial condition this is where we would plug them, in the [[../../Calculus/General solution]]. Not at any other time before.

# Heads up
Cannot be dumb about the guess. This is mostly plug and play, but we still have to think a little bit.

If the non-homogeneous ODE was (right-hand side is different)
$$y''-2y'-3y=\textcolor{pink}{3e^{-t}}$$
The homogeneous [[../../Calculus/General solution]] would still be $\textcolor{cyan}{y_h=c_1e^{3t}+c_2e^{-t}}$ (because it's still equal to zero). We still get inspiration from the right-hand side for the guess, but it cannot be $y_p=Ae^{\textcolor{orange}{-t}}$  because it's one of the particular solution to the homogeneous equation. It's a simple scalar multiplication away from the second term of $\textcolor{cyan}{y_h}$.

We then have to use a similar trick as in [[Const Coeff Homo DE#Case 2 Repeated root|repeated roots]]
$$\textcolor{yellow}{y_p}=A\textcolor{violet}{t}e^{-t}$$
$$
\begin{align}
&y''-2y'-3y&=3e^{-t} \\
\iff &(Ate^{-t})'' - 2(Ate^{-t})' - 3Ate^{-t} &= 3e^{-t} \\
\iff &A(-te^{-t} + e^{-t})' - 2A(-te^{-t} + e^{-t}) - 3Ate^{-t}&= 3e^{-t} \\
\iff &A(te^{-t} - e^{-t} - e^{-t}) - 2A(-te^{-t} + e^{-t}) - 3Ate^{-t}&= 3e^{-t} \\
\iff &\cancel{Ate^{-t}}- 2Ae^{-t} +\cancel{2Ate^{-t}} -2Ae^{-t} - \cancel{3Ate^{-t}}&= 3e^{-t} \\
\iff &-4Ae^{-t} &= 3e^{-t} \\
\iff &A &= -\frac{3}{4} \\
\end{align}
$$

So the [[../../Calculus/General solution]] is 
$$
\begin{align}
y_g&=y_h+\textcolor{yellow}{y_p}\\
&=\textcolor{cyan}{c_1e^{3t}+c_2e^{-t}} - \textcolor{yellow}{\frac{3}{4}te^{-t}} 
\end{align}
$$


# What to guess
You can mix and match any of these depending on what is on the right-hand side

| Non-homogeneity | Guess |
|:---:|:---:|
| $e^{rt}$ | $Ae^{rt}$ |
| $\sin(rt)$ or $\cos(rt)$ | $A\sin(rt) + B\cos(rt)$ |
| Degree $n$ polynomial | $A_0 + A_1t + \ldots + A_nt^n$ |

- Multiply by $t^s$ as needed to avoid matching homogeneous
- Add/Multiply different types together

Example
$$y''-2y'-3y=t^2 + 3e^{-t}\cos(4t)$$
Has all of the above. So the guess would be
$$\textcolor{yellow}{y_p=(A+Bt+Ct^2) + D(e^{-t}\cos(4t)) + E(e^{-t}\sin(4t))}$$
>The polynomial includes all degrees, not just $t^2$.

Homogeneous [[../../Calculus/General solution]] stays the same, $\textcolor{cyan}{y_h=c_1e^{3t}+c_2e^{-t}}$. There is no need to multiply anything by $t^s$ because none of the things in $\textcolor{yellow}{y_p}$ is a solution of the homogeneous case.

## Constraints
$$
\sum_{i=0}^{n}a_{i}y^{(i)}=Q(x)
$$
1. $a_i$ are constants
2. $Q(x)$ is a function which has finite number of linearly independant derivatives

## Cases

### Case 1
No term of $Q(x)$ is the same as a term of $y_h$ . In this case a particular solution $y_p$ will be a linear combination of the terms in $Q(x)$ and **all of its linearly independant derivatives**. That's why the terms must have a finite number of derivatives. If the number of derivatives is infinite then we need [[Variation of Parameters]].

> [!Todo] 
> After a few weeks, the previous paragraph makes no sense. Why should $Q$ have finite number of derivatives?


### Case 2
$Q(x)$ contains a term which, ignoring constant coefficients, is $x^k$ times a term $u(x)$ of $y_h$, where $k \geq 0$. In $y_p$ will then be a linear combination of $x^{k+1}u(x)$ and **all it's linearly independant derivatives** (ignoring constant coefficients). If in addition $Q(x)$ contains terms which belong to [[Undetermined Coefficient#Case 1|Case 1]], then the proper terms called for by this case must be included in $y_p$.

### Case 3
Applicable only if both of the following are fulfilled. 

1. The [[../../Calculus/Characteristic equation|characteristic equation]] has an $r$ multiple root
2. $Q(x)$ contains a term which, ignoring constant coefficients, is $x^k$ times a term $u(x)$ in $y_p$ where $u(x)$ was obtained from the $r$ multiple root. 

In this case, a particular solution $y_p$ will be a linear combination of $x^{k+r}u(x)$ and **all it's linearly independant derivatives**. If in addition $Q(x)$ contains terms which belong to [[Undetermined Coefficient#Case 1|Case 1]] and [[Undetermined Coefficient#Case 2|Case 2]], then the proper terms called for by theses cases must also be added to $y_p$.


> [!Example] 
> $$\ddot{x}+3\dot{x}+2x=e^{-3t}$$
> - [ ] Complete this example
> $$\ddot{x}+3\dot{x}+2x=\cos{\omega t}$$
> 

# Additionnal ressources
[Steve Brunton](https://youtu.be/Jvmb2jeRaGE)