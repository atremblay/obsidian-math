

> [!summary]
> Divergence is a measure of how, locally, the vector field $\vec{f}$ points toward the point $(x,y)$ (sink), away from $(x,y)$ (source) or nothing happens if the divergence is zero.
> 
> Dot product between the [[Gradient Operator|gradient operator]] and a vector valued function $\vec{f}$
> $$
> \text{div}(\vec{f}) = 
> \vec{\nabla} \cdot \vec{f} = 
> \begin{bmatrix}\frac{\partial}{\partial x} & \frac{\partial}{\partial y} \end{bmatrix} 
> \begin{bmatrix}f_1(x,y) \\ f_2(x,y) \end{bmatrix} = \frac{\partial f_1(x,y)}{\partial x} + \frac{\partial f_2(x,y)}{\partial y}
> $$
> 
  
  

> [!blank-container|right-medium]
> ![[Images/divergence.svg]]
> 

## Properties
### Linearity
The divergence operator is linear

**Addition**
$$
\vec{\nabla} \cdot (\vec{f}_A(x,y) + \vec{f}_B(x,y)) = \vec{\nabla} \vec{f}_A(x,y)  + \vec{\nabla} \vec{f}_B(x,y) 
$$
**Scalar multiplication**
$$
\vec{\nabla} \cdot \left(\alpha \vec{f}(x,y)\right) = \alpha \vec{\nabla} \cdot \vec{f}(x,y) 
$$

See also:
- [[Curl Operator]]