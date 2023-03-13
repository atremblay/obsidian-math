---
aliases:
    - "generalized eigenvectors"
    - "generalized eigenvector"
---

> [!Summary] 
> The rank $m$ generalized eigenvector $\mathbf{x}_m$ is defined as
> $$(\mathbf{A}-\lambda\mathbf{I})^m\mathbf{x}_m=\mathbf{0}$$
> or
> $$(\mathbf{A}-\lambda\mathbf{I})\mathbf{x}_m=\mathbf{x}_{m-1}$$

Given a [[Defective matrix]], we need additional eigenvectors in order to have a complete eigenspace to [[Decoupling a system of ODE|decouple a system of ODE]].

The following [[defective matrix]]
$$
\mathbf{A}=
\begin{bmatrix}
6 & -2 & -1 \\
3 & 1 & -1 \\
2 & -1 & 2 \\
\end{bmatrix}
$$
has a single eigenvalue $\lambda=3$ with multiplicity 3 (i.e. same eigenvalue 3 times), but it has only 1 eigenvector
$$\mathbf{x}=\begin{bmatrix}1\\1\\1\end{bmatrix}$$
> [!Question] 
> Unclear where the next bloc comes from. Is this a convenient definition or it has some mathematical basis?

Finding generalized eigenvectors is an iterative process. It will continue for as long as there are missing eigenvectors. The first one is based on the normal eigenvector $\mathbf{x}_1$. The second one is based on the first generalized eigenvector, etc.

$$
(\mathbf{A}-\lambda\mathbf{I})\mathbf{x}_m=\mathbf{x}_{m-1}
$$

$$
\mathbf{A} - 3\mathbf{I} =
\begin{bmatrix}
3 & -2 & -1 \\
3 & -2 & -1 \\
2 & -1 & -1 \\
\end{bmatrix}
$$

## First generalized eigenvector is 
$$
\begin{align}
(\mathbf{A}-\lambda\mathbf{I})\mathbf{x}_2&=\mathbf{x}_{1} \\
\begin{bmatrix}
    3 & -2 & -1 \\ 3 & -2 & -1 \\ 2 & -1 & -1 \\
\end{bmatrix}
\begin{bmatrix}x_{21}\\x_{22}\\x_{23}\end{bmatrix} &= \begin{bmatrix}1\\1\\1\end{bmatrix}
\end{align}
$$
Skipping a bit of the math
$$
\mathbf{x}_2=\begin{bmatrix}0\\0\\-1\end{bmatrix}
$$


> [!Question]
> This means that the first eigenvector $\mathbf{x}_1$ is in the column space of $(\mathbf{A}-\lambda\mathbf{I})$. It's a linear combination of the columns. Important?
> 

## Second generalized eigenvector is 
$$
\begin{align}
(\mathbf{A}-\lambda\mathbf{I})\mathbf{x}_3&=\mathbf{x}_{2} \\
\begin{bmatrix}
    3 & -2 & -1 \\ 3 & -2 & -1 \\ 2 & -1 & -1 \\
\end{bmatrix}
\begin{bmatrix}x_{31}\\x_{32}\\x_{33}\end{bmatrix} &= \begin{bmatrix}0\\0\\-1\end{bmatrix}
\end{align}
$$
Skipping a bit of the math
$$
\mathbf{x}_3=\begin{bmatrix}0\\-1\\2\end{bmatrix}
$$

## The end
$$
\begin{align}
(\mathbf{A}-\lambda\mathbf{I})\mathbf{x}_4&=\mathbf{x}_{3} \\
\begin{bmatrix}
    3 & -2 & -1 \\ 3 & -2 & -1 \\ 2 & -1 & -1 \\
\end{bmatrix}
\begin{bmatrix}x_{41}\\x_{42}\\x_{43}\end{bmatrix} &= \begin{bmatrix}0\\-1\\2\end{bmatrix}
\end{align}
$$
Has no solution. No more generalized eigenvectors. We have a complete eigenbasis.

# Generalized modal matrix $\mathbf{P}$

The matrix $\mathbf{P}$ built from the generalized eigenvectors is invertible

> [!Question]
> This matrix is invertible due to [[Invertible matrix#^3113ab|this property of an invertible matrix]]. But how are we sure that the columns are always linearly independant?

$$
\mathbf{P}=\begin{bmatrix} \mathbf{x}_1 & \mathbf{x}_2 & \mathbf{x}_3 \end{bmatrix} 
= \begin{bmatrix}
1 & 0 & 0 \\ 1 & 0 & -1 \\ 1 & -1 & 2
\end{bmatrix}, \hspace{2em}
\mathbf{P}^{-1}=\begin{bmatrix}
1 & 0 & 0 \\ 3 & -2 & -1 \\ 1 & -1 & 0
\end{bmatrix}
$$

$$
\mathbf{P}\mathbf{P}^{-1}=\begin{bmatrix}
1 & 0 & 0 \\ 1 & 0 & -1 \\ 1 & -1 & 2
\end{bmatrix}
\begin{bmatrix}
1 & 0 & 0 \\ 3 & -2 & -1 \\ 1 & -1 & 0
\end{bmatrix}
=\begin{bmatrix}
1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1
\end{bmatrix}
$$

$$
\mathbf{P}^{-1}\mathbf{P}=
\begin{bmatrix}
1 & 0 & 0 \\ 3 & -2 & -1 \\ 1 & -1 & 0
\end{bmatrix}
\begin{bmatrix}
1 & 0 & 0 \\ 1 & 0 & -1 \\ 1 & -1 & 2
\end{bmatrix}
=\begin{bmatrix}
1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1
\end{bmatrix}
$$
# [[Jordan form]]

$$
\begin{align}
\mathbf{P^{-1}AP} &= \mathbf{J}\\

\begin{bmatrix}
    1 & 0 & 0 \\ 3 & -2 & -1 \\ 1 & -1 & 0
\end{bmatrix}
\begin{bmatrix}
    6 & -2 & -1 \\ 3 & 1 & -1 \\ 2 & -1 & 2 \\
\end{bmatrix}
\begin{bmatrix}
    1 & 0 & 0 \\ 1 & 0 & -1 \\ 1 & -1 & 2
\end{bmatrix}
&= \begin{bmatrix}
    3 & 1 & 0 \\ 0 & 3 & 1 \\ 0 & 0 & 3
\end{bmatrix}
\end{align}
$$
[Wikipedia](https://en.wikipedia.org/wiki/Generalized_eigenvector)


> [!todo] 
> 1. These notes have been scrambled together. This is not organized. Organize it better.
> 2. Link it to ODE solution [[Decoupling a system of ODE]]

$\mathbf{J}$ is the diagonal matrix with eigenvalues with ones on top of it.

$$
\mathbf{J}=
\underbrace{
\begin{bmatrix}
    \lambda & 0 & 0 \\ 0 & \lambda & 0 \\ 0 & 0 & \lambda
\end{bmatrix}}_{\mathbf{S}} + 
\underbrace{
\begin{bmatrix}
    0 & 1 & 0 \\ 0 & 0 & 1 \\ 0 & 0 & 0
\end{bmatrix}}_{\mathbf{T}}
$$
Using this [[Matrix exponential#^06052f|matrix exponential property]]
$$
e^{\mathbf{J}t}=e^{(\mathbf{S+T})t}=e^{\mathbf{S}t}e^{\mathbf{T}t}
$$

$$
e^{\mathbf{S}t}=
\begin{bmatrix}
    e^{\lambda t} & 0 & 0 \\ 0 & e^{\lambda t} & 0 \\ 0 & 0 & e^{\lambda t}
\end{bmatrix}
$$

Using the definition of [[Matrix exponential]] definition
$$
e^{\mathbf{T}t} = \mathbf{I} + \mathbf{T}t + \cancelto{0}{\frac{\mathbf{T}^2t^2}{2!}} + \cancelto{0}{\ldots} = 
\begin{bmatrix}
    1 & t & 0 \\ 0 & 1 & t \\ 0 & 0 & 1
\end{bmatrix}
$$

Go through the motion, $\mathbf{T}^2=\mathbf{0}$, so every terms after the second term equals zero.

$$
e^{\mathbf{S}t}e^{\mathbf{T}t} = 
\begin{bmatrix}
    e^{\lambda t} & 0 & 0 \\ 0 & e^{\lambda t} & 0 \\ 0 & 0 & e^{\lambda t}
\end{bmatrix}
\begin{bmatrix}
    1 & t & 0 \\ 0 & 1 & t \\ 0 & 0 & 1
\end{bmatrix} = 
\begin{bmatrix}
    e^{\lambda t} & te^{\lambda t}  & 0 \\ 0 & e^{\lambda t} & te^{\lambda t} \\ 0 & 0 & e^{\lambda t}
\end{bmatrix}
$$
