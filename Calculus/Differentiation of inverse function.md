
> [!summary]
> $$
> \frac{dx}{dy} = \frac{1}{\frac{dy}{dx}} \iff \frac{dy}{dx} = \frac{1}{\frac{dx}{dy}}
>  $$
> As long as the denominator is not zero. This is most useful when we are dealing with [[Trigonometric functions|trigonometric functions]]. 


By the definition of the derivative
$$
\frac{dx}{dy} = \lim_{\Delta y \rightarrow 0}\frac{\Delta x}{\Delta y} 
$$


Because $\frac{\Delta x}{\Delta y}$ is a fraction, we can say  
$$
\frac{\Delta x}{\Delta y} = \frac{1}{\frac{\Delta y}{\Delta x}}
$$
Assuming that the derivative exists, when $\Delta x \rightarrow 0$, so does $\Delta y \rightarrow 0$ and vice versa. That is why we can change the limit
$$
\lim_{\Delta y \rightarrow 0}\frac{\Delta x}{\Delta y} = \lim_{\Delta x \rightarrow 0}\frac{1}{\frac{\Delta y}{\Delta x}}
$$


The limit ([[Properties#Quotient rule|quotient rule]]) only then apply to the denominator (numerator stays the same since it does not involve $\Delta x$).

$$
\lim_{\Delta x \rightarrow 0}\frac{1}{\frac{\Delta y}{\Delta x}} = \frac{1}{\lim_{\Delta x \rightarrow 0}\frac{\Delta y}{\Delta x}}= \frac{1}{\frac{dy}{dx}}
$$

Important, the denominator of $\frac{1}{\frac{dy}{dx}}$ is **not** a fraction, it's the derivative of $y$ with respect to $x$.


> [!example]-
> $$
> \begin{align}
> y'&=y\\
> \frac{dy}{dx} &= y\\
> \frac{dx}{dy} = \frac{1}{\frac{dy}{dx}} &= \frac{1}{y} \\
> \end{align}
> $$
> $$
> \begin{align}
> &\int{\frac{dx}{dy}dy} &= &\int{\frac{1}{y}}dy \\
> \iff &x &= &\ \log(y) + C_1 \\
> \iff &x-C_1 &=&\ \log(y) \\
> \iff &\exp(x-C_1) &= &\ y \\
> \iff &\frac{\exp(x)}{\exp(C_1)} &= &\ y \\
> \iff &C e^x &= &\ y
> \end{align}
> $$
> 
> 
> Had we not used the inverse property, it would have been annoying to deal with the left-hand side of
> $$
> \int \frac{dy}{dx} dy = \int y dy
> $$
