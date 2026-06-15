# Terminal Padding as an Initial-Condition Term

> **Source:** Simon, Berry, and Somma, arXiv:2605.16195
> **Tags:** #trick #history-state #matrix-equations #differential-equations

## What it does

Uses padded final-time clock sectors to include the initial-condition contribution in the same overlap identity as an inhomogeneous integral.

## The trick

For a matrix ODE,

$$
X(t)=\int_0^t ds\,e^{(t-s)A^\dagger}Ce^{(t-s)B}+e^{tA^\dagger}De^{tB}.
$$

The integral term fits a clock sum over evolving states. The initial-condition term has no integral. Simon--Berry--Somma handle this by appending $R$ terminal clock states where both history states hold the final propagators $e^{tA}|\phi\rangle$ and $e^{tB}|\psi\rangle$, then putting $D/R$ on those clock blocks:

$$
I_{\mathrm{terminal}}
=
\sum_{m=M}^{M+R-1}|m\rangle\!\langle m|\otimes \frac{D}{R}.
$$

The terminal-sector contribution to the overlap is

$$
\sum_{m=M}^{M+R-1}
\langle\phi|e^{tA^\dagger}\frac{D}{R}e^{tB}|\psi\rangle
=
\langle\phi|e^{tA^\dagger}De^{tB}|\psi\rangle.
$$

This is related to [[History-State Padding for Final-Time Readout]], but the purpose is different: the padding is not mainly to boost measurement probability; it turns a boundary term into a clock-sum term.

## When to reach for it

Use it when a history-state overlap naturally captures a time integral, but the solution also has a boundary or initial-condition term that should be estimated coherently with the same circuit.

## Complexity

The paper chooses

$$
R\sim \mu d/c
$$

so that the block-encoding constants for the short-integral blocks and the $D/R$ terminal blocks are balanced. Other choices of $R$ may be better for a specific problem.

## Caveat

Padding increases the clock size and affects normalization. If $d/c$ is large, the terminal sector is not free; it is a real part of the algorithmic cost.

## Related notes

- [[Efficient Quantum Algorithm for Linear Matrix Differential Equations and Applications to Open Quantum Systems (Simon-Berry-Somma 2026) — Paper Notes]]
- [[Two-Sided History-State Overlap for Matrix ODE Entries]]
- [[Clock-Diagonal Short-Integral Block Encoding]]
- [[History-State Padding for Final-Time Readout]]
