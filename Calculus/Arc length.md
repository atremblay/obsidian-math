---
cssclasses:
  - cornell-left
  - cornell-border
---

> [!summary]
> The concept is to break up a line in smaller and smaller segments and add up their length
> For a single independant variable, the arc length is defined as
> $$s(x) = \int \sqrt{ 1+ f^\prime(x) } \mathrm{d}x $$

The arc length is the length of a curve between two points. The idea is pretty simple:

1. Divide the curve in multiple segments $P_{0}$ to $P_{n}$
2. Calculate the length of each segments $P_{i-1}P_{i}$
3. Sum them up

$$S_{n}=\sum_{i=1}^n P_{i-1}P_{i}$$

> [!blank-container|left-medium]
> ![[Images/arc_length.svg]]

The more segments we have, the more accurate the measurement will be. Each segment has a $\Delta x_{i}$ and a $\Delta y_{i}$ that we can use to calculate its length.

$$(P_{i-1}P_{i})^2=\left(\Delta x_{i}\right)^2 + \left(\Delta y_{i}\right)^2$$
It will be convenient after to rewrite the right-hand side
$$(P_{i-1}P_{i})^2=\left(1 + \left(\frac{\Delta y_{i}}{\Delta x_{i}}\right)^2\right)\left(\Delta x_{i}\right)^2$$

$$P_{i-1}P_{i}=\Delta x_{i}\sqrt{  1 + \left(\frac{\Delta y_{i}}{\Delta x_{i}}\right)^2}$$

If we make the intervals infinitely small and sum them up, we have an integral

$$
\begin{align}
\lim_{ n \to \infty } S_{n}&=\lim_{ n \to \infty } \sum_{i=1}^n P_{i-1}P_{i}  \\
&= \lim_{ n \to \infty } \sum_{i=1}^n \sqrt{  1 + \left(\frac{\Delta y_{i}}{\Delta x_{i}}\right)^2}\Delta x_{i}  \\
&= \int \sqrt{ 1+ \frac{\mathrm{d}y}{\mathrm{d}x} } \mathrm{d}x   \\
&= \int \sqrt{ 1+ y^\prime } \mathrm{d}x 
\end{align} 
$$