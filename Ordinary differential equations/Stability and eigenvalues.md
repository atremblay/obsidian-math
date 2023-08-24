
> [!Summary]
> [[System of ODE#Decoupled|Decouple]] a system of ODE if it's not already decoupled
> $$
> \begin{align}
> \mathbf{x}(t)&=e^{\mathbf{A}t}\mathbf{x}(0)\\
> &=\mathbf{T}e^{\mathbf{D}t}\mathbf{T}^{-1}\mathbf{x}(0)
> \end{align}
> $$
> 
> ![[Images/eigenstability.svg|200]]
> The eigenvalues are in the matrix $\mathbf{D}$
> If the eigen value has a negative real part, it acts as a sink (blue part)
> If the eigen value has a positive real part, it acts as a source (orange part)
> If the eigen value has no real part, it turns in circle (red axis)
> 
> > $\textcolor{lightblue}{\text{Stable}}$ 
> > All eigenvalues must have a negative real part to converge. 
> > $\textcolor{orange}{\text{Unstable}}$ 
> > If there is only one positive real part the system will blow up. 
> > $\textcolor{red}{\text{Neutrally Stable}}$ 
> > If all eigenvalues have only imaginary parts 




In a linear ODE there is only one [[Definitions/Fixed point|fixed point]] and that is the origin (the trivial solution)

The [[Solutions/Solving high-order ODE|solution]] to a linear ODE $\dot{\mathbf{x}}=\mathbf{A}\mathbf{x}$ is 
$$\mathbf{x}(t)=e^{\mathbf{A}t}\mathbf{x}(0)$$
The eigenvalues of $\mathbf{A}$ will tell us if the system is stable or unstable. [[System of ODE#Decoupling|Decouple]] the system to get a better view of what's happening. 
$$\mathbf{x}(t)=\mathbf{T}e^{\mathbf{D}t}\mathbf{T}^{-1}\mathbf{x}(0)$$
## Source
For example
$$
\mathbf{A}=\begin{bmatrix}3 & -1 \\ -1 & 3 \end{bmatrix}, \hspace{1em}
\mathbf{T}=\begin{bmatrix}\textcolor{green}{1} & \textcolor{orange}{1} \\ \textcolor{green}{1} & \textcolor{orange}{-1} \end{bmatrix}, \hspace{1em} 
\mathbf{D}=\begin{bmatrix}2 & 0 \\ 0 & 4 \end{bmatrix}, \hspace{1em}
\mathbf{T}^{-1}=\begin{bmatrix}0.5 & 0.5 \\ 0.5 & -0.5 \end{bmatrix}
$$
where $\mathbf{T}$ are the eigenvectors and $\mathbf{D}$ are the eigenvalues. Every point in the plane gets blown away. This is due to the application of 
$$e^{\mathbf{D}t}=\begin{bmatrix}e^{2t} & 0 \\ 0 & e^{4t} \end{bmatrix}$$
As $t\to \infty$, the entries go to $\infty$ as well, so every directions are going to infinity. 

![[Images/source.png|400]]

## Sink
![[Images/sink.png|400]]

## Saddle point
![[Images/saddle.png|400]]


# Neutrally stable

The eigenvalues can also be complex.

$$
\mathbf{A}=\begin{bmatrix}0 & 2 \\ -2 & 0 \end{bmatrix}, \hspace{1em}
\mathbf{T}=\begin{bmatrix}\textcolor{green}{1} & \textcolor{orange}{1} \\ \textcolor{green}{i} & \textcolor{orange}{-i} \end{bmatrix}, \hspace{1em} 
\mathbf{D}=\begin{bmatrix}2i & 0 \\ 0 & -2i \end{bmatrix}, \hspace{1em}
\mathbf{T}^{-1}=\frac 1 2 \begin{bmatrix}1 & -i \\ 1 & i \end{bmatrix}
$$

A very important thing to observe is that the values of $\mathbf{A}$ are real, time is real, the state $\mathbf{x}$ is also real, so we can't have a system that moves forward in time that is not real. Somewhere the imaginary part will cancel each other.
Brace yourself

$$
\begin{align}
\mathbf{x}(t)  &=\mathbf{T}e^{\mathbf{D}t}\mathbf{T}^{-1}\mathbf{x}(0) \\
&=
\frac 1 2
\begin{bmatrix} 1 & 1 \\ i & -i \end{bmatrix}
\begin{bmatrix}e^{2it} & 0 \\ 0 & e^{-2it} \end{bmatrix}
\begin{bmatrix} 1 & -i \\ 1 & i \end{bmatrix}
\mathbf{x}(0) \\
&=
\frac 1 2
\begin{bmatrix} 1 & 1 \\ i & -i \end{bmatrix}
\begin{bmatrix}e^{2it} & -ie^{2it} \\ e^{-2it} & ie^{-2it} \end{bmatrix}
\mathbf{x}(0) \\
&=
\frac 1 2
\begin{bmatrix}
    \textcolor{orange}{e^{2it}+e^{-2it}} &  
    \textcolor{violet}{-ie^{2it} + ie^{-2it}} \\ 
    \textcolor{olive}{ie^{2it}-ie^{-2it}} &  
    \textcolor{lightblue}{e^{2it}+e^{-2it}} 
\end{bmatrix}
\mathbf{x}(0)
\end{align}
$$
Let's go over each of these terms where we will see that the imaginary parts disappear. We will use:
- [[../Calculus/Euler's formula]] $e^{i\theta} = \cos \theta + i \sin \theta$
- [[../Trigonometry/Trigonometric identity#Opposite Angles|Opposite Angles]]  $\sin -\theta = -\sin \theta$  and $\cos -\theta = \cos \theta$

The building blocks are 
$$
\begin{align}
e^{2it} &= \cos(2t) + i\sin(2t) \\
e^{-2it}&= \cos(-2t) + i\sin(-2t) = \cos(2t) - i\sin(2t)
\end{align}
$$
$$
\begin{align}
\textcolor{orange}{e^{2it}+e^{-2it}} &=   \cos(2t) + \cancel{i \sin(2t)} +  \cos(2t) - \cancel{i \sin(2t)} \\
&=  2\cos(2t) \\
\end{align}
$$
$$
\begin{align}
\textcolor{violet}{i(-e^{2it}+e^{-2it})} &=  i(\cancel{-\cos(2t)} - i \sin(2t) +  \cancel{\cos(2t)} - i \sin(2t)) \\
&=  2\sin(2t) \\
\end{align}
$$
$$
\begin{align}
\textcolor{olive}{i(e^{2it}-e^{-2it})} &=  i(\cancel{\cos(2t)} + i \sin(2t) -  \cancel{\cos(2t)} + i \sin(2t)) \\
&=  -2\sin(2t) \\
\end{align}
$$
$$
\begin{align}
\textcolor{lightblue}{e^{2it}+e^{-2it}} &=   \cos(2t) + \cancel{i \sin(2t)} +  \cos(2t) - \cancel{i \sin(2t)} \\
&=  2\cos(2t) \\
\end{align}
$$

All the imaginary parts disappeared and we are left with a simple rotation matrix.
$$
\begin{align}
\mathbf{x}(t)  &=
\frac 1 2
\begin{bmatrix}
    \textcolor{orange}{2\cos(2t)} &  
    \textcolor{violet}{2\sin(2t)} \\ 
    \textcolor{olive}{-2\sin(2t)} &  
    \textcolor{lightblue}{2\cos(2t)} 
\end{bmatrix}
\mathbf{x}(0) \\
&=
\begin{bmatrix}
    \textcolor{orange}{\cos(2t)} &  
    \textcolor{violet}{\sin(2t)} \\ 
    \textcolor{olive}{-\sin(2t)} &  
    \textcolor{lightblue}{\cos(2t)} 
\end{bmatrix}
\mathbf{x}(0)
\end{align}
$$
![[Images/neutral_stable.png|400]]


> [!Question]
> The real part of a complex pair is the same, so they will both behave the same way. What if the system is 4x4? Can both pairs have different sign for the real part? I assume it's possible yet. 


## With Real part
The example above had no real part and everything is just rotating. Adding a real part will either make the system spiral in or spiral out. Let's add something on the diagonal of $\mathbf{A}$

If $\lambda=a+ib$ then 
$$e^{\lambda t} = e^{(a+ib)t}= e^{at+ibt}= e^{at}e^{ibt}$$
The real part $a$ can be positive or negative, which corresponds to the next two sections.

### Spiraling out/Spiral source
$$
\mathbf{A}=\begin{bmatrix}1 & 2 \\ -2 & 1 \end{bmatrix}, \hspace{1em}
\mathbf{T}=\begin{bmatrix}1 & 1 \\ i & -i \end{bmatrix}, \hspace{1em} 
\mathbf{D}=\begin{bmatrix}1+2i & 0 \\ 0 & 1-2i \end{bmatrix}, \hspace{1em}
\mathbf{T}^{-1}=\frac 1 2 \begin{bmatrix}1 & -i \\ 1 & i \end{bmatrix}
$$
$$
\begin{align}
\mathbf{x}(t)  &=\mathbf{T}e^{\mathbf{D}t}\mathbf{T}^{-1}\mathbf{x}(0) \\
&=
\frac 1 2
\begin{bmatrix} 1 & 1 \\ i & -i \end{bmatrix}
\begin{bmatrix}e^{(1+2i)t} & 0 \\ 0 & e^{(1-2i)t} \end{bmatrix}
\begin{bmatrix} 1 & -i \\ 1 & i \end{bmatrix}
\mathbf{x}(0) \\
&=
\frac 1 2
\begin{bmatrix} 1 & 1 \\ i & -i \end{bmatrix}
\begin{bmatrix}e^{(1+2i)t} & -ie^{(1+2i)t} \\ e^{(1-2i)t} & ie^{(1-2i)t} \end{bmatrix}
\mathbf{x}(0) \\
&=
\frac 1 2
\begin{bmatrix}
    \textcolor{orange}{e^{(1+2i)t}+e^{(1-2i)t}} &  
    \textcolor{violet}{-ie^{(1+2i)t} + ie^{(1-2i)t}} \\ 
    \textcolor{olive}{ie^{(1+2i)t}-ie^{(1-2i)t}} &  
    \textcolor{lightblue}{e^{(1+2i)t}+e^{(1-2i)t}} 
\end{bmatrix}
\mathbf{x}(0) \\
&=
\frac {e^t} 2
\begin{bmatrix}
    \textcolor{orange}{e^{2it}+e^{-2it}} &  
    \textcolor{violet}{-ie^{2it} + ie^{-2it}} \\ 
    \textcolor{olive}{ie^{2it}-ie^{-2it}} &  
    \textcolor{lightblue}{e^{2it}+e^{-2it}} 
\end{bmatrix}
\mathbf{x}(0) \\
&= e^t
\begin{bmatrix}
    \textcolor{orange}{\cos(2t)} &  
    \textcolor{violet}{\sin(2t)} \\ 
    \textcolor{olive}{-\sin(2t)} &  
    \textcolor{lightblue}{\cos(2t)} 
\end{bmatrix}
\mathbf{x}(0) 
\end{align}
$$
The matrix is the same as above, but we multiply by $e^t$, so as time goes by, the whole thing will blow up to infinity, creating a spiral going out.
![[Images/spiral_out.png|400]]

### Spiraling in/Spiral sink
Simply flipping the sign of the diagonal.
$$
\mathbf{A}=\begin{bmatrix}-1 & 2 \\ -2 & -1 \end{bmatrix}, \hspace{1em}
\mathbf{T}=\begin{bmatrix}1 & 1 \\ i & -i \end{bmatrix}, \hspace{1em} 
\mathbf{D}=\begin{bmatrix}-1+2i & 0 \\ 0 & -1-2i \end{bmatrix}, \hspace{1em}
\mathbf{T}^{-1}=\frac 1 2 \begin{bmatrix}1 & -i \\ 1 & i \end{bmatrix}
$$
$$
\begin{align}
\mathbf{x}(t)  &=\mathbf{T}e^{\mathbf{D}t}\mathbf{T}^{-1}\mathbf{x}(0) \\
&= e^{-t}
\begin{bmatrix}
    \textcolor{orange}{\cos(2t)} &  
    \textcolor{violet}{\sin(2t)} \\ 
    \textcolor{olive}{-\sin(2t)} &  
    \textcolor{lightblue}{\cos(2t)} 
\end{bmatrix}
\mathbf{x}(0) 
\end{align}
$$
Now the exponential decays to zero, so the whole system spirals in.

![[Images/spiral_in.png|400]]

> [!tip]- How to cook-up examples
> Take a generic matrix and its determinant
> $$
> \mathbf{A}=\begin{bmatrix}x_{11} & x_{12} \\ x_{21} & x_{22} \end{bmatrix}
> $$
> $$
> \begin{align}
> det(\mathbf{A}-\lambda\mathbf{I})&=det\left(\begin{bmatrix}x_{11}-\lambda & x_{12} \\ x_{21} & x_{22}-\lambda \end{bmatrix}\right) \\
> &=(x_{11}-\lambda)(x_{22}-\lambda) - x_{12}x_{21} \\
> &=\lambda^2-(x_{11}+x_{22})\lambda-x_{12}x_{21} + x_{11}x_{22}
> \end{align}
> $$
> The roots of a quadratic formula are
> 
> $$
> \begin{align}
> \frac{-b\pm\sqrt{b^2-4ac}}{2a}&=\frac{-(-(x_{11}+x_{22}))\pm\sqrt{(-(x_{11}+x_{22}))^2-4(-x_{12}x_{21}+ x_{11}x_{22})}}{2} \\
> &=\frac{x_{11}+x_{22}\pm\sqrt{(x_{11}+x_{22})^2+4( x_{12}x_{21}- x_{11}x_{22})}}{2}
> \end{align}
> $$
> - In order to have a real part, the diagonal must sum to something other than zero
> - To have an imaginary part the square root must be negative
> - The denominator is 2, so might as well try to have a numerator that divides by 2.
> - The eigenvalues will come in conjugate pairs, always the same real part.

