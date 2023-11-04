We consider a column of air of cross-sectional area $A$, of height $d h$ and at a distance $h$ units from the surface of the earth, Fig. 17.65. For simplicity, we assume the earth is a plane, the air is at rest, and is unaffected by changes of latitude, longitude, and temperature. The positive direction is upwards. Let $p$ be the pressure ${ }^{*}$ on this column of air, due to the weight of all the air above it. Hence the total force $P$ on a column of air of cross-sectional area $A$ is $P=A p$. By differentiation we obtain

(a)

$$
d P=A d p
$$

*Pressure is the force acting perpendicularly on a unit area of surface. which gives the change of the total force $P$ for a change in the pressure $p$. Let $\rho$ be the weight of a unit volume of air. Therefore the weight of a column of air of height $d h$ and cross-sectional area $A$ is

$$
(A d h) \rho \text {. }
$$

The decrease in total force as one goes from the height $h$ to the height $h+d h$ must be equal to the weight of the column of air of thickness $d h$. Hence equating the negative of (a) with (b), we obtain

$$
\text { (17.66) }-A d p=\rho A d h, \quad \frac{d p}{d h}=-\rho,
$$

which states that the rate of change of air pressure with height is equal to the negative of the weight of a unit volume of air at that height. The units we shall use are: pounds per cubic foot for $\rho$, pounds


```tikz

\usepackage{tikz}
\begin{document}

\begin{tikzpicture}[scale=3]
% Bottom face filled
\fill[gray!70] (0,0) -- (1,0) -- (1.3,0.3) -- (0.3,0.3) -- cycle;
% Back face
\draw (0,0) -- (0,1.5) -- (0.3,1.8) -- (0.3,0.3) -- cycle;
% Left face
\draw (0,0) -- (1,0) -- (1.3,0.3) -- (0.3,0.3) -- cycle;
% Right face
\draw (1,0) -- (1,1.5) -- (1.3,1.8) -- (1.3,0.3) -- cycle;
% Top face
\draw (0,1.5) -- (0.3,1.8) -- (1.3,1.8) -- (1,1.5) -- cycle;
% Front face
\draw (0,0) -- (0,1.5) -- (1,1.5) -- (1,0) -- cycle;
\node[right, font=\Large] at (1,1) {$h$};
\node[above right, font=\Large] at (1.3,0.3) {Surface of the earth};

% top face filled
\fill[gray!70] (0,1.6) -- (0.3,1.9) -- (1.3,1.9) -- (1,1.6) -- cycle;
% left face filled
\draw (0,1.5) -- (0,1.6) -- (0.3,1.9) -- (0.3,1.8) -- cycle;
% right face filled
\draw (1,1.5) -- (1,1.6) -- (1.3,1.9) -- (1.3,1.8) -- cycle;
\node[
    pin={[font=\large, pin edge={thick}, anchor=west, pin distance=0.8cm]0:{$dh$}}
            ] at (1.3,1.85) {};
\node[
    pin={[font=\large, pin edge={thick}, anchor=west, pin distance=1.8cm]45:{$A$}}
            ] at (0.65,1.75) {};

\draw[->, -latex, thick] (2,1.5) -- (2,1.8) node[pos=0.5, right] {+};
\end{tikzpicture}

\end{document}
```

Figure 17.65 per square foot for $p$, and feet for height.

If $p$ is oceanic pressure and $h$ is the depth below sea level, then (17.66) becomes

$$
\frac{d p}{d h}=\rho,
$$

where $\rho$ is the weight per cubic foot of ocean water.

With the aid of (17.66) and (17.67), solve the following problems.

14. Assume that the air is an isothermal gas. Therefore by Boyle's law, its pressure and density are related by the formula

$$
\rho=k p \text {. }
$$

(a) Determine the pressure of the air as a function of the height $h$ above the earth if at sea level the air pressure is $14.7 \mathrm{lb} / \mathrm{sq}$ in. Hint. In $(17.66)$, replace $\rho$ by $k p$.

(b) What is the value of the constant $k$ in (17.671) if $\rho=0.081 \mathrm{lb} / \mathrm{cu} \mathrm{ft}$ at sea level?

(c) What is the air pressure at $10,000 \mathrm{ft}$, at $15,000 \mathrm{ft}$, at $70,000 \mathrm{ft}$, at $50 \mathrm{mi}$ ?

(d) Show that the pressure is zero only when $h$ is infinite.

(e) Express the density of air as a function of height. Hint. In (17.66) replace $d p$ by $d \rho / k$ as obtained from (17.671).

(f) What is the weight of the air at an altitude of $1000 \mathrm{ft}$, of $5000 \mathrm{ft}$, of $10,000 \mathrm{ft}$, of $50,000 \mathrm{ft}$ ?

15. If air expands adiabatically, i.e., without gaining or losing heat, then $p=k \rho^{1.4}$.

(a) Find the pressure of the air as a function of height. Assume that at sea level $p=14.7 \mathrm{lb} / \mathrm{sq}$ in. and $\rho=0.081 \mathrm{lb} / \mathrm{cu} \mathrm{ft}$. Hint. First find the value of $k$. Then in $(17.66)$ replace $\rho$ by $(p / k)^{5 / 7}$. (b) How high is the atmosphere, i.e., at what height is $\rho=0$ ? Hint. In (17.66) replace $d p$ by $1.4 k \rho^{0.4} d p$. Solve for $\rho$. Then find $h$ when $\rho=0$.

16. Assume that the weight $\rho$ of a cubic foot of sea water, under a pressure of $p \mathrm{lb} / \mathrm{ft}^{2}$ is given by the formula

$$
\rho=k\left(1+2 \cdot 10^{-8} p\right) \mathrm{lb} / \mathrm{ft}^{3}
$$

and is $64 \mathrm{lb} / \mathrm{ft}^{3}$ at sea level.

(a) Find the value of $k$. Hint. At sea level $p=0$.

(b) Find the pressure of sea water as a function of its depth below sea level. Hint. In (17.67), replace $\rho$ by its value as given in (17.672).

(c) Find the weight per cubic foot of sea water as a function of depth. Hint. In (17.67), replace $d p$ by $10^{8} d \rho / 2 k$ as determined from (17.672).

(d) What is the pressure and density of sea water at $20,000 \mathrm{ft}$ below sea level?

17. If the earth is assumed spherical instead of planar, show that (17.66) becomes

$$
\frac{d p}{d h}=-\rho-\frac{2 p}{R+h},
$$

where $R$ is the radius of the earth. Hint. See Fig. 17.69. The volume of a thin spherical shell of air at a distance $r$ units from the center of the earth and thickness $d r$ is $4 \pi r^{2} d r$. Its weight, therefore, is $\left(4 \pi r^{2} d r\right) \rho$. The total

[](https://cdn.mathpix.com/cropped/2023_07_30_ca1c1f7b3544dd2eae87g-202.jpg?height=240&width=485&top_left_y=937&top_left_x=357)

Figure 17.69

force $P$ on this spherical surface due to the weight of the air outside it is $P=4 \pi r^{2} p$. Hence $d P=4 \pi\left(r^{2} d p+2 r p d r\right)$. Set $-d P$, which is the decrease in the total force $P$ as one goes from the distance $r$ to the distance $r+d r$, equal to the weight of the spherical shell.