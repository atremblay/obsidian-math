When the [[Definitions/Eigenspace|dimension of the eigenspace]] is not equal to the multiplicity of its corresponding eigenvalue $\lambda$, then it's not possible to [[Diagonalization|diagonalize]] a square matrix $\mathbf{A}$.

The best thing we can do is to build a matrix $\mathbf{P}$ with [[Generalized eigenvectors]] to have something that is approximately diagonal
$$\mathbf{P^{-1}AP}=\mathbf{J}\iff\mathbf{AP}=\mathbf{PJ}$$
where $\mathbf{J}$ is a jordan matrix.


> [!Question]-  Solved
>If $\mathbf{P}$ is made of generalized eigen vectors, is it still an orthogonal basis? $\mathbf{PP^{-1}}=\mathbf{I}$? Can we make sure that everything is orthogonal?
>
>Wrong, an orthogonal basis means that $\mathbf{PP^T}=\mathbf{I}$ and not $\mathbf{PP^{-1}}=\mathbf{I}$. $\mathbf{PP^{-1}}=\mathbf{I}$ means that the vectors are independant.


# The ultimate Jordan form example

$n \times n$ matrix:
- Distinct, real $\lambda_1, \lambda_2,\ldots,\lambda_m$
- Complex pair $\lambda \pm i\omega$
- Repeated pair $\mu \times 3$ but $rank(\mathbf{A}-\lambda\mathbf{I})$ has 3D null space
- Repeated pair $\gamma \times 3$ but $rank(\mathbf{A}-\lambda\mathbf{I})$ has 2D null space
- Repeated pair $\delta \times 3$ but $rank(\mathbf{A}-\lambda\mathbf{I})$ has 1D null space

$$
\mathbf{J} = 
\left(\begin{array}{cccc}
\left[\begin{array}{cccc}
\lambda_1 & 0 & 0 & 0\\
0 & \lambda_2 & 0 & 0\\
0 & 0 & \ddots & 0 \\
0 & 0 & 0 & \lambda_n \\
\end{array}\right] &  &  & & 0\\

& \left[\begin{array}{cc}
\lambda & \omega\\
-\omega & \lambda\\
\end{array}\right] &  &  & &   \\

& & \left[\begin{array}{cc}
\mu & 0 & 0\\
0 & \mu & 0\\
0 & 0 & \mu\\
\end{array}\right] &  &  & \\

 & & &  \left[\begin{array}{cc}
\gamma & 1 & 0\\
0 & \gamma & 0\\
0 & 0 &\gamma \\
\end{array}\right] & \\

0 & &  & &  \left[\begin{array}{cc}
\delta & 1 & 0\\
0 & \delta & 1\\
0 & 0 &\delta \\
\end{array}\right] 
\end{array}\right)
$$

[System of DE: Diagonalization and Jordan Canonical Form](https://youtube.com/watch?v=SsCiQym5yQU&t=29m37s)
