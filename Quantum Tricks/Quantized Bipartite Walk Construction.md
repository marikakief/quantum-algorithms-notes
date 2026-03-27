
> **Source:** Szegedy, FOCS 2004
> **Tags:** #trick #quantum-walk #markov-chain #fundamental

## What it does

Converts a classical bipartite walk (pair of stochastic matrices) into a quantum walk operator that is a product of two reflections. This is the template for quantum walk algorithms and the ancestor of [[Qubitization Iterate|qubitization]].

## The trick

Given a bipartite walk $(c, r)$ where $c: [n| \to |m]$ and $r: |m] \to [n|$ are stochastic matrices, define quantum states:

$$
|\sqrt{c_i}\rangle = \sum_j \sqrt{c[i,j]}\,|i\rangle|j\rangle, \qquad |\sqrt{r_j}\rangle = \sum_i \sqrt{r[j,i]}\,|i\rangle|j\rangle
$$

Build projections:

$$
C = \sum_{i=1}^n |\sqrt{c_i}\rangle\langle\sqrt{c_i}|, \qquad R = \sum_{j=1}^m |\sqrt{r_j}\rangle\langle\sqrt{r_j}|
$$

The walk operator is:

$$
\mu = (2R - I)(2C - I) = \mathrm{ref}_B \cdot \mathrm{ref}_A
$$

For a symmetric Markov chain $P$ on $[n]$, take $(c, r) = (P, P)$. The stationary state is $u = \sum_{i,j} \sqrt{P[i,j]/n}\,|i\rangle|j\rangle$ and satisfies $\mu u = u$.

## Why this structure matters

The product-of-two-reflections form is the same structure as [[Oblivious Amplitude Amplification (Robust)|amplitude amplification]], and later [[Qubitization Iterate|qubitization]] uses this exact construction with the projections reinterpreted as PREPARE and SELECT oracles for [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]]. The [[Discriminant Matrix Spectral Theorem]] gives the spectrum.

## When to reach for it

- Designing quantum walk algorithms for search/detection problems on graphs
- Building [[Qubitization Iterate|qubitization]] walk operators from Hamiltonian decompositions
- Any setting where you have a Markov chain with known spectral gap and want a quantum speedup

## Complexity

- One step of $\mu$: 1 call to $2C-I$ + 1 call to $2R-I$
- Each reflection costs as much as preparing $|\sqrt{c_i}\rangle$ or $|\sqrt{r_j}\rangle$ — typically one oracle query + controlled rotations

## Caveat

Requires efficient preparation of $|\sqrt{c_i}\rangle$ and $|\sqrt{r_j}\rangle$ — i.e., you need coherent access to the transition probabilities of the chain. For Markov chains given only by a sampling oracle, this preparation step dominates the cost.

## Related notes

- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
- [[Discriminant Matrix Spectral Theorem]]
- [[Qubitization Iterate]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[Oblivious Amplitude Amplification (Robust)]]
- [[Standard-Form Encoding (Prepare + Signal Oracle)]]
