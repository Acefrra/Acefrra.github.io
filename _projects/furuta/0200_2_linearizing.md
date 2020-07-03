---
    project: furuta
    type: subsect
    section: Mathematical Modeling
    title: Linearized Model
---

At this point it is sufficient to linearize the model to start designing the linear controller K. By linearing the model, we are able to approximate and replace non-linear components ($$ cos, sin, ^{2}, ^{3}, etc..$$) with linear ones. This approximation is made about an operating point, and it is valid around it. As long as the error is small enough it is possible to consider the linear version over the non-linear.

The goal is to keep the pendulum upward. According to the model, this configuration is associated with the state:

$$\theta = \theta_{0}, \alpha = 0\text{ rad. ,} \theta_{dot} = 0\frac{rad.}{s}, \alpha_{dot} = 0\frac{rad.}{s} $$.

Which is our operating point.

To linearize a function $$f: \R -> \R$$ about a point $$x_{0} $$, the Taylor approximation of first order is:

$$
f(x) \approx f(x_{0}) + \frac{d(f)}{d x}(x_{0})(x-x_{0})
$$

If the function is $$F: \R^{n} -> \R^{n}$$ we replace the derivative with the the Jacobian and the product with the matrix product. The jacobian is given by:

$$
J(F)(x) = \begin{pmatrix}
   \frac{\delta f_{1}}{\delta x_{1}} & \frac{\delta f1}{\delta x_{2}} & .. & \frac{\delta f1}{\delta x{n}}\\
   . & . & . & . \\
   . & . & . & . \\
    \frac{\delta f_{n}}{\delta x_{1}} & \frac{\delta f_{n}}{\delta x_{2}} & .. & \frac{\delta f_{n}}{\delta x_{n}} \\
\end{pmatrix}
$$

while the  difference x - x0 is the vector:

$$
    x - x_{0} = \begin{pmatrix}
    x_{1} - x_{10}\\
    x_{2} - x_{20}\\
    . \\
    . \\
    x_{n} - x_{n0}\\
    \end{pmatrix} 
$$

Which leads to the extended Taylor formula:

$$
F(x) \approx F(\begin{pmatrix}
    x_{10}\\
    x_{20}\\
    . \\
    . \\
    x_{n0}\\
    \end{pmatrix}) + J(F)(\begin{pmatrix}
    x_{10}\\
    x_{20}\\
    . \\
    . \\
    x_{n0}\\
    \end{pmatrix}) \cdot (\begin{pmatrix}
    x_{1} - x_{10}\\
    x_{2} - x_{20}\\
    . \\
    . \\
    x_{n} - x_{n0}\\
    \end{pmatrix})
$$

In Maple the procedure <code>linearize(eqs, lin_point)</code> has been defined which takes as arguments <code>eqs </code>and <code>lin_point</code>, and compute the simbolic linearization.

All in all, the code will be the following:

<code>
    lin_point := [theta(t) = 3, alpha(t) = 0, alpha__dot(t) = 0, theta__dot(t) = 0];
    <br><br>
    eqs_space_state := linearize(eqs_first_order, lin_point);
    <br><br>
</code>
After the model has been linearized, the matrix A, B can be extracted. A multiplies the state x, while B the input $$\tau$$.

The state of the system is:

$$
x = \begin{pmatrix}
\theta \\
\alpha \\
\theta_{dot}\\
\alpha_{dot}\\
\end{pmatrix}
$$

So basically, we are going to collect the matrix A and B from the linear equation. The code is:

<code>
    var := [theta(t), alpha(t), theta__dot(t), alpha__dot(t)];
    <br><br>
    vardot := diff(var, t);
    <br><br>
    eqs_state_space2 := solve(eqs_st, vardot); #It is necessary to solve the equation with respect to the derivative, in order to isolate them.
    <br><br>
    A, RES := GenerateMatrix(map(rhs, eqs_state_space2[1]), var)
    #[1] is just to get rid of the outer square brackets.
    <br><br>
    B, RES := GenerateMatrix(map(rhs, eqs_state_space2[1]), [V__m])
</code>


