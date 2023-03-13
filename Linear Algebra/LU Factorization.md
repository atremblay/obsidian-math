At first, assume that $\mathbf{A}$ is an $m \times n$ matrix that can be row reduced to echelon form, without row interchanges. (Later, we will treat the general case.) Then $\mathbf{A}$ can be written in the form $\mathbf{A}=\mathbf{LU}$, where $\mathbf{L}$ is an $m \times m$ lower triangular matrix with 1’s on the diagonal and $\mathbf{U}$ is an $m \times n$ echelon form of $\mathbf{A}$

$$
\mathbf{A} = 
\underbrace{
\begin{bmatrix}
1 & 0 & 0 & 0 \\
* & 1 & 0 & 0 \\
* & * & 1 & 0 \\
* & * & * & 1 \\
\end{bmatrix}}_{\mathbf{L}}
\underbrace{
\begin{bmatrix}
\blacksquare & * & * & * & * \\
0 & \blacksquare & * & * & * \\
0 & 0 & 0 & \blacksquare & * \\
0 & 0 & 0 & 0 & 0 \\
\end{bmatrix}}_{\mathbf{U}}
$$


> [!todo] Complete
> This was just copied over from Linear algebra and its applications, page 126


