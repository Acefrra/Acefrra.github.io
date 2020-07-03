---
    project: furuta
    type: subsect
    section: Connection
    title: Time and Accuracy
---

The accuracy of a simulation is basically the accuracy at solving numerically the non linear system of ODEs of the system with respect to its analytical theoretical solution.
In general for a system of ODEs the smaller the step the better the accuracy.

In fact the first thing that has been implemented was the controller within the Coppelia environment, to verify that the system could simulate the controller and the system itself. For the pendulum simulation, the accuracy was good enough for a maximum step of 0.025 s. For higher steps it would have led to poor and not accurate simulation.

However for mechanical systems we may also want to see how the system evolves in real time, just for a more "practical" view.

In that case a smaller step time may affect real time rendering. Infact if the advancing and rendering time is comparable to the step chosen for the simulation, real time won't be guaranteed.

Moreover when introducing a remote interface to control the simulator, there are other negative effects adding up.

We want the system run in a synchronous mode, that is with Coppelia told when advance in the resolution of the ODEs.

However with this solution Coppelia will have to follow Matlab velocity. For each step Matlab takes about 25ms to compute the control. As a result when the simulation time step is under 25ms the Coppelia Simulation will be slower than expected.

Another side effect is that for higher step time the control is likely to fail. The speculation made is that this happens because Matlab detects the state before the system can actually evolve, and this lead to compunting the control signal for a wrong state of the system. For small step time this error is negligible, but for higher step the state evolves more and so the error is greater.

To correct this error, after sending the trigger signal, Matlab will wait for a wait_time before computing the state. This led to a significant improvement in the accuracy of the control, even though it slew down the simulation.