##### Example
#example 
> [!Example] Statement
> The number of bacteria in a yeast culture grows at a rate which is proportional to the number present. If the population of a colony of yeast bacteria doubles in one hour, find the number of bacteria which will be present at the end of 3.5 hours.

Using [[../Solutions/Separation of variables|separation of variables]]
$$\lim_{\Delta t \rightarrow 0}\frac{\Delta x}{\Delta t} = \frac{dx}{dt} = kx \implies x(t)=x_0e^{kt}$$
In this problem we do not care about the initial population value other than there is one, $x_0$.

$$x(1) = 2x_0 = x_0e^{k} \implies k=\ln{2}$$
$$x(3.5) = x_0e^{3.5\ln 2} \approx 11.31x_0  $$
##### Example
#example 
> [!Example] Statement
> The death rate of an ant colony is proportional to the number present. If no births were to take place, the population at the end of one week would be reduced by one-half. However because of births, the rate of which is also proportional to the population present, the ant population doubles in 2 weeks. Determine the birth rate of the colony per week.

Split the problem in two. Start with the scenario with pure deaths.
Using [[../Solutions/Separation of variables|separation of variables]]
$$\frac{dx}{dt} = -k_dx \implies x(t)=x_0e^{-k_dt}$$
where $k_d$ is the proportion of death.

In this problem we do not care about the initial population value other than there is one, $x_0$.

$$x(1) = 0.5x_0 = x_0e^{-k_d} \implies k_d=-\ln{0.5}$$

Now we incude the birth rate $k_b$

$$\begin{align}
x^\prime &= -k_dx + k_bx \\
\int \frac{1}{x(k_b-k_d)}\frac{dx}{dt}dt &= \int  dt \\
\frac{1}{k_b-k_d}\int \frac{1}{x} dx &= \int  dt \\
\frac{1}{k_b-k_d} \ln(x) &= t + C_1 \\
\ln(x(t)) &= (k_b-k_d)t + C_2 \\
x(t) &= Ce^{(k_b-k_d)t} \\
x(t)=x_0e^{(k_b-k_d)t}
\end{align}
$$
With birth and death, the population doubles in 2 weeks

$$x(2) = 2x_0 = x_0e^{(k_b+\ln 0.5)2} \implies k_b= 0.5\ln(2) - \ln(0.5) \approx 1.0397  $$


##### Example
#example 
> [!Example] Statement
> The population of a colony doubles in 50 days. In how may days will the population triple?

From all the previous examples, we know that the growth rate is
$$
x(t) = x_0e^{kt}
$$
Given the information that the population doubles after 50 days, we can find $k$
$$
\begin{align}
x(50) = 2x_0 &= x_0 e^{50k} \\
\iff k &= \frac{\ln{2}}{50}
\end{align}
$$
It will triple at $t^*$
$$
\begin{align}
x(t^*) = 3x_0 &= x_0e^{t^*\frac{\ln{2}}{50}} \\
3 &= e^{t^*\frac{\ln{2}}{50}} \\
\iff t^* &= \frac{50\ln{3}}{\ln{2}} \approx 79.25
\end{align}
$$

##### Example
#example 
> [!Example] Statement
> Assume that the half life of the radium in a piece of lead is 1500 years. How much radium will remain in the lead after 2500 years?

Start with
$$x(t) = x_0e^{-kt}$$
Since we are talking about decay, the exponent must be negative. We know that
$$
\begin{align}
x(1500) = 0.5x_0 &= x_0e^{-1500k} \\
\iff k &= -\frac{\ln{0.5}}{1500}
\end{align}
$$
Let's use $p$ to denote the percentage left after 2500 years 
$$
\begin{align}
x(2500) = px_0 &= x_0 e^{--2500\frac{\ln{0.5}}{1500}} \\ 
p &= e^{\frac 5 3 \ln{0.5}} \approx 0.31498026247371900
\end{align}
$$

$$\boxed{p \approx 0.31498}$$

##### Example
#example 
> [!Example] Statement
> If 1.7% of a substance decomposes in 50 years, what percentage of the substance will remain after 100 years? How many years will be required for 10% to decompose?

Again start with
$$x(t) = x_0e^{-kt}$$
Since we are talking about decay, the exponent must be negative. We know that
$$
\begin{align}
x(50) = 0.983x_0 &= x_0e^{-50k} \\
\iff k &= -\frac{\ln{0.983}}{50}
\end{align}
$$
> Note that as long as we are dealing with the same unit of time throughout the problem, we don't need to worry about the unit of time.

Let's use $p$ to denote the percentage left after 100 years 
$$
\begin{align}
x(100) = px_0 &= x_0 e^{--100\frac{\ln{0.983}}{50}} \\ 
p &= e^{2\ln{0.983}} = 0.966289
\end{align}
$$
Let's use $t^\prime$ to denote the number of years for a decay of 10%
$$
\begin{align}
x(t^\prime) = 0.9x_0 &= x_0 e^{t^\prime\frac{\ln{0.983}}{50}} \\ 
t^\prime &= \frac{50\ln{0.9}}{\ln{0.983}} \approx 307.24
\end{align}
$$

##### Example
#example 
> [!Example] Statement
> The bacteria count in a culture is 100 000. In 2.5 hours, the number has increased by 10 percent. 

$$
x(t) = 100000e^{kt}
$$
$$
\begin{align}
x(2.5) &= 1.1*100000 = 100000e^{2.5k} \\
\iff k = \frac{\ln{1.1}}{2.5}
\end{align}
$$


> [!Example] (a)
> In how many hours will the count reach 200 000?

$$
t = \frac{2.5\ln{2}}{\ln{1.1}} \approx 18.18135
$$


> [!Example] (b)
> What will the bacteria count be in 10 hours?

$$
p_{10} = e^{\frac{10}{2.5}\ln{1.1}} = 1.4641
$$

This is the proportion, so the whole population will be $1.4641 * 100000 = 146410$


##### Example
#example 
> [!Example]  Statement
> The population of country doubles in 50 years. Its present population is 20M.
>   


> [!Example] (a)
> When will its population reach 30M?



> [!Example] (b)
> What will its population be in 10 years?


##### Example
#example 
> [!Example] Statement
> 10% of a substance disintegrate in 100 years. What is its half life?


##### Example
#example 
> [!Example] Statement
> The bacteria count in a culture doubles in 3 hours. At the end of 15 hours, the count is 1M. How many bacteria were in the count initially?



##### Example
#example 
> [!Example] Statement
> By natural increase, a city, whose population is 40k, will double in 50 years. There is a net addition of 400 persons per hyear becuase of people leaving and moving into the city. Estimate its population in 10 years.


##### Example
#example 
> [!Example] Statement
> Solve the previous problem if there is a net decrease in the pupulation of 400 persons per year.


##### Example
#example 
> [!Example] Statement
> A culture of bacteria whose pupulation is $N_0$ will, by natural increase, double in $4\log 2$ days. If bacteria are extracted from the colony as at the uniform rate of $R$ per day, fin dthe number of bacteria present as a function of time. Show that the pupulation will increase if $R \lt N_0/4$, will remain stationay if $R=N_0/4$ and will decrease if $R \gt N_0/4$


##### Example
#example 
> [!Example] Statement
> If the rate of loss of volume of a spherical substance, for example a moth ball, due to evaporation, is proporional to its surface area. Express the radius of the ball as a function of time.


##### Example
#example 
> [!Example] Statement
> The volume of a spherical raindrop increases as it falls because of the adhesion to its surface of mist particles. Assume it retains its spherical shape during its fall and that the rate of change of its volume with respect to the distance $y$ it has fallen, is proportional to the surface area at that distance. Express the radius of the raindrop as a function of $y$


















