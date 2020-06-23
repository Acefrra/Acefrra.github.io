---
    layout: documentation

    project: furuta
    title: Mathematical Modeling
    type: sect
---

The model of the system is from Quansar. To model the system they used a lagrangian approach. To do that it is necessary to describe the configuration of the sytem with the lagrangian coordinates $$[q_{1}, q_{2}, ...... q_{n}]$$ with $$ n = d.o.f $$.

{% include_relative images/model.html %}

They decided to describe the system with the angles $$[\theta(t) , \alpha(t)]$$, with the angles positive for CCW rotations of the pendulum. The angle alpha is measured from the positive $$z$$ semi-axis.

Then they wrote down the lagrangian equation:

$$ \frac{\delta L}{\delta t \delta q_{i}} - \frac{\delta L}{\delta q_{i}} = Q_{i}$$

Where L = T - V is the lagrangian function, with T kinetic energy and V is the potential energy of the whole system whilst $$Q_{i}$$ are the generalized non-conservative forces acting on the entire system with respect to the coordinate $$q_{i}$$.

On the rotary arm the torque $$\tau$$ of the motor is acting, as well a friction viscous force with coefficent $$B_{r}$$. For the pendulum we have the gravity and the viscous force with coefficent $$B_{p}$$.

For the rotary arm:

* it's massless, with inertia $$ J_{r} $$;
* length $$ L_{r} $$;

For the pendulum:

* inertia $$ J_{p} $$;
* it has a lumped mass $$m_{p}$$ positioned at $$ \frac{L_{p}}{2} $$;
* length $$ L_{p} $$;
 
We have 2 differential equations of motion:

$$ \frac{\delta L}{\delta t \delta \theta} - \frac{\delta L}{\delta \theta} = Q_{1}$$

$$ \frac{\delta L}{\delta t \delta \alpha} - \frac{\delta L}{\delta \alpha} = Q_{2}$$

with $$ Q_{1} = \tau - B_{r} \dot{\theta} $$ and $$Q_{2} = -B_{p}\dot{\alpha}$$, where $$B_{r}$$ and $$B_{p}$$ are the coefficent of viscous friction, and $$\tau$$ is the torque applied by the attached motor.


As a result the equations of motion are obtained:

<div style="overflow:auto;">
Equation 1:
$$(m_{p}L^{2}_{r} +\frac{1}{4}m_{p}L_{p}^{2} - \frac{1}{4}m_{p}L_{p}^{2}cos(\alpha)^{2} + J_{r})\ddot{\theta} - (\frac{1}{2}m_{p}L_{p}L_{r}cos(\alpha))\ddot{\alpha}+(\frac{1}{2}m_{p}L_{p}^{2}sin(\alpha)cos(\alpha))\dot{\theta}\dot{\alpha}+(\frac{1}{2}m_{p}L_{p}L_{r}sin(\alpha))\dot{\alpha}^{2} = \tau - B_{r}\dot{\theta}$$
</div>
<div style="overflow:auto;">
Equation 2:
$$ -\frac{1}{2}m_{p}L_{p}L_{r}cos(\alpha)\ddot{\theta}+(J_{p}+\frac{1}{4}m_{p}L_{p}^{2})\ddot{\alpha}-(\frac{1}{4}m_{p}L_{p}^{2}cos(\alpha)sin(\alpha)\dot{\theta}(t)\frac{1}{2}m_{p}L_{p}g sin(\alpha) = -B_{p}\dot{\alpha}$$
</div>

Moreover the motor used is a DC motor, with equation:

<div style="overflow:auto;">
Equation 3:
$$\tau = \frac{\eta_{g}k_{g}\eta_{m}k_{t}(V_{m}(t)-k_{g}k_{m}(\frac{d}{dt}\theta(t)))}{r_{m}} $$
</div>

Rotary Servo User Manual
* kt , motor current-torque
* km motor back-emf constant
* kg high geat total gear ratio
* etam motor efficiency
* etag gearbox efficency

Rotary Servo User Manual


By plugging it in equation 1, the input actually switches from the torque $$\tau$$ to the voltage $$\V_{m}$$
To define an equation, in Maple we simply do:

<code>
eq := equation;
</code>

Expressing the time for the variable, but omitting it for the parameters. For example to define equation1 we would write:

<code>
    eq := m__p*L^2__r + 1/4*m__p*L__p^2 - 1/4*m__p*L__p^2*cos(alpha)^2 + J__r*diff(theta,t,t) - 1/2*(m__p*L__p*L__r*cos(alpha))*diff(alpha(t),t,t)*diff(alpha,t,t) + (1/2*m__p*L__p^2*sin(alpha)*cos(alpha))*diff(theta,t)*diff(alpha,t)+(1/2*m__p*L__p*L__r*sin(alpha))*diff(alpha,t)^2 = tau - B__r*diff(theta(t),t)
</code>

In particular <code> __p </code> is used to write something at pedix, <code> diff(theta(t),t) </code>, compute the first derivative of theta(t) with respect to t (the function must be declared as function of t), <code> diff(theta(t),t,t) </code> is for the second derivative. And we use round brackets to define the order of operations. 







