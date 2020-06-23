---
    project: furuta
    type: subsect
    section: Mathematical Modeling
    title: Parameters Choice
---

To choose the parameters kinematic or dynamic wise studies can be done, and chose them in order to optimize a certain cost function, or to stay within the actuators or encumbrance specifications.

However, since this project mainly focuses on the control part, some parameters has been picked from literature and the final set of parameters are the following:

Mechanical Parameters:

<div style="overflow:auto">
$$
\begin{pmatrix}
J_{p} = 0.0023 &
m_{p} = 0.125 &
L_{r} = 0.215 &
L_{p} = 0.335 &
J_{r} = 0.00023&
B_{p} = 0&
g = 9.81&
B_{r} = 0
\end{pmatrix}
$$
</div>

Elettromechanical Parameters:
<div style="overflow:auto">
$$
\begin{pmatrix}
\eta_{g} = 0.85&
\eta_{m} = 0.87&
k_{g} = 70&
k_{m} = 0.0076&
k_{t} = 0.0076&
r_{m} = 2.6&
\end{pmatrix}
$$
</div>

To define them in maple we would write:

<code>
data_mechanical := [J__p = 0.0023, m__p = 0.125, L__r = 0.215, L__p = 0.335, J__r = 0.0023, B__p = 0,g = 9.81,B__r = 0];
<br><br>
data_electrical := [eta__g = 0.85,eta__m = 0.87,k__g = 70,k__m = 0.0076,k__t = 0.0076,r__m = 2.6];
<br><br>
data := merge(data_mechanical, data_electrical);
</code>

