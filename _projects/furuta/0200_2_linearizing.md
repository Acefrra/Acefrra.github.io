---
    project: furuta
    type: subsect
    section: Mathematical Modeling
    title: Linearized Model
---

At this point it is sufficient to linearize the model to start designing the linear controller K. By linearing the model, we are able to approximate and replace non-linear components ($$ cos, sin, ^{2}, ^{3}, etc..$$) with linear ones. This approximation is made about an operating point, and it is valid around it. As long as the error is small enough it is possible to consider the linear version over the non-linear.

The goal is to keep the pendulum upward. According to the model, this configuration is associated with the state:

$$\theta = \theta_{0}, \alpha = 0, \theta_{dot} = 0, \alpha_{dot} = 0 $$.

Which is our operating point.

To linearize a function $$f: \R -> \R$$ about a point $$x_{0} $$, the Taylor approximation of first order is:
$$
f(x) = f(x_{0}) + \frac{d(f)}{d x}(x_{0})(x-x_{0})
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

In Maple I defined the procedure linearize(eqs, lin_point), which takes as arguments eqs and lin_point, and compute the linearization.

