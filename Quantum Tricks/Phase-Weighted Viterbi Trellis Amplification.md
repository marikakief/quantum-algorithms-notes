# Phase-Weighted Viterbi Trellis Amplification

> **Source:** Grice and Meyer, arXiv:1405.7479
> **Tags:** #trick #amplitude-amplification #viterbi #hidden-markov-models #optimization

## What it does

Turns a trellis path-likelihood score into relative phase, then uses a Grover-like legal-path reflection to increase the measurement probability of the best path.

## The trick

Start from a coherent legal-path state, for example from [[Trellis Path Superposition for Hidden Markov Models]],
$$
|F_N\rangle=\frac{1}{\sqrt{|F_N|}}\sum_{\pi\in F_N}|\pi\rangle.
$$
Apply a diagonal phase oracle
$$
G_\phi|\pi\rangle=e^{i\Phi(\pi)}|\pi\rangle,
$$
where $\Phi(\pi)$ is a monotone function of the path likelihood. For convolutional-code decoding, $\Phi(\pi)$ can be tied to the number of channel errors along the path.

Then apply a reflection about the legal-path superposition, schematically
$$
R_{F_N}=2|F_N\rangle\langle F_N|-I,
$$
and repeat $G_\phi R_{F_N}$. If the best path is assigned phase close to $-1$ and the lower-likelihood paths are closer to $1$, this behaves like [[Standard Amplitude Amplification]] with a noisy, graded marking oracle.

## When to reach for it

- Maximum-likelihood path selection when a full binary good/bad oracle is awkward but a monotone score oracle is easy.
- Dynamic-programming problems with a coherent legal-state generator and a natural phase score.
- Small-frame trellis problems where a few amplification rounds may already separate the best path.

## Complexity

If $L=|F_N|\approx F^N$, the worst-case number of rounds can be
$$
O(\sqrt L)=O(\sqrt{F^N}),
$$
matching the usual Grover scale. The per-round cost is the cost of the phase oracle plus the legal-path reflection. In Grice--Meyer, one QVA round is quoted as
$$
O(N|Q|F(\log F)^2)
$$
gates before parallelisation over $|Q|$.

## Caveat

The phase schedule matters. If the best and second-best paths have similar likelihoods, or if the phase parameter is poorly tuned, the amplification can give only a weak bias. This is a heuristic scoring-to-amplitude trick, not a general replacement for a threshold oracle.

## Related notes

- [[A Quantum Algorithm for Viterbi Decoding of Classical Convolutional Codes (Grice-Meyer 2015) — Paper Notes]]
- [[Trellis Path Superposition for Hidden Markov Models]]
- [[Standard Amplitude Amplification]]
- [[Grover Search with Evolving Threshold for Minimum Finding]]
