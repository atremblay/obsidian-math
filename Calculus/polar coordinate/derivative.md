---
aliases:
    - "polar coordinate derivative"
tags:
    - "polar_coordinate"
---
> [!summary]
> $$
> \boxed{
> \cot \psi=\frac{\frac{\mathrm{d} \rho}{\mathrm{d} \theta}}{\rho},\hspace{1em} \tan \psi=\frac{\rho}{\rho^\prime}
> }
> $$
> Where $\psi$ is the angle from the [[#^d30f18|radius vector]] to the tangent line at point $P$ where the derivative is calculated.


 Intuitively, as $\Delta \theta \rightarrow 0$
 1. $Q\rightarrow P$ and the segment $QP$ will have the same slope as $\frac{\mathrm{d}\rho}{\mathrm{d}\theta}$
 2. The segment $OP'$ will be the same as $OP$
 3. The angle $\alpha$ will then be the same as $\psi$

Let's suppose, for intuition's sake, that $PP' \perp P'Q$ so that we can say
$$\cot \alpha = \frac{\Delta \rho}{\rho \Delta \theta} $$
We have an expression involving $\frac{\Delta \rho}{\Delta \theta}$. Taking the limit 
$$\lim_{ \Delta \theta \to 0 } \frac{\Delta \rho}{\Delta \theta} = \frac{\mathrm{d} \rho}{\mathrm{d} \theta} $$
By the third point above

$$
\begin{align}
\lim_{ \Delta \theta \to 0 } &\cot \alpha = \lim_{ \Delta \theta \to 0 } \frac{\Delta \rho}{\rho \Delta \theta} \\ \\
\Rightarrow &\boxed{\cot \textcolor{cyan}{\psi} =  \frac{\frac{\mathrm{d} \rho}{\mathrm{d} \theta}}{\rho}}
\end{align}$$

<span class='centerImg'>![[../Images/Polar coordinate derivative.svg]]</span>

![[../Images/polar_derivative.gif]]

A more formal approach is to use the point $R$. We still want to look at the angle $\alpha$ as $\lim_{ \Delta \theta \to 0 } \cot \alpha = \cot \psi$

$$
\cot \alpha = \frac{RQ}{RP}
$$
However
$$
RQ = \textcolor{orange}{OQ} - \textcolor{violet}{OR} = \textcolor{orange}{\rho + \Delta\rho} - \textcolor{violet}{\rho \cos \Delta \theta}
$$
and 
$$
PR = \rho \sin \Delta \theta
$$
Substitute
$$
\frac{RQ}{RP}=\frac{\textcolor{orange}{\rho + \Delta\rho} - \textcolor{violet}{\rho \cos \Delta \theta}}{\rho \sin \Delta \theta} = \frac{\rho (1- \cos \Delta \theta) + \Delta\rho }{\rho \sin \Delta \theta}
$$
We are close to [[Important limits#^abe062|this]] and [[Important limits#^46d97d|this]] limit, we only need to divide by $\Delta \theta$ in the numerator and denominator
$$
\begin{align}
\frac{RQ}{RP} &= \frac{\rho\frac{ (1- \cos \Delta \theta)}{\Delta \theta} + \frac{\Delta\rho}{\Delta \theta} }{\rho\frac{ \sin \Delta \theta}{\Delta \theta}} \\
&= \frac{
\rho\frac{ (1- \cos \Delta \theta)}{\Delta \theta}
}{
\rho\frac{ \sin \Delta \theta}{\Delta \theta}} 
+ \frac{
\frac{\Delta\rho}{\Delta \theta} 
}{
\rho\frac{ \sin \Delta \theta}{\Delta \theta}
} \\
&= \frac{
\frac{ (1- \cos \Delta \theta)}{\Delta \theta}
}{
\frac{ \sin \Delta \theta}{\Delta \theta}} 
+ \frac{
\frac{\Delta\rho}{\Delta \theta} 
}{
\rho\frac{ \sin \Delta \theta}{\Delta \theta}
} \\
\end{align}
$$
Everything is ready to check the limit (by using the [[Properties#^55ded8|product rule]] and [[Properties#^208b6f|quotient rule]])

$$
\begin{align}
\cot \psi = \lim_{ \Delta \theta \to 0 } \cot \alpha &= \lim_{ \Delta \theta \to 0 } \\
&= \lim_{ \Delta \theta \to 0 } \frac{
\frac{ (1- \cos \Delta \theta)}{\Delta \theta}
}{
\frac{ \sin \Delta \theta}{\Delta \theta}} 
+ \lim_{ \Delta \theta \to 0 } \frac{
\frac{\Delta\rho}{\Delta \theta} 
}{
\rho\frac{ \sin \Delta \theta}{\Delta \theta}
}
\end{align}
$$
By some [[Important limits]]
$$
\begin{align}
\cot \psi = \lim_{ \Delta \theta \to 0 } \cot \alpha &= \lim_{ \Delta \theta \to 0 } \frac{
\frac{ (1- \cos \Delta \theta)}{\Delta \theta}
}{
\frac{ \sin \Delta \theta}{\Delta \theta}} 
+ \lim_{ \Delta \theta \to 0 } \frac{
\frac{\Delta\rho}{\Delta \theta} 
}{
\rho\frac{ \sin \Delta \theta}{\Delta \theta} 
} \\
&= \frac{
0
}{
1
} 
+ \frac{
\frac{\mathrm{d}\rho}{\mathrm{\Delta \theta}} 
}{
\rho
} \\
&= \frac{\rho^\prime}{\rho}
\end{align}
$$

> [!definition] radius vector ($r$ or $\rho$)
> Segment from the origin $O$ to a point $P$ on a curve. 

^d30f18

> [!definition] vectorial angle ($\theta$)
> Angle from the polar axis and the [[#^d30f18|radius vector]]. Typically calculated counter-clockwise. 

^623746

> [!definition] polar axis
> The axis of reference when using the [[#^623746|vectorial angle]] . Pretty much always the x-axis.