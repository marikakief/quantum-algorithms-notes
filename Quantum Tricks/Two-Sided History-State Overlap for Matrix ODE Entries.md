# Two-Sided History-State Overlap for Matrix ODE Entries

> **Source:** Simon, Berry, and Somma, arXiv:2605.16195
> **Tags:** #trick #history-state #matrix-equations #differential-equations #block-encoding

## What it does

Turns an entry of a matrix differential-equation solution into an overlap between two history states, one carrying the left propagator and one carrying the right propagator.

## The trick

For

$$
\dot X(t)=A^\dagger X(t)+X(t)B+C,
\qquad X(0)=D,
$$

the solution contains two-sided terms

$$
e^{(t-s)A^\dagger} C e^{(t-s)B}
\quad\text{and}\quad
 e^{tA^\dagger}D e^{tB}.
$$

Instead of vectorizing $X(t)$, prepare two history states

$$
|\phi_{\mathrm{hist}}\rangle
=\sum_m |m\rangle e^{tmA/M}|\phi\rangle + \text{terminal padding},
$$

$$
|\psi_{\mathrm{hist}}\rangle
=\sum_m |m\rangle e^{tmB/M}|\psi\rangle + \text{terminal padding}.
$$

Then build a clock-diagonal operator $I$ whose early clock blocks contain the short-window integral $I_C$ and whose terminal blocks contain $D/R$. The matrix entry becomes

$$
\langle\phi|X(t)|\psi\rangle
=
\langle\phi_{\mathrm{hist}}|I|\psi_{\mathrm{hist}}\rangle.
$$

The left and right propagators stay separate. That is the point: no $N^2$-dimensional vectorized solution state is needed.

## When to reach for it

Use it when:

- the unknown is a matrix/operator rather than a vector;
- the dynamics has the form “left action + right action + source”; 
- the task is an entry, bilinear form, or local observable of the matrix solution;
- vectorization would create a normalized state with tiny useful amplitudes.

## Complexity

The overlap must be estimated to precision scaled by the history-state and block-encoding normalizations. In Simon--Berry--Somma, the LCHS route gives normalization $O(cL)$ and total query complexity

$$
\widetilde{O}\!\left(\frac{cL}{\epsilon}t\mu\right),
\qquad \mu=\max\{a,b\}.
$$

## Caveat

This estimates one matrix entry or generalized entry. If you need many entries, readout cost comes back. Also, the approach inherits the cost of preparing the two non-unitary history states.

## Related notes

- [[Efficient Quantum Algorithm for Linear Matrix Differential Equations and Applications to Open Quantum Systems (Simon-Berry-Somma 2026) — Paper Notes]]
- [[Quantum Linear Matrix Equations — Paper Notes]]
- [[Clock-Diagonal Short-Integral Block Encoding]]
- [[Terminal Padding as an Initial-Condition Term]]
- [[Sandwich LCU for Matrix Equations]]
- [[History-State Padding for Final-Time Readout]]
