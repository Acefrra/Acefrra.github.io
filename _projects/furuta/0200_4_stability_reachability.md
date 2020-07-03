---
    project: furuta
    type: subsect
    section: Mathematical Modeling
    title: Stability and Reachabilty
---

Even though we got the A and B matrixes, they are still parametric. To get them numerical in Maple we do:

<code>
A_data := subs(data, A);
<br><br>
B_data := subs(data, B);
<br><br>
</code>

After this we got the matrixes:

$$
    A_{data} := \begin{pmatrix}
    0 & 0 & 1 & 0 \\
    0 & 0 & 0 & 1 \\
    0 & 34.69 & -17.54 & 0 \\
    0 & 62.26 & -13.59 & 0 \\
    \end{pmatrix}
$$

$$ 
    B_{data} := \begin{pmatrix}
    0 \\
    0 \\
    32.97 \\
    25.56
    \end{pmatrix}
$$

with maple is possible to compute the eigenvalues with:

<code>
    Eigenvalues(A_data);
</code>

and we get the four eigenvalues $$ \lambda_{1} = 0, \lambda_{2} = -19.10 , \lambda_{3} = 6.53, \lambda_{4} = -4.97$$. As it can be seen there is one eigenvalue with real part positive. It can be proved that this point is **not locally asymptotically stable**.

Neverthless, if we can stabilize this system and put all the eigenvalues inside the left complex plane for example by exerting a full state feedback, the system is going to **be locally asymptotically stable**.

In order to that it is necessary that the system is reachable that is the reachability matrix:

$$
R = \begin{pmatrix} A & A^{2}B &...& A^{n}B \end{pmatrix}
$$

is full rank. In our case, n = 4. With Maple this is code to verify that:

<code>
R := Concatenate(2, A_data, A_data.B_data, A_data^2.B_data, A_data^3.B_data];       #2 means to merge the matrixes along columns.
<br><br>
rank(R);
</code>

since it is 4, the system is **reachable**. Thus the system is **stabilizable**. As a result, it will be possible to find the matrix K.
