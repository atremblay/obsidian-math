$\Delta x=\frac{b-a}{N}$

Right-sided rectangle
$$
\begin{align}
\int_a^b f(x)\mathrm{d}x &= \lim_{N \rightarrow \infty} \sum_{\textcolor{orange}{k=1}}^{\textcolor{orange}{N}}f(a+\Delta x)\Delta x \\
&= \lim_{N \rightarrow \infty} \sum_{\textcolor{orange}{k=1}}^{\textcolor{orange}{N}}f(x_k)\Delta x
\end{align}
$$

Left-sided rectange'
$$
\begin{align}
\int_a^b f(x)\mathrm{d}x &= \lim_{N \rightarrow \infty} \sum_{\textcolor{orange}{k=0}}^{\textcolor{orange}{N-1}}f(a+\Delta x)\Delta x \\
&= \lim_{N \rightarrow \infty} \sum_{\textcolor{orange}{k=0}}^{\textcolor{orange}{N-1}}f(x_k)\Delta x
\end{align}
$$

Trapezoidal (average)
$$
\begin{align}
\int_a^b f(x)\mathrm{d}x 
&= \lim_{N \rightarrow \infty} \sum_{k=0}^{N-1}\frac{f(x_k)+f(x_{k+1})}{2}\Delta x
\end{align}
$$



# Ressources
https://pythonnumericalmethods.berkeley.edu/notebooks/chapter21.03-Trapezoid-Rule.html
https://numpy.org/doc/stable/reference/generated/numpy.trapz.html
