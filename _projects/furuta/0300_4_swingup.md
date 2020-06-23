---
    project: furuta
    type: subsect
    section: Controller Design
    title: Swing-Up Controller
---

It has been possible to balance the pendulum at its upright position. However as we discussed the control will be effective only if the configuration is close to the upright due to the Taylor approximation.

However, it is possible to develop a controller that brings the pendulum to the equilibrium point. So that it would be possible to apply the balancing control. This controller is known as **Energy-Shaping Controller** and in this specific case it is called **Swing-Up Controller**, as it is able to swing up the pendulum from the downright position.

These type of controller is energy based. Infact it applies a control to increase the energy up until it reaches the target. In our case the target is the energy when the pendulum is upright. The energy of the pendulum is:

E = Ek + Ep = 

We want to reach the energy of the pendulum when it is still with small velocity, so when:

E = Ep (alpha = 0) = .... = 0

The energy based controller will apply a torque until the difference between the "reference" enrgy and the current enrgy is small.
To apply this technique it is necessary to express the energy as function of the input of the system, so it may not be straight forwardas it requires an understanding of the physical process.

In our 




