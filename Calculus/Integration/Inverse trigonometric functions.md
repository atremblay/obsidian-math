$$
\int \frac{1}{\sqrt{1-x^2 }} \, dx = \arcsin x+C
$$

$$
\int -\frac{1}{\sqrt{1-x^2 }} \, dx = \arccos x+C
$$
$$
\int \frac{1}{1+x^2 } \, dx = \arctan x+C
$$
$$
\int -\frac{1}{1+x^2 } \, dx = \operatorname{arc cot} x+C
$$
$$
\int \pm\frac{1}{x\sqrt{x^2-1 }} \, dx = \operatorname{arc sec} x+C
$$
$$
\int \mp\frac{1}{x\sqrt{x^2-1 }} \, dx = \operatorname{arc csc} x+C
$$

In the last two formulas the upper sign holds if $x > 1$ and the lower sign holds if $x < – 1$. 

Version with [[../Derivative/Chain Rule]]/[[Substitution rule]]. Recall that 
$$
\frac{dy}{dx} = \frac{dy}{du}\frac{du}{dx}
$$

so
$$
y=\int \frac{dy}{dx} dx=  \int \frac{dy}{du}\frac{du}{dx} dx
$$

$$
\int \frac{1}{\sqrt{1-u^2 }} \, \frac{\mathrm{d}u}{\mathrm{d}x}dx = \arcsin u+C
$$

$$
\int -\frac{1}{\sqrt{1-u^2 }} \, \frac{\mathrm{d}u}{\mathrm{d}x}dx = \arccos u+C
$$
$$
\int \frac{1}{1+u^2 } \, \frac{\mathrm{d}u}{\mathrm{d}x}dx = \arctan u+C
$$
$$
\int -\frac{1}{1+u^2 } \, \frac{\mathrm{d}u}{\mathrm{d}x}dx = \operatorname{arc cot} u+C
$$
### arcsec

$$
\begin{drcases}
    \int \pm\frac{1}{u\sqrt{u^2-1 }} \, \frac{\mathrm{d}u}{\mathrm{d}x}dx = \operatorname{arc sec} u+C
\end{drcases} \qquad \begin{array}{}+&\text{ if }& u\gt1 \\-&\text{ if }& u\lt -1\end{array} 
$$

### arccsc
$$
\begin{drcases}
    \int \mp\frac{1}{u\sqrt{u^2-1 }} \, \frac{\mathrm{d}u}{\mathrm{d}x}dx = \operatorname{arc csc} u+C
\end{drcases} \qquad \begin{array}{}-&\text{ if }& u\gt1 \\+&\text{ if }& u\lt -1\end{array} 
$$


$$
\int \sin (x) d x=-\cos (x) + C
$$
$$
\int \cos (x) d x=\sin (x) + C
$$

$$
\int \tan (x) d x=-\log (\cos (x)) + C
$$

> [!step]-
> 
> Step 1
> Take the integral:
> $$\int \tan (x) d x$$
> Step 2
> Rewrite $\tan (x)$ as $\frac{\sin (x)}{\cos (x)}$ :
> $$=\int \frac{\sin (x)}{\cos (x)} d x$$
> Step 3
> For the integrand $\frac{\sin (x)}{\cos (x)}$, substitute $u=\cos (x)$ and $d u=-\sin (x) d x$ :
> $$=\int-\frac{1}{u} d u$$
> Step 4
> Factor out constants:
> $$=-\int \frac{1}{u} d u$$
> Step 5
> The integral of $\frac{1}{u}$ is $\log (u)$ :
> $$=-\log (u)+\text { constant }$$
> Step 6
> Substitute back for $u=\cos (x)$ :
> Answer:
> $$=-\log (\cos (x))+\text { constant }$$
> 

$$
\int \csc (x) dx =\log \left(\sin \left(\frac{x}{2}\right)\right)-\log \left(\cos \left(\frac{x}{2}\right)\right)+C 
$$

$$
\int \sec (x) d x=\log \left(\sin \left(\frac{x}{2}\right)+\cos \left(\frac{x}{2}\right)\right)-\log \left(\cos \left(\frac{x}{2}\right)-\sin \left(\frac{x}{2}\right)\right) + C
$$
$$
\int \cot (x) d x=\log (\sin (x)) + C
$$

$$
\int \operatorname{arcsin}(x) d x=\sqrt{1-x^2}+x \operatorname{arcsin}(x)+C
$$
$$
\int \arccos (x) d x=x \arccos (x)-\sqrt{1-x^2} + C
$$
$$
\int \operatorname{arccot}(x) d x=\frac{1}{2} \log \left(x^2+1\right)+x \operatorname{arccot}(x) + C
$$

$$
\begin{aligned}
& \int \operatorname{arccsc}(x) d x= \frac{\sqrt{1-\frac{1}{x^2}} x\left(\log \left(\frac{x}{\sqrt{x^2-1}}+1\right)-\log \left(1-\frac{x}{\sqrt{x^2-1}}\right)\right)}{2 \sqrt{x^2-1}}+x \operatorname{arccsc}(x) + C
\end{aligned}
$$

$$
\int \operatorname{arcsec}(x) d x= \frac{\sqrt{1-\frac{1}{x^2}} x\left(\log \left(1-\frac{x}{\sqrt{x^2-1}}\right)-\log \left(\frac{x}{\sqrt{x^2-1}}+1\right)\right)}{2 \sqrt{x^2-1}}+x \operatorname{arcsec}(x) + C
$$

