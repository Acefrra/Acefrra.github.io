---
    project: furuta
    type: subsect
    section: Controller Design
    title: Pole Placement
---

The placement of the pole will strictly depend on the performance we expect from the system.

The requirement decided were:

* Percentage Overshoot %OS = 10%;
* Settling Time = 3s;

This requirements refer to the step response of the system to the reference input $$ \theta_{d} $$, that is the step response of the transfer function TF. The requirements are defined for a second order system of the type:

$$ G(s) = \frac{\omega_{n}^{2}}{s^2+2\zeta\omega_{n}+\omega_{n}^{2}} $$

The relation between the overshoot and the angular frequency $$ \omega_{n} $$ is:

$$ \zeta = \frac{ln(\frac{OS}{100})}{\sqrt[2]{\pi^{2}+ln((\frac{OS}{100})^{2})}} $$

The relation between the settling time $$T_{s}$$ and $$\omega$$ and $$\zeta$$ is:

$$ \omega \approx \frac{4}{T_{s}\zeta} $$

which is valid if $$\zeta \ll 1$$.[3]

The poles of such sysem are $$p_{12} = -\sigma + j\omega_{d} $$where $$\sigma = \zeta \omega_{n} $$and $$\omega_{d}=\omega_{n}\sqrt[2]{(1-\zeta^{2})}$$. However our system going to be of fourth order. For this reason, the other two poles will be chosen in such a way they can be negleted, that is their real part must at least 10 times the real part of the complex poles, also known as **dominant poles**. 

In matlab we have the following code:

<code>
perc_OS = 12;   %12 Overshoot
<br>Ts = 3;       %1.5s Settling time
<br>disp("Desired Overshoot:");
<br>disp(perc_OS);
<br>disp("Desired Settling Time:");
<br>disp(Ts);
<br>zeta = (-(log(perc_OS/100))/sqrt(pi^2+(log(perc_OS/100))^2));
<br>wn = 4/(zeta*Ts);
<br>sigma = zeta * wn;
<br>wd = wn*(1-zeta^2)^0.5;
<br>p1 = -sigma+1i*wd;
<br>p2 = conj(p1);
<br>p3 = -sigma*10; % Desired pole location
<br>p4 = -sigma*11; % Desired pole location
</code>

In Matlab, given the matrix A and B, it is possible to compute the matrix K (already in the desired coordinates with the command):

<code>
K = acker(A, B, poles);
</code>

The matlab function  control_FURPEN has been defined:

<code>
function K = control_FURPEN(A,B,p1,p2,p3,p4)
<br>% Desired poles (-30 and -40 are given)
<br>poles = [p1, p2, p3, p4];
<br>% Find control gain using MATLAB pole-placement command (acker or place)
<br>K = acker(A, B, poles);
<br>
end
<br>
</code>




