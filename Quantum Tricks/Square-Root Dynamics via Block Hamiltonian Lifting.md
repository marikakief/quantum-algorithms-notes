
> **Tags:** #trick #block-hamiltonian #oscillator-simulation
> **Source:** Babbush, Berry, Kothari, Somma, Wiebe, *Exponential quantum speedup in simulating coupled classical oscillators*, arXiv:2303.13012, PRX **13**, 041041 (2023)

## What it does
Transforms second-order dynamics $\ddot y=-Ay$ into first-order unitary dynamics by lifting with a block Hamiltonian built from $A=BB^\dagger$.

## The trick
Simulate $H=\begin{pmatrix}0&B\\B^\dagger&0\end{pmatrix}$, so evolution encodes both velocity-like and force/stretch-like components in separate subspaces.

## When to reach for it
- Harmonic/linear systems where $A\succeq 0$ and observables are quadratic/global

## Complexity
Enables use of standard sparse/[[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] simulation toolkits on otherwise classical second-order systems.

## Caveat
Requires efficient factorization/access to $B$ and good normalization control.

## Related Paper Notes
- [[Exponential Quantum Speedup for Coupled Classical Oscillators (Quantum 2020-04-20-254 follow-on) — Paper Notes]]
