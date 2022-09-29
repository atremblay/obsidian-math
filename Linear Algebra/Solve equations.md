If we have this generic set of 2 equations and 2 unknowns $x_1$ and$x_2$ 
$$
\begin{align}
a_1x_1+a_2x_2&=A \\
b_1x_1+b_2x_2&=B
\end{align}
$$
Solve in matrix form
$$
\begin{align}
&\left[
    \begin{matrix}
        a_1 & a_2\\
        b_1 & b_2
    \end{matrix}
    \middle\vert
    \begin{matrix}
        A \\ B
    \end{matrix}
\right] \\
\sim &\left[
    \begin{matrix}
        a_1 & a_2\\
        b_1 - \frac{b_1a_1}{a_1}& b_2 - \frac{b_1a_2}{a_1}
    \end{matrix}
    \middle\vert
    \begin{matrix}
        A \\ B- \frac{b_1A}{a_1}
    \end{matrix}
\right] \\
\sim &\left[
    \begin{matrix}
        a_1 & a_2\\
        0 & \frac{b_2a_1 - b_1a_2}{a_1}
    \end{matrix}
    \middle\vert
    \begin{matrix}
        A \\ \frac{Ba_1-b_1A}{a_1}
    \end{matrix}
\right] \\
\sim &\left[
    \begin{matrix}
        a_1 & a_2\\
        0 & 1
    \end{matrix}
    \middle\vert
    \begin{matrix}
        A \\ \frac{Ba_1-b_1A}{b_2a_1 - b_1a_2}
    \end{matrix}
\right] \\
\sim &\left[
    \begin{matrix}
        a_1 & a_2-a_2\\
        0 & 1
    \end{matrix}
    \middle\vert
    \begin{matrix}
        A - \frac{a_2(Ba_1-b_1A)}{b_2a_1 - b_1a_2}\\ 
        \frac{Ba_1-b_1A}{b_2a_1 - b_1a_2}
    \end{matrix}
\right] \\
\sim  &\left[
    \begin{matrix}
        \frac{a_1}{a_1} & 0\\
        0 & 1
    \end{matrix}
    \middle\vert
    \begin{matrix}
        \frac{A(b_2a_1 - b_1a_2)- a_2(Ba_1-b_1A)}{a_1(b_2a_1 - b_1a_2)}\\ 
        \frac{Ba_1-b_1A}{b_2a_1 - b_1a_2}
    \end{matrix}
\right] \\
\sim  &\left[
    \begin{matrix}
        1 & 0\\
        0 & 1
    \end{matrix}
    \middle\vert
    \begin{matrix}
        \frac{Ab_2a_1 - \cancel{a_2b_1A}- a_2Ba_1+\cancel{a_2b_1A}}{a_1(b_2a_1 - b_1a_2)}\\ 
        \frac{Ba_1-b_1A}{b_2a_1 - b_1a_2}
    \end{matrix}
\right] \\
\sim  &\left[
    \begin{matrix}
        1 & 0\\
        0 & 1
    \end{matrix}
    \middle\vert
    \begin{matrix}
        \frac{Ab_2\cancel{a_1} - a_2B\cancel{a_1}}{\cancel{a_1}(b_2a_1 - b_1a_2)}\\ 
        \frac{Ba_1-b_1A}{b_2a_1 - b_1a_2}
    \end{matrix}
\right] \\
\sim  &\left[
    \begin{matrix}
        1 & 0\\
        0 & 1
    \end{matrix}
    \middle\vert
    \begin{matrix}
        \frac{Ab_2 - a_2B}{b_2a_1 - b_1a_2}\\ 
        \frac{Ba_1-b_1A}{b_2a_1 - b_1a_2}
    \end{matrix}
\right]
\end{align}
$$
It's not obvious, but there are determinants in these solutions

$$
x_1=\frac{
    \begin{vmatrix}
        A & a_2\\
        B & b_2
    \end{vmatrix}
    }{
    \begin{vmatrix}
        a_1 & a_2\\
        b_1 & b_2
    \end{vmatrix}
    }, \hspace{2em}
x_2=\frac{
    \begin{vmatrix}
        a_1 & A\\
        b_1 & B
    \end{vmatrix}
    }{
    \begin{vmatrix}
        a_1 & a_2\\
        b_1 & b_2
    \end{vmatrix}
    }
$$

