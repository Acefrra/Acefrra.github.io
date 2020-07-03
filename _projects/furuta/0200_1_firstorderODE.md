---
    project: furuta
    type: subsect
    section: Mathematical Modeling
    title: Reducing to first order ODE
---

The gain controller K is gonna be built over a LTI system which is of the form:


$$ \begin{cases} \dot x(t) = A x(t) + B u(t) \\ y(t) = C x(t) + D u(t)\end{cases}
$$

Where x(t) is the state, u(t) is the input and y(t) is the output. $$ x(t) \in \R^{n}$$, $$u(t) \in \R^{m} $$ and $$y(t) \in \R^{p}$$. $$A $$is the matrix of the dynamic, B of the input, C of the output. $$ A \in \R^{n \times n} $$ , $$ B \in \R^{n \times m} $$ , $$ C \in \R^{p \times n} $$ , $$ D \in \R^{p \times m}$$.

The state equations is a system of linear differential equation of first order. However, the equations of motion we got are second order and they are not linear. To reduce them, we can add the other two differential equations of first order:

$$ \begin{cases} \dot \alpha(t) = \alpha_{dot}(t) \\ \dot \theta(t) = \theta_{dot}(t) \end{cases} $$

In this way we easily get rid of $$ \ddot \alpha(t) $$ and $$ \ddot \theta(t) $$, which are going to be expressed as first derivatives respectively of $$\alpha_{dot}$$ and $$\theta_{dot}$$.

In order to do that in Maple we first define the new equations.

<code>
add_diff := [thetadot = diff(theta(t),t), alphadot = diff(alpha(t),t)];
</code>

In this case we defined a <code> list </code> of the two equations (in Maple, lists are data structures surrounded with square brackets).
To substitute these equations to the original equation we do <code> subs(add_diff, eq1);</code> for equation 1 and <code>subs(add_diff, eq2)</code> for equation 2. To union them a user defined procedure <code> merge(list1, list2) </code> has been written. As a result to obtain the whole set of equations we do:

<code>
eqs := merge(eq1, eq2):
</code>

<code>
eqs_first_order := merge(subs(add_diff, eqs), add_diff):
</code>