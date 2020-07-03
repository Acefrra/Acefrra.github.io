---
    project: furuta
    type: subsect
    section: Controller Design
    title: Input Tracker
---

By applying the feedback K it is possible to stabilize the system, but it would be nice if we can move the rotating arm wherever we want and still keep the equilibrium. In order to do so we can add a new input $$u(t)$$. It will not be the voltage $$ V_{m} $$ but a reference $$ \theta_{d}$$. Picking the output $$\theta(t)$$, a tranfer function G(s) between $$\theta_{d}(t)$$ and $$\theta(t)$$ is defined with a certain gain $$DC_{gain}$$. By setting this $$DC_{gain}$$ to be 1 we built a system that will follow the reference $$\theta_{d}(t)$$. The scheme of such system is the following.

{% include_relative images/closed_loop.html %}

To compute the gain, first the transfer function from $$\theta_{d}(t)$$ to $$\theta(t)$$ has been computed, the system is  (A-BK, B, C, D). K is the controller gain.

<code>
[num,den]=ss2tf(A-B*K,B,C,D); %To get the tranfer function
<br><br>
TF = tf(num,den);
</code>

Then we compute the DC gain and the correction factor N:

<code>
DC = dcgain(TF);
<br><br>
N = 1/DC;
</code>

The new transfer function will be exactly TF just scaled by the factor N



