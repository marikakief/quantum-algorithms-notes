# Phase Estimation on the Grover Operator for Counting

> **Source:** Boyer, Brassard, Høyer & Tapp, arXiv:quant-ph/9605034 (sketch); Brassard, Høyer & Tapp, arXiv:quant-ph/9805082 (full)
> **Tags:** #trick #counting #phase-estimation #grover #amplitude-estimation

## What it does

Estimates the number of solutions $t$ to a search problem using $O(\sqrt{N})$ oracle queries, without finding any specific solution.

## The trick

The Grover operator $G$ restricted to the 2D subspace $\text{span}\{|A\rangle, |B\rangle\}$ (good and bad states) has eigenvalues $e^{\pm 2i\theta}$ where $\sin^2\theta = t/N$.

Apply [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]] to $G$ acting on $|\Psi_0\rangle$ (the uniform superposition):

1. Prepare $\sum_{j=0}^{P-1} |j\rangle |\Psi_0\rangle / \sqrt{P}$.
2. Apply controlled-$G^j$, creating $\sum_j |j\rangle G^j|\Psi_0\rangle / \sqrt{P}$.
3. Apply QFT to the first register.
4. Measure to obtain a frequency $f \approx P\theta/\pi$.
5. Compute $t \approx N\sin^2(\pi f/P)$.

The state $|\Psi_0\rangle$ is an equal superposition of the two eigenstates of $G$ in the relevant subspace, so phase estimation returns one of the two eigenphases $\pm 2\theta$ with equal probability. Either gives $\theta$ and hence $t$.

## When to reach for it

- Estimating the fraction of items satisfying a predicate (approximate counting).
- Monte Carlo mean estimation — a later amplitude-estimation application of the same spectral idea.
- As a subroutine in algorithms that need to estimate success probabilities.
- Anytime you want to *count* rather than *find* solutions.

## Complexity

With phase-register size/controlled-iterate range $P$, BHT/BHMT counting gives an estimate obeying

$$
|t-\tilde t| < \frac{2\pi}{P}\sqrt{tN} + \frac{\pi^2}{P^2}N
$$

with constant success probability. Additive error $\epsilon$ in $a=t/N$ costs $O(1/\epsilon)$ Grover-operator uses. Relative error $\epsilon t$ for $t>0$ costs $O(\sqrt{N/t}/\epsilon)$ via the relative-counting procedure.

## Caveat

Requires controlled applications of the Grover operator, which doubles the oracle cost per step. Near $t=0$, resolving whether a marked item exists is hard. Near $t=N$, the derivative of $N\sin^2\theta$ also vanishes, but one can count the complement if the oracle can be coherently inverted.

The full treatment appears in [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes|Brassard-Høyer-Tapp (1998)]] and generalised as [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]] in [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard et al. (2002)]].

## Related notes
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]]
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes]]
- [[Amplitude Estimation via Phase Estimation on Q]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
