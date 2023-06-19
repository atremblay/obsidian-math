
> [!Summary]
> The curl measures the rotation of a vector field $\vec{f}$ by doing a [[../Cross Product|cross product]] between the  [[Gradient Operator|grad operator]] and the vector field $\vec{f}$. It's an operator that takes in a vector and output a vector
> $$
> \vec{\nabla} \times \vec{f} = 
> \det \begin{bmatrix} 
>     \vec{i} & \vec{j} & \vec{k} \\
>     \frac{\partial}{\partial x} & \frac{\partial}{\partial y} & \frac{\partial}{\partial z} \\
>     f_1(x,y,z) & f_2(x,y,z) & f_3(x,y,z)
> \end{bmatrix}
> $$
> Unlike the [gradient](https://en.wikipedia.org/wiki/Gradient "Gradient") and [divergence](https://en.wikipedia.org/wiki/Divergence "Divergence"), curl as formulated in vector calculus does not generalize simply to other dimensions; some [generalizations](https://en.wikipedia.org/wiki/Curl_(mathematics)#Generalizations) are possible, but only in three dimensions is the geometrically defined curl of a vector field again a vector field.





The curl of a vector field is a vector operation that describes the infinitesimal rotation of the field in a three-dimensional space. It's important in electromagnetism, fluid dynamics, and the theory of deformation.

Consider a vector field 
$$
\vec{f} = \begin{bmatrix}
f_1(x,y,z) \\ f_2(x,y,z) \\ f_3(x,y,z)
\end{bmatrix}
$$
that represents, for instance, the flow velocity of a fluid. The curl of $\vec{f}$, denoted as $\text{curl} \vec{f}$ or $\nabla \vec{f}$, is given by:

$$
\nabla \times \vec{f} = \left(\frac{\partial f_3}{\partial y}-\frac{\partial f_2}{\partial z}\right)\vec{i} + 
\left(\frac{\partial f_1}{\partial z}-\frac{\partial f_3}{\partial x}\right)\vec{j} + 
\left(\frac{\partial f_2}{\partial x}-\frac{\partial f_1}{\partial y}\right)\vec{k}
$$

Like the coefficients in the [[../Cross Product#What are the coefficients of $ vec{i}$, $ vec{j}$ and $ vec{k}$|cross product]], the coefficients of $\vec{i}$, $\vec{j}$ and $\vec{k}$ in the curl also have specific interpretations:

- The $\vec{i}$ coefficient $(\partial f_3 / \partial y - \partial f_2 / \partial z)$ represents the tendency of the field to rotate around the x-axis. It measures the difference between the rate of change of the z-component along the y-axis and the rate of change of the y-component along the z-axis.
- The $\vec{j}$ coefficient $(\partial f_1 / \partial z - \partial f_3 / \partial x)$ represents the tendency of the field to rotate around the y-axis. It measures the difference between the rate of change of the x-component along the z-axis and the rate of change of the z-component along the x-axis.
- The $\vec{k}$ coefficient $(\partial f_2 / \partial x - \partial f_1 / \partial y)$ represents the tendency of the field to rotate around the z-axis. It measures the difference between the rate of change of the y-component along the x-axis and the rate of change of the x-component along the y-axis.

In the context of fluid flow, each coefficient measures the tendency of the fluid to rotate around the respective axis. So, the curl vector gives us the axis of rotation, and its magnitude gives us the amount of rotation.

It's worth noting that the curl is not defined in two dimensions, and its generalization to other dimensions is not straightforward. The curl of a vector field can be thought of as a measure of the "rotation" or "circulation" of the field.

![](https://youtu.be/A2Sn_xAKMwQ)