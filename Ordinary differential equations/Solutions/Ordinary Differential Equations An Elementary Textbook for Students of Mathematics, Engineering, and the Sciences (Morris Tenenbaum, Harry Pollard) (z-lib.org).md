

## 3. LESSON 8. Differential Equations with Linear Coefficients.

LESSON 8A. A Review of Some Plane Analytic Geometry. The first degree equation $a x+b y+c=0$ represents a straight line. For this reason it is called a linear equation. (Nots. The presence of the constant $c$ in the equation prevents the function defined by it from being homogeneous.) If the coefficients of $x$ and $y$ in one linear equation are proportional to the $x$ and $y$ coefficients in another, the two lines they represent are parallel. For example, the two lines

$$
\begin{aligned}
& 3 x-2 y+7=0, \\
& 6 x-4 y+3=0,
\end{aligned}
$$

are parallel since $3:-2=6:-4$. If the three constants in one linear equation are proportional to the three constants respectively in a second linear equation, the two lines coincide, i.e., they are the same line. For example, the two lines

$$
\begin{aligned}
& 2 x+3 y+1=0 \\
& 4 x+6 y+2=0
\end{aligned}
$$

are coincident. (Do you see why?)

Another concept of analytic geometry that we shall need for this lesson is that of "translation of axes." Let $(x, y)$ be the coordinates of a point $\boldsymbol{P}$ with respect to an origin $(0,0)$ (Fig. 8.1), and let us translate the origin to

![](https://cdn.mathpix.com/cropped/2023_07_02_9fc027f3b8ce4f58fe60g-076.jpg?height=459&width=692&top_left_y=1131&top_left_x=250)

Pigure 8.

a new position whose $x$ and $y$ distances from $(0,0)$ are $h$ and $k$ respectively. To distinguish the new origin from the old one, we call its coordinates $(\overline{0}, \overline{0})$. The point $P$ will then have two sets of coordinates, one with respect to $(\overline{0}, \overline{0})$, which we designate by $(\bar{x}, \bar{y})$, and the other with respect to $(0,0)$, which we have already designated as $(x, y)$. This means that if a point is measured from $(0,0)$, its coordinates have no bars over them; if it is measured from $(\overline{0}, \overline{0})$ its coordinates have bars over them. The question we now ask and whose answer we seek is this. What is the relationship between the two sets of coordinates $(x, y)$ and $(\bar{x}, \bar{y})$ ?

If you will examine Fig. 8.1 carefully, you will see that

$$
x=x+h, \quad y=y+k .
$$

Hence by (8.11),

$$
x=x-h, \quad y=y-k .
$$

These are the equations of translation. Their purpose, you may recall, is to change more complicated second degree equations into simpler ones by eliminating the first degree terms. We shall now demonstrate how a translation of axes can help solve a differential equation with linear coefficients.

LESSON 8B. Solution of a Differential Equation in Which the Coefficients of $d x$ and $d y$ Are Linear, Nonhomogeneous, and When Equated to Zero Represent Nonparallel Lines. Consider the differential equation

$$
\left(a_{1} x+b_{1} y+c_{1}\right) d x+\left(a_{2} x+b_{2} y+c_{2}\right) d y=0
$$

in which the coefficients of $d x$ and $d y$ are linear and when equated to zero represent nonparallel lines. We assume also that both $c_{1}$ and $c_{2}$ are not zero. (If both $c_{1}=0$ and $c_{2}=0$, then (8.2) is a differential equation with homogeneous coefficients which can be solved by the method of Lesson 7.) Since the coefficients in (8.2) are assumed to define nonparallel lines, the pair of equations

$$
\begin{aligned}
& a_{1} x+b_{1} y+c_{1}=0, \\
& a_{2} x+b_{2} y+c_{2}=0,
\end{aligned}
$$

formed with them, have a unique point of intersection and therefore a unique solution for $x$ and $y$. Let us call this point $(h, k)$. If we now translate the origin to $(h, k)$, then by $(8.11),(8.2)$ becomes, with respect to this new origin $(\overline{0}, \overline{0})$,

$$
\begin{aligned}
{\left[a_{1}(x+h)+b_{1}(y+k)\right.} & \left.+c_{1}\right] d x \\
& +\left[a_{2}(x+h)+b_{2}(\bar{y}+k)+c_{2}\right] d y=0
\end{aligned}
$$

which simplifies to

$$
\begin{aligned}
& {\left[a_{1} x+b_{1} y+\left(a_{1} h+b_{1} k+c_{1}\right)\right] d x } \\
&+\left[a_{2} x+b_{2} \bar{y}+\left(a_{2} h+b_{2} k+c_{2}\right)\right] d \bar{y}=0
\end{aligned}
$$

But $(h, k)$ is the point of intersection of the two lines in (8.21) and therefore lies on both of them. Hence the term in the parentheses in each bracket of $(8.23)$ is zero. This equation therefore reduces to

$$
\left(a_{1} x+b_{1} \bar{y}\right) d x+\left(a_{2} x+b_{2} y\right) d y=0
$$

which is now a homogeneous type solvable for $x$ and $y$ by the method of Lesson 7. By (8.11) we can then find solutions in terms of $x$ and $y$.

Note.

1. The left-hand members of the system (8.21) by which $h$ and $k$ are determined are the coefficients in the given differential equation (8.2).
2. Equation (8.24) which is equivalent to (8.2) with respect to a new origin translated to the point $(h, k)$, can be easily obtained from (8.2). Omit the constants $c_{1}$ and $c_{2}$ and place bars over $x$ and $y$.

Example 8.25. Find a 1-parameter family of solutions of

$$
(2 x-y+1) d x+(x+y) d y=0 .
$$

Solution. The coefficient of $d x$ is linear but nonhomogeneous, and the two lines defined by the coefficients of $d x$ and $d y$ are nonparallel. Hence the procedure outlined above applies. Solving simultaneously the two equations determined by the coefficients of $d x$ and $d y$, namely,

(b)

$$
\begin{array}{r}
2 x-y+1=0, \\
x+y=0,
\end{array}
$$

we find that their point of intersection is $\left(-\frac{1}{3}, \frac{1}{3}\right)$. Hence, in (8.11) $h=-\frac{1}{3}, k=\frac{1}{3}$. Translating the origin to the point $\left(-\frac{1}{3}, \frac{1}{3}\right)$, the equations of translation are by $(8.11)$ and (8.12)
(c) $x=x-\frac{1}{3}, \quad y=y+\frac{1}{3}, \quad x=x+\frac{1}{3}, \quad \bar{x}=y-\frac{1}{3}$.

By (8.24), (a) becomes with respect to this new origin (see Note 2 above)

$$
(2 \bar{x}-\bar{y}) d \bar{x}+(\bar{x}+\bar{y}) d \bar{y}=0 .
$$

To solve it, we apply the method of Lesson 7. Let

$$
y=u x, \quad d y=u d x+x d u
$$

By substituting (e) in (d) and following the procedure outlined in Lesson 7, we obtain

(f) $\log |x|=c_{1}-\frac{1}{\sqrt{2}} \operatorname{Arctan} \frac{u}{\sqrt{2}}-\frac{1}{2} \log \left|2+u^{2}\right|, x \neq 0$, which is a 1-parameter family of solutions of (d). In (f), we replace $u$ by its value in (e) and multiply by 2 . There results

(g) $\quad \log \left|\bar{x}^{2}\left(2+\frac{y^{2}}{x^{2}}\right)\right|=c-\sqrt{2}$ Arctan $\frac{y}{\sqrt{2} x}, \quad \bar{x} \neq 0$.

Substituting the last two equations of (c) in (g) gives finally

(h) $\log \left[2\left(\frac{3 x+1}{3}\right)^{2}+\left(\frac{3 y-1}{3}\right)^{2} \mid=c-\sqrt{2} \operatorname{Arctan} \frac{3 y-1}{\sqrt{2}(3 x+1)}\right.$,

$$
x \neq-\frac{1}{3}
$$

Comment 8.26. The solution (h) above is an excellent example of the point made at the beginning of this chapter in introductory remark No. 3. Here is a solution written in implicit form, which has little value for practical purposes. To try to find the function $g(x)$ implicitly defined by this relation would be an extremely laborious if not a hopeless task. In general a 1-parameter family of solutions of a differential equation with linear coefficients or with homogeneous coefficients will usually be a complicated expression of this kind. In these cases, more important for practical purposes than an implicit solution is a knowledge of the approximate behavior of the integral curves. There are fortunately means available by which it is possible to determine the character of these integral curves from the differential equation (8.2) itself, without the need to solve it. Since differential equations with homogeneous or linear coefficients arise in practical problems when trying to find an approximation to the behavior of the motion of a particle whose velocities in the $x$ and $y$ directions are given by the two differential equations

$$
\begin{aligned}
& \frac{d x}{d t}=f(x, y), \\
& \frac{d y}{d t}=g(x, y),
\end{aligned}
$$

we have deferred to Lesson 32 a further discussion of this important topic. We shall show there, how it is possible to find an approximation of the particle's motion by changing the two equations into one equation with linear coefficients, and then showing how more useful information can be obtained from the resulting differential equation itself than from its usual complicated implicit solution. We shall thus be able to learn what the solution (h) in the above example approximately looks like, not from this solution, but from the given differential equation (a); see Example 32.44. LESSON 8C. A Second Method of Solving the Differential Equation (8.2) with Nonhomogeneous Coefficients. In (8.2), let

$$
\begin{aligned}
& u=a_{1} x+b_{1} y+c_{1}, \\
& v=a_{2} x+b_{2} y+c_{2} .
\end{aligned}
$$

Therefore

$$
\begin{aligned}
& d u=a_{1} d x+b_{1} d y \\
& d v=a_{2} d x+b_{2} d y
\end{aligned}
$$

Now solve (8.31) for $d x$ and $d y$. The substitution in (8.2) of (8.3) and these values of $d x$ and $d y$ will also lead to a differential equation with homogeneous coefficients solvable by the method of Lesson 7.

Example 8.32. Find a 1-parameter family of solutions of

$$
(2 x-y+1) d x+(x+y) d y=0 .
$$

Solution. As indicated in (8.3), we let

$$
u=2 x-y+1, \quad v=x+y .
$$

Therefore

$$
\begin{aligned}
& d u=2 d x-d y, \\
& d v=d x+d y,
\end{aligned}
$$

The solution of (c) for $d x$ and $d y$ is

$$
d x=\frac{d u+d v}{3}, \quad d y=-\frac{d u-2 d v}{3}
$$

Substituting (b) and (d) in (a), we obtain

$$
u\left(\frac{d u+d v}{3}\right)-v\left(\frac{d u-2 d v}{3}\right)=0
$$

which simplifies to

$$
(u-v) d u+(u+2 v) d v=0
$$

This equation is now of the type with homogeneous coefficients. Following the method of Lesson 7, we let

$$
u=t v, \quad d u=t d v+v d t .
$$

Substituting these values in (f), we obtain

$$
(t v-v)(t d v+v d t)+(t v+2 v) d v=0
$$

which reduces to

$$
\frac{d v}{v}+\frac{t-1}{t^{2}+2} d t=0, \quad v \neq 0
$$

Its solution is

(j) $\log |v|+\frac{1}{2} \log \left(t^{2}+2\right)-\frac{1}{\sqrt{2}} \operatorname{Arctan} \frac{t}{\sqrt{2}}=c, \quad v \neq 0$,

$$
\log \left[v^{2}\left(t^{2}+2\right)\right]=C+\sqrt{2} \operatorname{Arctan} \frac{t}{\sqrt{2}}, \quad v \neq 0
$$

By (g) and (b),

$$
t=\frac{u}{v}=\frac{2 x-y+1}{x+y}, x+y \neq 0
$$

Substituting (k) in (j), we obtain

(l) $\log \left[(2 x-y+1)^{2}+2(x+y)^{2}\right]=C+\sqrt{2} \operatorname{Arctan} \frac{2 x-y+1}{\sqrt{2}(x+y)}$, $x+y \neq 0$.

LESSON 8D. Solution of a Differential Equation in Which the Coefficients of $d x$ and $d y$ Define Parallel or Coincident Lines. If the lines defined by the coefficients of $d x$ and $d y$ in (8.2) are parallel, the method of Lesson 8B will not work. Parallel lines do not have a point of intersection and therefore (8.21) has no solution for $x$ and $y$. In this case we must resort to a different substitution. It is illustrated in the following example.

Example 8.4. Find a 1-parameter family of solutions of

$$
(2 x+3 y-1) d x+(4 x+6 y+2) d y=0,
$$

also any particular solution not obtainable from the family.

Solution. We observe that the lines defined by the coefficients of $d x$ and $d y$ are parallel but not coincident lines. In all such cases, the substitution of a new variable for the coefficient of $d x$ or of $d y$ will transform the equation into one which is separable. We therefore let

(b) $u=2 x+3 y-1, \quad d u=2 d x+3 d y, \quad d x=\frac{d u-3 d y}{2}$.

Then by (b)

$$
2 u+4=4 x+6 y+2 \text {. }
$$

Substituting (b) and (c) in (a), we obtain

$$
u\left(\frac{d u-3 d y}{2}\right)+(2 u+4) d y=0
$$

which simplifies to

$$
u d u+(u+8) d y=0
$$

an equation whose variables are separable. If $u \neq-8$, (e) can be written as

$$
\frac{u}{u+8} d u+d y=0, \quad u \neq-8
$$

Integration of (f) gives

$$
u-8 \log |u+8|+y=c, \quad u \neq-8 .
$$

Finally, replace in (g) the value of $u$ as given in (b), noting at the same time that the exclusion of $u=-8$ implies the exclusion of the line $2 x+3 y+7=0$. Hence (g) becomes

(h) $2 x+3 y-1-8 \log |2 x+3 y+7|+y=c, 2 x+3 y+7 \neq 0$,

which is a 1-parameter family of solutions of (a).

The function defined by

$$
2 x+3 y+7=0
$$

which had to be excluded in obtaining (h) also satisfes (a). (Be sure to verify it.) It is a particular solution not obtainable from the family (h).

Example 8.41. Find a 1-parameter family of solutions of
(a)
$(2 x+3 y+2) d x+(4 x+6 y+4) d y=0$

also any particular solution not obtainable from the family.

Solution. We observe that the coefficients in (a) define the same line. If we exclude values of $x$ and $y$ for which

$$
2 x+3 y+2=0,
$$

we may divide (a) by it and obtain

$$
d x+2 d y=0
$$

Its solution is

$$
x+2 y=c,
$$

which is valid for those values of $x$ and $y$ which do not lie on the line $2 x+3 y+2=0$. It is the required 1-parameter family. However, the function defined by

$$
2 x+3 y+2=0
$$

also satisfies (a). It is a particular solution not obtainable from the family.

## 4. EXERCISE 8

Find a 1-parameter family of solutions of each of the following equations.

1. $(x+2 y-4) d x-(2 x-4 y) d y=0$.
2. $(3 x+2 y+1) d x-(3 x+2 y-1) d y=0$.
3. $(x+y+1) d x+(2 x+2 y+2) d y=0$.
4. $(x+y-1) d x+(2 x+2 y-3) d y=0$.
5. $(x+y-1) d x-(x-y-1) d y=0$.
6. $(x+y) d x+(2 x+2 y-1) d y=0$.
7. $(7 y-3) d x+(2 x+1) d y=0$.
8. $(x+2 y) d x+(3 x+6 y+3) d y=0$.
9. $(x+2 y) d x+(y-1) d y=0$.
10. $(3 x-2 y+4) d x-(2 x+7 y-1) d y=0$.

Find a particular solution, satisfying the initial condition, of each of the following differential equations.

11. $(x+y) d x+(3 x+3 y-4) d y=0, y(1)=0$.
12. $(3 x+2 y+3) d x-(x+2 y-1) d y=0, y(-2)=1$.
13. $(y+7) d x+(2 x+y+3) d y=0, y(0)=1$.
14. $(x+y+2) d x-(x-y-4) d y=0, y(1)=0$.

## 5. ANSWERS 8

1. $\log \left[4(y-1)^{2}+(x-2)^{2}\right]-2 \operatorname{Arctan} \frac{2 y-2}{x-2}=c$.
2. $\log |15 x+10 y-1|+\frac{5}{2}(x-y)=c$.
3. $x+2 y=c$.
4. $x+2 y+\log |x+y-2|=c$.
5. $(x-1)^{2}+y^{2}=c e^{2 \operatorname{Aro} \tan \{y /(x-1) !}$.
6. $x+2 y+\log |x+y-1|=c$.
7. $7 \log |2 x+1|+2 \log |7 y-3|=c$.
8. $x+3 y-3 \log |x+2 y+3|=c$.
9. $[(x+2) /(x+y+1)]+\log |x+y+1|=c$.
10. $3 x^{2}-4 x y+8 x-7 y^{2}+2 y=c$.
11. $x+3 y+2 \log (2-x-y)=1$.
12. $(2 x+2 y+1)(3 x-2 y+9)^{4}=-1$.
13. $(y+7)^{2}(3 x+y+1)=128$.
14. $\log \left[(x-1)^{2}+(y+3)^{2}\right]+2 \operatorname{Arctan} \frac{x-1}{y+3}=2 \log 3$.
