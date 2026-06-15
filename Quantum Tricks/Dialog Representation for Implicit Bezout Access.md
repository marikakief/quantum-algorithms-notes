# Dialog Representation for Implicit Bezout Access

> **Source:** Khattar, Shutty, Gidney, Zalcman, Yosri, Maslov, Babbush, Jordan, arXiv:2510.10967
> **Tags:** #trick #reversible-computing #Euclidean-algorithm #finite-fields #arithmetic

## What it does

Represents the output of an Extended Euclidean Algorithm run as a sequence of small transition matrices rather than explicit Bezout polynomials.

## The trick

Each EEA step maps a pair of polynomials by a $2\times 2$ transition matrix:

$$
\begin{pmatrix}
r_{i-1}\\
r_i
\end{pmatrix}
=
\tau_i
\begin{pmatrix}
r_{i-2}\\
r_{i-1}
\end{pmatrix}.
$$

The product $\tau_m\cdots \tau_1$ contains the Bezout coefficients. The **Dialog** stores the sequence of local transition data, such as coefficients and swap decisions from a division-free GCD algorithm, rather than multiplying out the final Bezout polynomials.

Because the representation is linear, operations that need the Bezout polynomial can often be rewritten as applying the transition sequence to a vector:

- Modular division: apply the forward transformation to $[0,C(z)]^T$.
- Modular multiplication: apply the inverse transformation to $[C(z),0]^T$.
- Evaluation of a Bezout polynomial at $\gamma$: evaluate each transition matrix at $\gamma$ and multiply the resulting field matrices.

The paper stores the shrinking remainder-polynomial data and the growing dialog in a shared register. As each EEA iteration frees one field element from the polynomial state, that field element's qubits are reused to store the next dialog symbol.

## When to reach for it

- You need EEA-derived data inside a reversible circuit but do not need all Bezout coefficients materialized at once.
- The downstream computation can replay transition matrices.
- Qubit count matters more than the inconvenience of sequential playback.

## Complexity

For two degree-$n$ polynomials over $\mathbb{F}_q$ with $b=\lceil\log_2 q\rceil$, the paper's implicit EEA has $2nb+O(\log^2 n)$ qubits and $2n^2$ dominant field multiplications. For modular division, the direct-division dialog method uses $4nb+O(\log^2 n)$ qubits and $4n^2$ dominant multiplications.

## Caveat

Implicit access can move cost downstream. In the paper's binary-extension-field OPI estimates, explicit Bezout access is cheaper overall because Chien search and Forney evaluation can then use mostly quantum-classical multiplication, which is Clifford-only over $\mathbb{F}_{2^b}$.

## Related notes

- [[Verifiable Quantum Advantage via Optimized DQI Circuits (Khattar, Shutty, Gidney et al 2025) — Paper Notes]]
- [[In-Register Quotient Storage for Reversible EEA]]
- [[Uncomputation via Syndrome Decoding]]
