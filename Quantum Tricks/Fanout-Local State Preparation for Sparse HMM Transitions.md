# Fanout-Local State Preparation for Sparse HMM Transitions

> **Source:** Grice and Meyer, arXiv:1405.7479
> **Tags:** #trick #state-preparation #sparse-structure #hidden-markov-models #controlled-unitaries

## What it does

Exploits small HMM fanout by preparing only the locally supported next-state superposition, even when the hidden-state alphabet is large.

## The trick

For each current state $i$ and emission $y$, define a transition state
$$
|\psi_{i,y}\rangle=\sum_{j\in \Gamma(i,y)} a_{i,j,y}|j\rangle,
$$
where $\Gamma(i,y)$ is the allowed next-state set and
$$
|\Gamma(i,y)|\le F\ll |Q|.
$$
Instead of preparing a dense state over all $Q$ possible next states, compile a unitary
$$
U_{|\psi_{i,y}\rangle}|0\rangle=|\psi_{i,y}\rangle
$$
using only the $F$-supported subspace.

Grice--Meyer give a canonical construction from two-level rotations. A product
$$
R_y(\theta_1)_{\{|0\rangle,|1\rangle\}}
R_y(\theta_2)_{\{|0\rangle,|2\rangle\}}
\cdots
R_y(\theta_K)_{\{|0\rangle,|K\rangle\}}
$$
sets the first column of the unitary to a desired real vector in spherical coordinates, for $K\le F$. Gray-code subspace changes and generalized Toffoli gates route those rotations to the desired basis states.

## When to reach for it

- Sparse Markov chains or HMMs where $|Q|$ is large but each state has few successors.
- Quantum trellis algorithms where transition locality is the source of any gain.
- Controlled state-preparation blocks inside larger dynamic-programming-style circuits.

## Complexity

The canonical construction costs
$$
O(F(\log F)^2)
$$
gates per transition-preparation unitary in the Grice--Meyer accounting. A direct block-diagonal transition layer over all current states therefore costs
$$
O(|Q|F(\log F)^2)
$$
gates.

## Caveat

The $|Q|$ factor is only hidden in time if the machine can run the controlled transition blocks in parallel. Also, the canonical two-level-rotation construction is a baseline; for structured transition graphs one should compile a custom preparation circuit.

## Related notes

- [[A Quantum Algorithm for Viterbi Decoding of Classical Convolutional Codes (Grice-Meyer 2015) — Paper Notes]]
- [[Trellis Path Superposition for Hidden Markov Models]]
- [[Phase-Weighted Viterbi Trellis Amplification]]
