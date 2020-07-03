---
    project: furuta
    type: subsect
    section: Controller Design
    title: Non Minimum Phase Systems
---

If we display the transfer function of the furuta pendulum (open loop) between Vm and  we can immediately see something is going on.

$$ G(s) = \frac{32.98 s^{2} -1166}{s^4+17.54s^{3}-62.27s^{2} -620.5s} $$
 
This transfer function has two zeros at $$s_{12} = {\displaystyle \pm }  5.943$$.

{% include_relative images/RootLocusOpenLoop.html %}

A system with RH-plane zeros will show a particular behaviour and this kind of system are called **non-minimum phase systems**.

After designing the input tracker in order to respect the requirements and to make the system stable we are going to have the following Root Locus:

{% include_relative images/RootLocusClosedLoop.html %}

Even though the system is stable it is not possible to cancel the positive zero and the behaviour of the system can be seen by its system response.

{% include_relative images/System_Response.html %}

This is a behaviour of non-minimium phase systems. The system will initially move in the opposite direction with respect to the set point.

Even though we do not like this behaviour it is stricly necessary from a physic point of view. To rotate CCW, the rotating arm has first to move clockwise. Only in this way it possible to keep the pendulum in equilibrium while moving to a particular position.

Even from a control point of view it is not possible to avoid this behaviour. In fact since this behaviour is caused by positive zeros the feedback controller cannot cancel them. In fact it would need to have positive poles that would let the controller unstable.