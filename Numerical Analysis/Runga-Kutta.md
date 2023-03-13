This is the main method to simulate a dynamical system. It's very efficient and precise. It will not succeed in certain cases, but most integrator are using this, often called ODE45.

Given a system
$$\dot{x} = f(x,t)$$
# RK2
$$
x_{k+1} = x_k + \Delta t f_2
$$
$f_1 = f(x_k, t_k)$
$f_2 = f(x_k + \frac{\Delta t}{2}f_1, t_k+\frac{\Delta t}{2})$


# RK4
$$
x_{k+1} = x_k + \frac{\Delta t}{6}(f_1 + 2f_2 + 2f_3 + f4)
$$
$f_1 = f(x_k, t_k)$
$f_2 = f(x_k + \frac{\Delta t}{2}f_1, t_k+\frac{\Delta t}{2})$
$f_3 = f(x_k + \frac{\Delta t}{2}f_2, t_k+\frac{\Delta t}{2})$
$f_4 = f(x_k + \Delta t f_3, t_k+\Delta t)$



