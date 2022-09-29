# Limit of a function ($\epsilon-\delta$ definition)

Let $f(x)$ be a function defined on an open interval around $x_0$ ($f(x_0)$ **need not be defined**). We say that the limit of $f(x)$ as $x$ approaches $x_0$ is L, i.e.

$$\lim_{x\rightarrow x_0} f(x)=L$$

if $\forall \epsilon >0,\exists \delta>0$ such that $\forall x$

$$0<|x-x_0|<\delta \implies |f(x)−L|<\epsilon$$

[](https://ds055uzetaobb.cloudfront.net/brioche/uploads/FQobsWJeq0-group.png?width=3000)

The limit is not evaluating $f(L)$, that's why $f(L)$ need not be defined. e.g.
$$f(x)=\frac{1}{1+e^{\frac{1}{x}}}$$
Even if $f(0)$ is undefined we can still say
$$\lim_{x\rightarrow0}f(x)=0$$

## Left and Right
Depending on what side you approach $x_0$ that may change the limit
### Left
$$
\lim_{x\rightarrow x_0^-} f(x)=L,\hspace{2em}0<\textcolor{orange}{x_0-x}<\delta \implies \textcolor{cyan}{L-f(x)}<\epsilon
$$
### Right
$$
\lim_{x\rightarrow x_0^+} f(x)=L,\hspace{2em}0<\textcolor{orange}{x-x_0}<\delta \implies \textcolor{cyan}{f(x)-L}<\epsilon
$$
e.g.
$$\lim_{x\rightarrow 0^-}\frac{1}{1+e^{\frac{1}{x}}}=1$$
$$\lim_{x\rightarrow 0^+}\frac{1}{1+e^{\frac{1}{x}}}=0$$
# Continuity of a function
Continuity uses the definition of a limit + using $f(x_0)$. The limit did not need $f(x_0)$ to be defined. Here it is required. 
$$f(x) \in C \textit{ at } x=x_0 \iff \lim_{x\rightarrow x_0}f(x) = f(x_0)
$$
Where $C$ is the class of continuous functions.
## Left and right
### Left
$$f(x) \in C \textit{ at } x=x_0 \iff \lim_{x\rightarrow x_0^-}f(x) = f(x_0)
$$
### Right
$$f(x) \in C \textit{ at } x=x_0 \iff \lim_{x\rightarrow x_0^+}f(x) = f(x_0)
$$

e.g.
$$f(x)=\frac{1}{1+e^{\frac{1}{x}}}$$
is not defined at $f(0)$, but if we assume that $f(0)=0$ then we can say that $f(x) \in C \text{ on the right at }x=0$ because $\lim_{x\rightarrow 0^+}=f(0)=0$.