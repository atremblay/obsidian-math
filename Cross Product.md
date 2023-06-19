
> [!summary] 
> The cross product gives us a vector that is perpendicular to the plane defined by the original two vectors, with a magnitude equal to the area of the parallelogram formed by the original vectors, and with a direction given by the right-hand rule.
> $$
> \begin{align}
> A \times B &= 
> \det \begin{bmatrix} 
>     \vec{i} & \vec{j} & \vec{k} \\
>     a_1 & a_2 & a_3 \\
>     b_1 & b_2 & b_3
> \end{bmatrix}\\
> &= \begin{bmatrix}
> a_2b_3 - a_3b_2 \\ a_3b_1 - a_1b_3 \\ a_1b_2 - a_2b_1
> \end{bmatrix}
> \end{align}
> $$


> [!NOTE]
> The cross product was not so much "discovered" as it was "invented" or "defined" (see [[Quaternions]]) to meet certain needs in mathematics and physics. The cross product is a mathematical tool that was created to solve specific types of problems and to capture certain geometric and physical properties.


Given two vectors $A=\begin{bmatrix}a_1 & a_2 &a_3\end{bmatrix}$ and $B=\begin{bmatrix}b_1 & b_2 &b_3\end{bmatrix}$, then their cross product is given by:

$$
\begin{align}
A \times B &= 
\begin{vmatrix} 
    \vec{i} & \vec{j} & \vec{k} \\
    a_1 & a_2 & a_3 \\
    b_1 & b_2 & b_3
\end{vmatrix} \tag{1}\\
&= (a_2b_3 - a_3b_2)\vec{i} - (a_1b_3 - a_3b_1)\vec{j} + (a_1b_2 - a_2b_1)\vec{k} \tag{2} \\
&= \begin{bmatrix}
a_2b_3 - a_3b_2 \\ a_3b_1 - a_1b_3 \\ a_1b_2 - a_2b_1
\end{bmatrix} \tag{3}
\end{align}
$$

Where $\vec{i}$, $\vec{j}$ and $\vec{k}$ are the unit vectors in the x, y, and z directions respectively. The cross product only directly applies in three dimensions. For higher dimensions, a different operation called the exterior product is used.

If $C=A \times B$ then
$$
\begin{align}
C = \begin{bmatrix} c_1 \\ c_2 \\ c_3 \end{bmatrix} = \begin{bmatrix}
a_2b_3 - a_3b_2 \\ a_3b_1 - a_1b_3 \\ a_1b_2 - a_2b_1
\end{bmatrix} \tag{4}
\end{align}
$$
## What are the coefficients of $\vec{i}$, $\vec{j}$ and $\vec{k}$

Conceptual understanding of the coefficients:
- The $\vec{i}$ coefficient: $(a_2b_3 - a_3b_2)$. This measures the difference between the products of the y and z components of vectors $A$ and $B$. It can be interpreted as how much $A$ and $B$ fail to be orthogonal in the yz-plane.
- The $\vec{j}$ coefficient: $(a_3b_1 - a_1b_3)$. This measures the difference between the products of the z and y components of vectors $A$ and $B$. It can be interpreted as how much $A$ and $B$ fail to be orthogonal in the zx-plane.
- The $\vec{k}$ coefficient: $(a_1b_2 - a_2b_1)$. This measures the difference between the products of the x and y components of vectors $A$ and $B$. It can be interpreted as how much $A$ and $B$ fail to be orthogonal in the xy-plane.

The coefficients can be considered as a measure of 'non-orthogonality' in each of the 2D subspaces of our 3D space. In this sense, the cross product provides a way of quantifying the degree and direction of non-orthogonality of two vectors.

## Properties
### Orthogonality
Let's consider the vector $A = (a_1, a_2, a_3)$ and $C = A \times B = (c1, c2, c3)$ (4), then $A \cdot C$ is:

$$
\begin{align}
a_1c_1 + a_2c_2 + a_3c_3 
&= a_1(a_2b_3 - a_3b_2) + a_2(a_3b_1 - a_1b_3) + a_3(a_1b_2 - a_2b_1) \\
&= a_1a_2b_3 - a_1a_3b_2 + a_2a_3b_1 - a_1a_2b_3 + a_1a_3b_2 - a_2a_3b_1 \\
&= 0
\end{align}
$$

You can do a similar calculation to show that $B \cdot C = 0$. So we have shown that $C = A \times B$ is indeed orthogonal to both $A$ and $B$.

### Non-commutativity

The direction of the cross product is given by the right-hand rule. If you point your right hand's fingers in the direction of A and curl them towards B, your thumb will point in the direction of the cross product. This means the cross product is not commutative, i.e., $A \times B$ is not the same as $B \times A$; in fact, they are opposite ($A \times B = - B \times A$).

### Magnitude of C

The magnitude (or length) of the cross product is equal to the area of the parallelogram that the vectors A and B span. If A and B are unit vectors, the area of this parallelogram is the sine of the angle between A and B.


## Origin
See [[Quaternions]].  It was initially for weird complex numbers, but part of the multiplication of two quaternions “defines” the cross product. The $\times$ operator has then been reused in other places. 