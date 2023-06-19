

Quaternions and the cross product are both ways of expressing relationships between vectors, but they come from different mathematical systems.

A quaternion is a type of mathematical object similar to a complex number but extended to four dimensions. Quaternions were introduced by Sir William Rowan Hamilton in 1843 as a new mathematical system and they have a form like $q = a + bi + cj + dk$, where $a$, $b$, $c$, and $d$ are real numbers, and $i$, $j$, and $k$ are the fundamental quaternion units with the property that $i^2 = j^2 = k^2 = ijk = -1$. 

Hamilton found a way to express the multiplication of two quaternions in terms of dot and cross products. If you have two quaternions that have no real part, i.e., are purely imaginary (of the form $q_1 = b_1i + c_1j + d_1k$ and $q_2 = b_2i + c_2j + d_2k$), you can think of them as vectors in three-dimensional space. Then the product of two purely imaginary quaternions is:

$q_1 q_2 = - \mathbf{v}_1 \cdot \mathbf{v}_2 + \mathbf{v}_1 \times \mathbf{v}_2$,

where $\mathbf{v}_1$ and $\mathbf{v}_2$ are the vectors corresponding to $q_1$ and $q_2$, $\cdot$ denotes the dot product, and $\times$ denotes the cross product.

This means that the real part of the quaternion product corresponds to the negative of the dot product of the vectors, and the imaginary part corresponds to the cross product of the vectors.

Later, Gibbs and Heaviside found it useful to separate these two parts into distinct operations, which led to the definition of the dot product and cross product as we know them today.

It's important to note that the cross product is a specific case of a more general concept called the exterior product, which can be used in any number of dimensions. The exterior product of two vectors is a bivector, a geometric object that represents the plane spanned by the two vectors and whose magnitude is the area of the parallelogram formed by the two vectors. In three dimensions, every bivector corresponds uniquely to a vector orthogonal to the plane of the bivector, which gives the cross product. However, in other dimensions, there are bivectors that do not correspond to any vector, which makes the cross product specific to three dimensions.


Sure, let's work out the product of two purely imaginary quaternions. A purely imaginary quaternion is similar to a vector in three dimensions, and it has the form $q = bi + cj + dk$, where $b$, $c$, and $d$ are real numbers. 

Let's take two purely imaginary quaternions, $q_1 = b_1i + c_1j + d_1k$ and $q_2 = b_2i + c_2j + d_2k$. 

Their product is given by:

$$
\begin{align*}
q_1 q_2 &= (b_1i + c_1j + d_1k)(b_2i + c_2j + d_2k) \\
&= b_1 b_2 i^2 + b_1 c_2 ij + b_1 d_2 ik + c_1 b_2 ji + c_1 c_2 j^2 + c_1 d_2 jk \\
&+ d_1 b_2 ki + d_1 c_2 kj + d_1 d_2 k^2
\end{align*}
$$

The multiplication of quaternion units follows these rules: $i^2 = j^2 = k^2 = -1$ and $ij = k$, $jk = i$, and $ki = j$, and also $ji = -k$, $kj = -i$, and $ik = -j$.

Using these rules, the above expression simplifies to:

$$
\begin{align*}
q_1 q_2 &= -b_1 b_2 + b_1 c_2 k - b_1 d_2 j - c_1 b_2 k - c_1 c_2 + c_1 d_2 i \\
&+ d_1 b_2 j - d_1 c_2 i - d_1 d_2 \\
&= -(b_1 b_2 + c_1 c_2 + d_1 d_2) + (c_1 d_2 - d_1 c_2)i + (d_1 b_2 - b_1 d_2)j + (b_1 c_2 - c_1 b_2)k
\end{align*}
$$

This is equivalent to:

$$
\begin{align*}
q_1 q_2 &= -\mathbf{v}_1 \cdot \mathbf{v}_2 + \mathbf{v}_1 \times \mathbf{v}_2
\end{align*}
$$

where $\mathbf{v}_1$ and $\mathbf{v}_2$ are the vectors corresponding to $q_1$ and $q_2$, $\cdot$ denotes the dot product, and $\times$ denotes the cross product.

This means that the real part of the quaternion product corresponds to the negative of the dot product of the vectors, and the imaginary part corresponds to the cross product of the vectors.