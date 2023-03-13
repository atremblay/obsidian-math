
> [!Summary] 
> $$
> \begin{align}
> \mathbf{x}(t)&=e^{\mathbf{A}t}\mathbf{x}(0)\\
> &=\mathbf{T}e^{\mathbf{D}t}\mathbf{T}^{-1}\mathbf{x}(0)
> \end{align}
> $$

For a general system $\dot{\mathbf{x}}=\mathbf{A}\mathbf{x}$ we want a change of coordinates $\mathbf{x}=\mathbf{T}\mathbf{z}$, with $\mathbf{T}$ invertible, that diagonalizes/decouples my ODE so that we can easily solve it (as seen [[#Solution|here]]) in this new coordinate system

$$
\dot{\mathbf{z}}=\mathbf{D}\mathbf{z}
\implies \mathbf{z}(t)=e^{\mathbf{D}t}\mathbf{z}(0)
$$
$\mathbf{D}$ is a diagonal matrix. Taking the derivative of the new coordinate system
$$
\begin{align}
\mathbf{T}\dot{\mathbf{z}}&=\dot{\mathbf{x}}=\mathbf{A}\mathbf{x}=\mathbf{A}\mathbf{T}\mathbf{z} \\
\implies  \dot{\mathbf{z}} &= \underbrace{\mathbf{T}^{-1}\mathbf{A}\mathbf{T}}_{\mathbf{D}}\mathbf{z} \\
&= \mathbf{D}\mathbf{z}
\end{align}
$$
This is now an ODE expressed only in terms of $\mathbf{z}$. In that system, the ODEs are decoupled. For this we need a $\mathbf{T}$ such that $\mathbf{T}^{-1}\mathbf{A}\mathbf{T}=\mathbf{D}$ . From this we see
$$
\boxed{\mathbf{A}\mathbf{T}=\mathbf{T}\mathbf{D}}
$$
which is the **eigenvalue equation**
- $\mathbf{T}$ is a matrix of eigenvectors
- $\mathbf{D}$ is a matrix with the eigenvalues on the diagonal
- We can solve the system in $\mathbf{z}=\mathbf{T}^{-1}\mathbf{x}$

$$
\begin{align}
\mathbf{z}(t)=\mathbf{T}^{-1}\mathbf{x}(t)&=e^{\mathbf{D}}\mathbf{z}(0)=e^{\mathbf{D}}\mathbf{T}^{-1}\mathbf{x}(0)\\
\mathbf{x}(t)&=\mathbf{T}e^{\mathbf{D}t}\mathbf{T}^{-1}\mathbf{x}(0)
\end{align}
$$
> [!Question] 
> If $\mathbf{TT^{-1}}=\mathbf{I}$, then the eigen vectors are orthogonal. What is the property that guarantee this?


> [!Question] 
> What order should the eigenvalues be? I don't see anything that links an eigenvalue to a specific position in $\mathbf{x}$


