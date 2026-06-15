# Trellis Path Superposition for Hidden Markov Models

> **Source:** Grice and Meyer, arXiv:1405.7479
> **Tags:** #trick #state-preparation #hidden-markov-models #trellis #quantum-algorithms

## What it does

Builds a coherent superposition over all legal paths through a sparse HMM trellis without enumerating illegal paths.

## The trick

Represent a length-$N$ hidden path
$$
\pi=\pi_1\pi_2\cdots\pi_N
$$
as the computational-basis state
$$
|\pi_1\pi_2\cdots\pi_N\rangle.
$$
For each observed emission $y_n$, append a fresh $|0\rangle$ register and apply a block-diagonal controlled transition unitary
$$
V_{y_n}=\sum_{i\in Q}|i\rangle\langle i|\otimes U_{|\psi_{i,y_n}\rangle},
$$
where
$$
U_{|\psi_{i,y_n}\rangle}|0\rangle=|\psi_{i,y_n}\rangle
$$
and $|\psi_{i,y_n}\rangle$ is supported only on allowed next states. Chaining the $V_{y_n}$ blocks creates
$$
\sum_{\pi\in F_N} \alpha_\pi |\pi\rangle,
$$
where $F_N$ is the legal-path set. The construction mirrors dynamic programming, but keeps all partial paths coherent.

## When to reach for it

- HMM or trellis problems where legal paths are sparse relative to $Q^N$.
- Convolutional-code decoding, speech-recognition lattices, or other finite-state models with bounded branching.
- Quantum dynamic-programming attempts where the first task is to restrict support to legal histories.

## Complexity

If every transition state has fanout at most $F$, and the transition-preparation unitary costs $C_F$, then the path builder costs
$$
O(N|Q|C_F)
$$
gates in the direct block-controlled implementation. In Grice--Meyer, a canonical two-level-rotation construction gives
$$
C_F=O(F(\log F)^2).
$$

## Caveat

This prepares a coherent legal-path lattice; it does not by itself solve maximum-likelihood decoding. You still need a way to turn path likelihoods into measurement bias, for example [[Phase-Weighted Viterbi Trellis Amplification]].

## Related notes

- [[A Quantum Algorithm for Viterbi Decoding of Classical Convolutional Codes (Grice-Meyer 2015) — Paper Notes]]
- [[Fanout-Local State Preparation for Sparse HMM Transitions]]
- [[Phase-Weighted Viterbi Trellis Amplification]]
