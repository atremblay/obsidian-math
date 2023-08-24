
> [!definition]
> From [wikipedia](https://en.wikipedia.org/wiki/Superposition_principle)
> The **superposition principle**, also known as **superposition property**, states that, for all [linear systems](https://en.wikipedia.org/wiki/Linear_system "Linear system"), the net response caused by two or more stimuli is the sum of the responses that would have been caused by each stimulus individually. So that if input $A$ produces response $X$ and input $B$ produces response $Y$ then input $(A+B)$ produces response $(X+Y)$.

The wikipedia definition is generic. Here is what it means for ODE. This is going to be hectic in terms of notation.

With $n$ (non) homogeneous equations
$$
f_n(x)y^{(n)} + \ldots + f_1(x)y^{(1)} + f_0(x)y = Q_i(x) 
$$

where $i = 1 \ldots n$, each will have one and only solution due to the [[Existence and uniqueness]]. The particular solutions are $y_{p_1}$, $y_{p_2}$, ..., $y_{p_n}$
$$
\begin{align}
f_n(x)y_{p_1}^{(n)} + \ldots + f_1(x)y_{p_1}^{(1)} + f_0(x)y_{p_1} &= Q_1(x) \\
f_n(x)y_{p_2}^{(n)} + \ldots + f_1(x)y_{p_2}^{(1)} + f_0(x)y_{p_2} &= Q_2(x) \\
&\ldots \\
f_n(x)y_{p_n}^{(n)} + \ldots + f_1(x)y_{p_n}^{(1)} + f_0(x)y_{p_n} &= Q_n(x) \\
\end{align}
$$

Let's multiply each of these by their own **arbitrary** constants $c_{1}, \ldots, c_{n}$
$$
\begin{align}
c_{1}\left( 
\textcolor{orange}{f_n(x)}y_{p_1}^{(n)} + \ldots + \textcolor{violet}{f_1(x)}y_{p_1}^{(1)} + \textcolor{olive}{f_0(x)}y_{p_1} \\
\right) &= c_{1}Q_1(x)\\ 
c_{2}\left( \\
\textcolor{orange}{f_n(x)}y_{p_2}^{(n)} + \ldots + \textcolor{violet}{f_1(x)}y_{p_2}^{(1)} + \textcolor{olive}{f_0(x)}y_{p_2} \\
\right) &= c_{2}Q_2(x) \\
&\ldots \\
c_{n}\left( \\
\textcolor{orange}{f_n(x)}y_{p_n}^{(n)} + \ldots + \textcolor{violet}{f_1(x)}y_{p_n}^{(1)} + \textcolor{olive}{f_0(x)}y_{p_n} \\
\right) &= c_{n}Q_n(x) \\
\end{align}
$$

and sum them up (and rearange a little bit around the $f_{i}(x)$). This is the superposition
$$
\begin{align}
&\textcolor{orange}{f_n(x)}\left[c_{1}y_{p_1}^{(n)}  + c_{2}y_{p_2}^{(n)} + \ldots + c_{n}y_{p_n}^{(n)} \right] +  \\ &\ldots \\
&\textcolor{violet}{f_1(x)}\left[c_{1}y_{p_1}^{(1)}  + c_{2}y_{p_2}^{(1)} + \ldots + c_{n}y_{p_n}^{(1)} \right] +  \\ \\
&\textcolor{olive}{f_0(x)}\left[c_{1}y_{p_1}  + c_{2}y_{p_2} + \ldots + c_{n}y_{p_n} \right]  \\ 
&= c_{1}Q_1(x) + c_{2}Q_2(x) + \ldots+ c_{2}Q_n(x)
\end{align}
$$

^72d290

For a little bit more clarity, we can put the $n^{th}$ order derivative outside each square bracket

$$
\begin{align}
&\textcolor{orange}{f_n(x)}\left[c_{1}y_{p_1}  + c_{2}y_{p_2} + \ldots + c_{n}y_{p_n} \right]^{(n)} +  \\ &\ldots \\
&\textcolor{violet}{f_1(x)}\left[c_{1}y_{p_1}  + c_{2}y_{p_2} + \ldots + c_{n}y_{p_n} \right]^{(1)} +  \\ \\
&\textcolor{olive}{f_0(x)}\left[c_{1}y_{p_1}  + c_{2}y_{p_2} + \ldots + c_{n}y_{p_n} \right] \\ 
&= c_{1}Q_1(x) + c_{2}Q_2(x) + \ldots+ c_{2}Q_n(x)
\end{align}
$$

We see that the combination of the inputs result in a combination of the output. Each square brackets are the same, so let's define a new particular solution as
$$y_p = c_{1}y_{p_1} + c_{2}y_{p_2} +  \ldots + c_{n}y_{p_n}$$ 

and 
$$Q(x)= c_{1}Q_1(x) + c_{2}Q_2(x) + \ldots + c_{n}Q_n(x)   $$

and replace
$$
\begin{align}
&\textcolor{orange}{f_n(x)}y_p^{(n)} + 
\ldots +
\textcolor{violet}{f_1(x)}y_p^{(1)} +  
\textcolor{olive}{f_0(x)}y_p =Q(x)
\end{align}
$$

This shows that $y_{p}=\sum_{i=1}^n c_{i}y_{p_{i}}$ is a solution of  $Q(x) = \sum_{i=1}^n c_{i}Q_i(x)$

Each $y_{p_{i}}$ are all [[../Calculus/Lin. Ind. of Func|linearly independant]]


> [!question]-
> I'm still unclear why they are all linearly independant. Apparently, for the case of constant coefficient (i.e. $f_{i}(x) = \text{constant}$) these are proofs of that [[Solutions/Const Coeff Homo DE]],  [[Solutions/Undetermined Coefficient|Undetermined coefficient for non-homogeneous ODE]] and [[Solutions/Variation of Parameters]]

> [!Example]-
> If $\textcolor{lime}{y_{p_1}}=1+x$ is a solution to 
> $$
> \textcolor{orange}{y''-y'+y}=x
> $$
> 
> and $\textcolor{violet}{y_{p_2}}=e^{2x}$ is a solution to 
> $$
> \textcolor{orange}{y''-y'+y}=2e^{2x}
> $$
> 
> then $y_p = \textcolor{lime}{y_{p_1}} + \textcolor{violet}{y_{p_2}}$ is a solution to
> $$
> \textcolor{orange}{y''-y'+y}=x+2e^{2x}
> $$
> 
> > Notice how $\textcolor{orange}{y''-y'+y}$ is always the same. Only the right-hand side changes and that gives different solutions.  

# Specific cases
## Homogenous

If $Q_{i}(x)=0$ for all $i$ 

$$
\begin{align}
f_n(x)y_{p_1}^{(n)} + \ldots + f_1(x)y_{p_1}^{(1)} + f_0(x)y_{p_1} &= 0 \\
f_n(x)y_{p_2}^{(n)} + \ldots + f_1(x)y_{p_2}^{(1)} + f_0(x)y_{p_2} &= 0 \\
&\ldots \\
f_n(x)y_{p_n}^{(n)} + \ldots + f_1(x)y_{p_n}^{(1)} + f_0(x)y_{p_n} &= 0 \\
\end{align}
$$

Then the solution $y_c=\sum_{i=1}^n c_{i}y_{p_{i}}$
$$
\begin{align}
f_n(x)y_c^{(n)} + \ldots +f_1(x)y_c^{(1)} +  f_0(x)y_c =0
\end{align}
$$

Notice the subscript $y_c$. $c$ stands for [[Definitions/Complementary function|complementary]] because this is a solution to the homogeneous version where $Q_{i}(x)=0$ for all $i$ .


## $c_i=a$
If all constants $c_i$ are the same, then they just cancel out on both sides of the equality. [[#^72d290|This]] becomes

$$
\begin{align}
f_n(x)\left[ay_{p_1}  + ay_{p_2} + \ldots + ay_{p_n} \right]^{(n)} \\ + \ldots \\
+ f_1(x)\left[ay_{p_1}  + ay_{p_2} + \ldots + ay_{p_n} \right]^{(1)}  \\ 
+ f_0(x)\left[ay_{p_1}  + ay_{p_2} + \ldots + ay_{p_n} \right] &= aQ_1(x) + aQ_2(x) + \ldots+ aQ_n(x)  \\
\iff \cancel{a} \sum_{i=0}^n f_i(x)\left[\sum_{j=1}^n y_{p_j}  \right]^{(i)} &= \cancel{a} \sum Q_i(x)
\end{align}
$$

The [[Definitions/Complementary function|complementary solution]] is now
$$y_c=\sum_{j=1}^n y_{p_{j}}$$