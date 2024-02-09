We consider now functions defined for all points in some set $A$ in $R^{n}$ and with values real numbers. Such functions are called real-valued functions. The set $A$ is called the domain of $f$ and the set of values of $f$ is called its range. For simplicity in this discussion, we take $n=2$. Thus we consider only functions of two independent variables. All the definitions and theorems that we mention below are valid for functions of more than two variables. The student should formulate the appropriate statements of the corresponding definitions and theorems for such functions.

Let $f$ be a function defined for all points $(x, y)$ in some set $A$ in $R^{2}$. Let $\left(x^{0}, y^{0}\right)$ be a fixed point in $A$ or on the [[../Ordinary differential equations/Definitions/(Open, Closed) Set#^505957|boundary]]. We say that $f$ has limit $L$ as $(x, y)$ approaches $\left(x^{0}, y^{0}\right)$ and we write

$$
\lim _{(x, y) \rightarrow\left(x^{0}, y^{0}\right)} f(x, y)=L
$$

if, given any $\epsilon>0$, there is a $\delta>0$ such that

$$
|f(x, y)-L|<\epsilon
$$

for all $(x, y) \neq\left(x^{0}, y^{0}\right)$ such that

$$
(x, y) \in B\left(\left(x^{0}, y^{0}\right), \delta\right) \cap A .
$$

In other words, $f(x, y) \rightarrow L$ as $(x, y) \rightarrow\left(x^{0}, y^{0}\right)$ if given any positive number $\epsilon$ we can find a positive number $\delta$ such that the distance of $f(x, y)$ from $L$ is less than $\epsilon$ for all points $(x, y)$ of $A$ which are at a distance less than $\delta$ from $\left(x^{0}, y^{0}\right)$ excepting possibly the point $\left(x^{0}, y^{0}\right)$ itself. The function $f$ is said to be continuous at the point $\left(x^{0}, y^{0}\right) \in A$ if

$$
\lim _{(x, y) \rightarrow\left(x^{0}, y^{0}\right)} f(x, y)=f\left(x^{0}, y^{0}\right) .
$$

$f$ is called continuous in $A$ if it is continuous at every point of $A$.

We now state an important theorem concerning the maximum and minimurn values of a continuous function: Let $A$ be a closed and bounded set in $R^{n}$ and suppose that $f$ is defined and continuous on $A$. Then $f$ attains its maximum and minimum values in $A$; i.e., there is a point $a \in \mathrm{A}$ such that

$$
f(a) \ge f(x), \quad \text { for every } x \in A
$$

and a point $b \in \mathrm{A}$ such that

$$
f(b) \le f(x), \quad \text { for every } x \in A
$$