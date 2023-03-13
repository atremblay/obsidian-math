> [!summary]
> $$\boxed{\mathbf{A}=\mathbf{PDP^{-1}}}$$
> or
> $$\boxed{\mathbf{AP}=\mathbf{PD}}$$
> $\mathbf{A}$ is diagonalizable if and only if there are enough eigenvectors to form a basis of $\mathbb{R}^n$.
> 
> $\mathbf{P}$: Columns are eigenvectors of $\mathbf{A}$
> $\mathbf{D}$: Diagonal are the eigenvalues of $\mathbf{A}$

An $n \times n$ matrix $\mathbf{A}$ is diagonalizable if and only if $\mathbf{A}$ has $n$ linearly independent eigenvectors.

In fact, $\mathbf{A}=\mathbf{PDP^{-1}}$, with $\mathbf{D}$ a diagonal matrix, if and only if the columns of $\mathbf{P}$ are $n$ linearly independent eigenvectors of $\mathbf{A}$. In this case, the diagonal entries of $\mathbf{D}$ are eigenvalues of $\mathbf{A}$ that correspond, respectively, to the eigenvectors in $\mathbf{P}$.


> [!todo] 
> The proof is interesting. Write it down here. Page 284 of linear algebra and its applications


Let $\mathbf{A}$ be an $n \times n$ matrix whose distinct eigenvalues are $\lambda_1,\ldots,\lambda_p$.

1. For $1 \leq k \leq p$, the [[Eigenspace#^414ca9|dimension of the eigenspace]] for $\lambda_k$ is less than or equal to the multiplicity of the eigenvalue $\lambda_k$ .
2. The matrix $\mathbf{A}$ is diagonalizable if and only if the sum of the [[Eigenspace#^414ca9|dimensions of the eigenspaces]] equals n, and this happens if and only if (i) the characteristic polynomial factors completely into linear factors and (ii) the [[Eigenspace#^414ca9|dimension of the eigenspace]] for each $\lambda_k$ equals the multiplicity of $\lambda_k$.
4. If $\mathbf{A}$ is diagonalizable and $\mathcal{B}_k$ is a basis for the eigenspace corresponding to $\lambda_k$ for each $k$, then the total collection of vectors in the sets $\mathcal{B}_1,\ldots,\mathcal{B}_p$ forms an eigenvector basis for $\mathbb{R}^n$.