---
tags: 
    - calculus
    - gravity
    - projectile
---
# What is the required angle to reach the maximum distance when a projectile is fired from a given height?


- Initial speed: $V$
- Angle of $\theta$ (Variable)
- Initial height: $h$
- Vertical acceleration: $a_y=-32$
- Horizontal acceleration: $a_x=0$
- Initial vertical speed: $V_y^{(0)}=V\sin(\theta)$
- Initial horizontal speed: $V_x^{(0)}=V\cos(\theta)$

**Vertical speed at time $t$**

Integrate the acceleration $a_y=-32$ w.r.t. $t$ 
$$\begin{equation}
\int{a_ydt}=V_y^{(t)} = -32t + C
\end{equation}
$$
At $t=0$, the vertical speed is $V_y^{(0)}=V\sin(\theta)$, thus
$$V_y^{(t)} = -32t + V\sin(\theta)$$

**Horizontal speed at time $t$**

Integrate the acceleration $a_x=0$ w.r.t. $t$ 
$$\int{a_xdt}=V_x^{(t)} = C$$
At $t=0$, the horizontal speed is $V_x^{(0)}=V\cos(\theta)$, thus
$$V_x^{(t)} = V\cos(\theta)$$

**Vertical distance at time $t$**

Integrate the vertical speed $V_y=-32t+V\sin(\theta)$ w.r.t. $t$ 
$$\int{V_y^{(t)}} = s_y^{(t)} = -16t^2 + V\sin(\theta)t + C$$
At $t=0$, the height is $s_y^{(0)}=h$, thus
$$s_y^{(t)} = -16t^2 + Vsin(\theta)t + h$$

**Horizontal distance at time $t$**

Integrate the horizontal speed $V_x=V\cos(\theta)$ w.r.t. $t$ 
$$\int{V_x^{(t)}} = s_x^{(t)} = V\sin(\theta)t + C$$
At $t=0$, the horizontal distance is $s_x^{(0)}=0$, thus
$$s_x^{(t)} = V\sin(\theta)t$$

The projectile is airborne from the moment the projectile is fired until it reaches the ground $s_y=0$

$$
0 = -16t^2 + V\sin(\theta)t + h 
\iff t = \frac{-V\sin(\theta) \pm \sqrt{(V\sin(\theta))^2-4(-16)(h)}}{2(-16)} = \frac{V\sin(\theta) \pm \sqrt{(V\sin(\theta))^2+64h}}{32}
$$

We can drop the minus option as it would give a negative time. It will take $t^*$ to reach the ground. This depends on the angle $\theta$, the initial speed and the starting height. In this problem, the initial speed is set and we want to change the angle to reach maximum distance.
$$
t^* = \frac{V\sin(\theta) + \sqrt{(V\sin(\theta))^2+64h}}{32}
$$

The horizontal distance reached during that time is

$$
s_x^{(t^*)} = V\cos(\theta)t^* = V\cos(\theta)\frac{V\sin(\theta) + \sqrt{(V\sin(\theta))^2+64h}}{32}
$$

The maximal distance is reached when $\frac{d}{d\theta}s_x^{(t^*)}=0$. It's basically a balancing act between the horizontal speed and the time the projectile is in the air (or the vertical speed).

$$
\frac{d}{d\theta}s_x^{(t^*)} = \frac{d}{d\theta} \frac{V^2\sin(\theta)\cos(\theta)}{32} + \frac{d}{d\theta}\frac{V\cos(\theta) \sqrt{\left(V\sin(\theta)\right)^2+64h}}{32}
$$

**First term**

Using the [[Trigonometric identity#Double Angle|double angle]] identity $\cos^2(\theta) - \sin^2(\theta) = \cos(2\theta)$ (easier to carry around)

$$\textcolor{orange}{\frac{d}{d\theta}}\frac{V^2\textcolor{orange}{\sin(\theta)\cos(\theta)}}{32}=\frac{V^2\textcolor{orange}{\left(\cos^2(\theta) - \sin^2(\theta)\right)}}{32} = \frac{V^2\textcolor{orange}{\cos(2\theta)}}{32}$$

**Second term**

$$\frac{d}{d\theta}\frac{V\cos(\theta) \sqrt{(V\sin(\theta))^2+64h}}{32}=\frac{1}{32}\left[\sqrt{(V\sin(\theta))^2+64h}\frac{d}{d\theta}V\cos(\theta) + V\cos(\theta)\frac{d}{d\theta}\sqrt{(V\sin(\theta))^2+64h}\right]$$

$$
=\frac{1}{32}\left[-V\sin(\theta)\sqrt{(V\sin(\theta))^2+64h} + \frac{1}{2}V\cos(\theta)\frac{1}{\sqrt{(V\sin(\theta))^2+64h}}\frac{d}{d\theta}\left[(V\sin(\theta))^2+64(h)\right]\right]
$$

$$
=\frac{1}{32}\left[-V\sin(\theta)\sqrt{(V\sin(\theta))^2+64h} + \frac{1}{2}V\cos(\theta)\frac{2V^2\sin(\theta)\cos(\theta)}{\sqrt{(V\sin(\theta))^2+64h}}\right]
$$

$$
=\frac{1}{32}\left[-V\sin(\theta)\sqrt{(V\sin(\theta))^2+64h} + \frac{V^3\sin(\theta)\cos^2(\theta)}{\sqrt{(V\sin(\theta))^2+64h}}\right]
$$

$$
=\frac{1}{32}\left[ \frac{-V\sin(\theta)\left((V\sin(\theta))^2+64h\right) + V^3\sin(\theta)\cos^2(\theta)}{\sqrt{(V\sin(\theta))^2+64h}}\right]
$$
$$
= \frac{-V^3\sin^3(\theta)-64hV\sin(\theta) + V^3\sin(\theta)\cos^2(\theta)}{32\sqrt{(V\sin(\theta))^2+64h}}
$$

**Together**

$$
\frac{d}{d\theta}s_x^{(t^*)} =\frac{V^2\cos(2\theta)}{32} + \frac{-V^3\sin^3(\theta)-64hV\sin(\theta) + V^3\sin(\theta)\cos^2(\theta)}{32\sqrt{(V\sin(\theta))^2+64h}}=0
$$

$$
\iff \frac{V^2\cos(2\theta)}{32} = \frac{V^3\sin^3(\theta)+64hV\sin(\theta) - V^3\sin(\theta)\cos^2(\theta)}{32\sqrt{(V\sin(\theta))^2+64h}}
$$

$$
\iff V\cos(2\theta) = \frac{V^2\sin^3(\theta)+64h\sin(\theta) - V^2\sin(\theta)\cos^2(\theta)}{\sqrt{(V\sin(\theta))^2+64h}}
$$

$$
\iff V\cos(2\theta) = \frac{V^2\sin(\theta)\left(\sin^2(\theta) - \cos^2(\theta)\right)+64h\sin(\theta)}{\sqrt{(V\sin(\theta))^2+64h}}
$$

$$
\iff V\cos(2\theta) = \frac{-V^2\sin(\theta)\cos(2\theta)+64h\sin(\theta)}{\sqrt{(V\sin(\theta))^2+64h}}
$$

$$
\iff \frac{V\cos(2\theta)}{\sin(\theta)} = \frac{-V^2\cos(2\theta)+64h}{\sqrt{(V\sin(\theta))^2+64h}}
$$

$$
\iff \left(\frac{Vcos(2\theta)}{sin(\theta)}\right)^2 = \left(\frac{-V^2cos(2\theta)+64h}{\sqrt{(Vsin(\theta))^2+64h}}\right)^2
$$

$$
\iff \frac{V^2cos^2(2\theta)}{sin^2(\theta)} = \frac{\left(-V^2cos(2\theta)+64h\right)^2}{(Vsin(\theta))^2+64h}
$$

$$
\iff \frac{V^2cos^2(2\theta)}{sin^2(\theta)} = \frac{V^4cos(2\theta)-128hV^2cos(2\theta) + (64h)^2}{(Vsin(\theta))^2+64h}
$$

$$
\iff \frac{V^2cos^2(2\theta)(V^2sin^2(\theta)+64h)*}{sin^2(\theta)} = V^4cos(2\theta)-128hV^2cos(2\theta) + (64h)^2
$$

$$
\iff \frac{V^4cos^2(2\theta)sin^2(\theta)+64hV^2cos^2(2\theta)}{sin^2(\theta)} = V^4cos(2\theta)-128hV^2cos(2\theta) + (64h)^2
$$

$$
\iff V^4cos^2(2\theta) + \frac{64hV^2cos^2(2\theta)}{sin^2(\theta)} = V^4cos(2\theta)-128hV^2cos(2\theta) + (64h)^2
$$

$$
\iff \frac{V^2cos^2(2\theta)}{sin^2(\theta)} = -2V^2cos(2\theta) + 64h
$$

$$
\iff \frac{V^2\left(cos^2(\theta)-sin^2(\theta)\right)^2}{sin^2(\theta)} = -2V^2\left(cos^2(\theta)-sin^2(\theta)\right) + 64h
$$

$$
\iff \frac{V^2\left(cos^4(\theta)-2cos^2(\theta)sin^2(\theta) + sin^4(\theta)\right)}{sin^2(\theta)} = -2V^2\left(cos^2(\theta)-sin^2(\theta)\right) + 64h
$$

$$
\iff \frac{V^2cos^4(\theta)-2V^2cos^2(\theta)sin^2(\theta) + V^2sin^4(\theta)}{sin^2(\theta)} = -2V^2cos^2(\theta)+2V^2sin^2(\theta) + 64h
$$
$$
\iff \frac{V^2cos^4(\theta)}{sin^2(\theta)} - 2V^2cos^2(\theta) + V^2sin^2(\theta) = -2V^2cos^2(\theta)+2V^2sin^2(\theta) + 64h
$$
$$
\iff \frac{V^2cos^4(\theta)}{sin^2(\theta)} = V^2sin^2(\theta) + 64h
$$
$$
\iff \frac{V^2cos^4(\theta)-V^2sin^4(\theta)}{sin^2(\theta)} = 64h
$$

$$
\iff \frac{V^2\left(cos^4(\theta)-sin^4(\theta)\right)}{sin^2(\theta)} = 64h
$$
$$
\iff \frac{V^2\left(cos^2(\theta)-sin^2(\theta)\right)\left(cos^2(\theta)+sin^2(\theta)\right)}{sin^2(\theta)} = 64h
$$
$$
\iff \frac{V^2\left(cos^2(\theta)-sin^2(\theta)\right)}{sin^2(\theta)} = 64h
$$

$$
\iff \frac{cos^2(\theta)}{sin^2(\theta)}-1 = \frac{64h}{V^2}
$$

$$
\iff \frac{cos^2(\theta)}{sin^2(\theta)} = \frac{64h+V^2}{V^2}
$$

$$
\iff \frac{sin^2(\theta)}{cos^2(\theta)}=\tan^2(\theta) = \frac{V^2}{64h+V^2}
$$

$$
\iff \tan(\theta) = \pm\sqrt{\frac{V^2}{64h+V^2}}
$$

$$
\iff \theta = \arctan\left(\pm\sqrt{\frac{V^2}{64h+V^2}}\right)
$$

Negative angle is not allowed, so $\theta = \arctan\left(\sqrt{\frac{V^2}{64h+V^2}}\right)$

We started working with gravity $g = -32 feet/sec^2$. To make the above solution more general

$$\theta = \arctan\left(\sqrt{\frac{V^2}{V^2-2gh}}\right)$$

Where $h$ and $V$ must use the same units as the gravity (feet or meters)

---

If we want to know one of: horizontal distance, the angle, the velocity or the time of flight then we can use this formula. 
$$
s_x^{(t)} = V\cos(\theta)t = V\cos(\theta)\frac{V\sin(\theta) + \sqrt{(V\sin(\theta))^2+64h}}{32}
$$

Suppose that a ball is thrown and you want to know the initial velocity, then you need to have the initial angle and the total distance travelled. The calculations are easier if we also have the time of flight. 

If $h=0$, the whole thing simplifies quite a bit with the [[Trigonometric identity#Products|product identity]] 

$$
\begin{align}
s_x^{(t)}&= V\cos(\theta)\frac{V\sin(\theta) + \sqrt{(V\sin(\theta))^2}}{32} \\
&= \frac{V^2\textcolor{orange}{\cos(\theta)\sin(\theta)}}{16} \\
&= \frac{V^2 \textcolor{orange}{\left(\sin(\theta+\theta)-\sin(\theta-\theta) \right)}}{\textcolor{orange}{2}(16)} \\
&= \frac{V^2 \sin(2\theta)}{32} \\
\iff V&= \sqrt{\frac{32s_x^{(t)}}{\sin(2\theta)}}
\end{align}
$$
Negative speed is not possible, so we only consider the positive case. 