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
- Monte Carlo mean estimation — the $\sqrt{N}$ advantage over classical sampling.
- As a subroutine in algorithms that need to estimate success probabilities.
- Anytime you want to *count* rather than *find* solutions.

## Complexity

$O(\sqrt{N}/\epsilon)$ for relative error $\epsilon$ in estimating $\sqrt{t/N}$. With $P$-qubit phase register, the estimate has additive error $O(\sqrt{t(N-t)}/P + N/P^2)$ in $t$.

## Caveat

Requires controlled applications of the Grover operator, which doubles the oracle cost per step. The precision in $t$ degrades when $t$ is close to $0$ or $N$ (the eigenphases become hard to resolve near $0$ and $\pi/2$).

The full treatment appears in [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes|Brassard-Høyer-Tapp (1998)]] and generalised as [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]] in [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard et al. (2002)]].

## Related notes
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]]
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes]]
- [[Amplitude Estimation via Phase Estimation on Q]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
