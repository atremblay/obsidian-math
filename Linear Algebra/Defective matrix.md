---
aliases:
    - "defective matrix"
    - "degenerate matrix"
---


> [!Summary] 
> - If the matrix $\mathbf{A}$ of shape $n \times n$ has the eigenvalue $\lambda$ repeated $n$ times and $rank(\mathbf{A}-\lambda\mathbf{I}) = 0$, then it will have $n$ eigenvectors.
> - If not, then it's defective and will require [[Generalized eigenvectors]]
> - In other words, if the multiplicity of the eigenvalue $\lambda$ != $rank(nullspace(\mathbf{A}-\lambda\mathbf{I}))$ then the matrix $\mathbf{A}-\lambda\mathbf{I}$ is defective


> [!Example]
> > [!Example]- Case 1 - Dimension of nullspace of $(\mathbf{A}-\lambda \mathbf{I})=3$
> > $$\begin{bmatrix} \lambda & 0 & 0 \\ 0 & \lambda & 0 \\ 0 & 0 & \lambda \end{bmatrix} $$
> > $rank(\mathbf{A}-\lambda \mathbf{I})=0$. There will be 3 clean eigenvectors. No need for [[Generalized eigenvectors]].
> 
> 
> > [!Example]- Case 2 - Dimension of nullspace of $(\mathbf{A}-\lambda \mathbf{I})=2$
> > $$\begin{bmatrix} \lambda & 1 & 0 \\ 0 & \lambda & 0 \\ 0 & 0 & \lambda \end{bmatrix} $$
> > $rank(\mathbf{A}-\lambda \mathbf{I})=1$. There will be 2 clean eigenvectors, will need 1 [[Generalized eigenvectors|generalized eigenvector]].
> 
> 
> > [!Example]- Case 3 - Dimension of nullspace of $(\mathbf{A}-\lambda \mathbf{I})=1$
> > $$\begin{bmatrix} \lambda & 1 & 0 \\ 0 & \lambda & 1 \\ 0 & 0 & \lambda \end{bmatrix} $$
> > $rank(\mathbf{A}-\lambda \mathbf{I})=2$. There will be 1 clean eigenvector, will need 2 [[Generalized eigenvectors]].


A defective matrix is a matrix that has less independant eigenvectors than its rank. It fails to provided a basis for $\mathbb{R}^n$ made of eigenvectors. This example is in $\mathbb{R}^3$
$$
\mathbf{A}=
\begin{bmatrix}
6 & -2 & -1 \\
3 & 1 & -1 \\
2 & -1 & 2 \\
\end{bmatrix}
$$

The [[../Calculus/Characteristic equation]] of this matrix is $(3-\lambda)^3$, so just one eigenvalue with multiplicity 3. 
$$
\det\left(\begin{bmatrix}
6 & -2 & -1 \\
3 & 1 & -1 \\
2 & -1 & 2 \\
\end{bmatrix} - 
\lambda
\begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1 \\
\end{bmatrix}
\right)=-\lambda^3+9\lambda^2-27\lambda+27
$$
An non-defective matrix would have 3 independant eigenvectors.

$$
\mathbf{A} - 3\mathbf{I} =
\begin{bmatrix}
3 & -2 & -1 \\
3 & -2 & -1 \\
2 & -1 & -1 \\
\end{bmatrix}
$$

$$
\begin{align}
\begin{bmatrix}
1 & 0 & -1 \\ 0 & 1 & -1 \\ 0 & 0 & 0 
\end{bmatrix}
\begin{bmatrix}
x_1 \\ x_2 \\ x_3 
\end{bmatrix} &=
\begin{bmatrix}
x_1 -x_3 \\ x_2 - x_3 \\ 0 
\end{bmatrix}=
\begin{bmatrix}
0 \\ 0 \\ 0 
\end{bmatrix} \\
\implies
\begin{bmatrix}
x_1 \\ x_2 \\ x_3 
\end{bmatrix} &=
x_3\begin{bmatrix}
1 \\ 1 \\ 1 
\end{bmatrix}
\end{align}
$$
$x_3$ is a free parameter, let's set it to 1 and we have $x_1=x_2=x_3=1$, so the only eigen vector is
$$\mathbf{x}=\begin{bmatrix}1\\1\\1\end{bmatrix}$$

> Unless $\mathbf{A}$ is a diagonal matrix with the same value on the diagonal, if the algebraic multiplicity of $\lambda$ is equal to the dimension of $\mathbf{A}$ then it will be defective. It's because you need at least as many free parameters as the multiplicity of $\lambda$ to **not** be defective.

