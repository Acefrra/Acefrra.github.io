---
    layout: documentation

    project: furuta
    title: Controller Design
    type: sect
---

After getting the matrixes A and B. It is possible to design the state feedback controller. This control technique uses the whole state x of the system to get information about the process, and keep it from diverging by using its input channels. 

This technique requires to be able to exactly measure the state of the system. That means it is necessary to have a high number of sensors.

The new state space representation of the system will be:

$$ \begin{cases} \dot x(t) = (A - B K) x(t) \\ y(t) = C x(t)\end{cases}
$$

The matrix of the dynamic is A - BK. So it is possible to design the matrix K such that the system has desired poles.

In particular, given the original system:

$$
\Sigma = \begin{cases} \dot x(t) = A x(t) + B u(t) \\ y(t) = C x(t)\end{cases}
$$

is transformed to the controllable canonical form:

$$
\Sigma_{contr} = \begin{cases} \dot x(t) = A_{contr} x(t) + B_{contr} u(t) \\ y(t) = C x(t)\end{cases}
$$

With:

$$
A_{contr} = \begin{pmatrix}
0 & 1 & 0 & ... & 0 \\
. & 0 & 1 & ... & 0 \\
. & . & . & ... & 0 \\
0 & . & . & 0   & 1 \\
-\alpha_{0} & -\alpha_{1} & . & . & -\alpha_{n-1}
\end{pmatrix}
$$

$$
B_{contr} = \begin{pmatrix}
0 \\
. \\
. \\
1 \\
\end{pmatrix}
$$

$$ \alpha_{1}, \alpha_{2}, .... , \alpha_{n-1} $$ are the coefficents of the charateristic polynomial $$p_{A}(s) = s^{n} + \alpha_{n-1}s^{n-1} + ... \alpha_{1}s + \alpha_{0}$$ of the matrix $$A_{contr}$$.

If $$p_{desired} = (s-s_{1})(s-s{2})...(s-s_{n}) = s^{n} + \beta_{n-1}s^{n-1} + ... \beta_{1}s^{1} + \beta_{0} $$ Then the K matrix will be built to be $$ K = \begin{bmatrix}
(\beta_{0} - \alpha_{0}) & (\beta_{1} - \alpha_{1}) & ... & (\beta_{n-1} - \alpha_{n-1})
\end{bmatrix}
$$

If we apply such state feedback to $$\Sigma_{contr}$$, the matrix $$A_{contr}-B_{contr}K $$ will be:

$$
(A_{contr} - B_{contr} K) = \begin{pmatrix}
0 & 1 & 0 & ... & 0 \\
. & 0 & 1 & ... & 0 \\
. & . & . & ... & 0 \\
0 & . & . & 0   & 1 \\
-\beta_{0} & -\beta_{1} & . & . & -\beta_{n-1}
\end{pmatrix}
$$

If A is not in the controllable form, it can be proved that the transformation matrix T = R M brings to that form, althouh this require R is inveertible that is full rank, as already discussed. In this case the matrix K must be transformed back in the original coordinates.

In particular the matrix K will be given by:

$$ K = \begin{bmatrix}
(\beta_{0} - \alpha_{0}) & (\beta_{1} - \alpha_{1}) & ... & (\beta_{n-1} - \alpha_{n-1})
\end{bmatrix} M^{-1} R^{-1}
$$









