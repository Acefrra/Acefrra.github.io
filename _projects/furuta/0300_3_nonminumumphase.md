---
    project: furuta
    type: subsect
    section: Controller Design
    title: Non Minimum Phase Systems
---

If the transfer function of the furuta pendulum of the furuta pendulum we can immediately see something is going on.

G(s)

This transfer function has the poles as placed, but it also has two zeros at z_{12} = {\displaystyle \pm } 5.943. A system with RH-plane zeros will show a particular behaviour and this kind of system are called **non-minimum phase systems**. The step response of such systems is peculiar because the system will start moving in the opposite direction with respect to the reference. If we infact take the step response of outr system we will get the following.

{% include_relative images/System_Response.html %}

Even though we don't like this behaviour it is stricly necessary from a physic point of view. The rotating arm has first to move clockwise, in  the pendulum tilts a little bit, it reach to the desired direction by compensating the falling pendulum. Only in this way it possible to keep the pendulum in equilibrium while moving to a particular position. 




