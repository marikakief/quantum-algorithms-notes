# Mimicking State via Matrix Multiplicative Weights

> **Source:** King, Gosset, Kothari, Babbush, arXiv:2404.19211
> **Tags:** #trick #shadow-tomography #multiplicative-weights #sign-recovery #Pauli-learning #two-copy

## What it does

Computes a classical description of a quantum state $\sigma$ that reproduces the signs of all large Pauli expectation values of an unknown state $\rho$. Combined with Bell sampling on $\rho \otimes \sigma$, this recovers the signs that [[Bell Difference Sampling for Magnitude Estimation|magnitude estimation]] alone cannot determine.

## The trick

After Bell sampling identifies the significant set $S_\epsilon$ (all Paulis $P$ with $|\mathrm{tr}(\rho P)| \geq \epsilon/2$), we need a **mimicking state** $\sigma$ satisfying $|\mathrm{tr}(\sigma P)| \geq \epsilon/4$ for all $P \in S_\epsilon$.

The matrix multiplicative weights (MMW) algorithm iteratively constructs $\sigma$:

1. Start with $\omega^{(0)} = I/2^n$ (maximally mixed state).
2. At each step $t$, search for a Pauli $P \in S_\epsilon$ where $\omega^{(t)}$ disagrees with the magnitude estimates: $|\mathrm{tr}(P\omega^{(t)}) - u_P| > \epsilon/2$ and $|\mathrm{tr}(P\omega^{(t)}) + u_P| > \epsilon/2$.
3. If found, use $O((\log T)/\epsilon^2)$ additional copies of $\rho$ to learn $\mathrm{sign}(\mathrm{tr}(P\rho))$.
4. Update: $\omega^{(t+1)} = \exp(-\beta \sum_{\tau} M^{(\tau)}) / \mathrm{tr}(\exp(\cdots))$ where $M^{(t)} = \mathrm{sign}(\mathrm{tr}(P\omega^{(t)}) - r_P u_P) \cdot P$ and $\beta = \sqrt{n/T}$.
5. Terminate after at most $T = \lceil 64n/\epsilon^2 \rceil + 1$ steps.

**Why it terminates:** The MMW potential function argument (Arora-Kale) shows that the total "disagreement" $\sum_t |y^{(t)} - \mathrm{tr}(P^{(t)}\rho)|$ is bounded by $2\sqrt{nT}$. But each step contributes at least $\epsilon/4$ to this sum. So $T \cdot \epsilon/4 \leq 2\sqrt{nT}$ forces $T \leq 64n/\epsilon^2$.

Once $\sigma$ is known classically, Bell sample $\rho \otimes \sigma$. The observable $P \otimes P$ has expectation $\mathrm{tr}(P\rho)\mathrm{tr}(P\sigma)$. Since $\mathrm{tr}(P\sigma)$ is known (including sign), and the magnitude $|\mathrm{tr}(P\rho)|$ is known from stage 1, the sign of the product reveals $\mathrm{sign}(\mathrm{tr}(P\rho))$.

## When to reach for it

- Learning all $4^n$ Pauli expectation values of an $n$-qubit state (the full Pauli shadow tomography problem).
- Any setting where the observable set is exponentially large and fractional coloring approaches yield super-polynomial chromatic numbers.
- Compressed classical representations of quantum states with rapid retrieval.

## Complexity

- Copies of $\rho$: $O(n\log(n/\epsilon)/\epsilon^4)$ total (magnitude estimation + sign queries + final Bell sampling).
- Classical runtime: $\mathrm{poly}(2^n, 1/\epsilon)$ — the MMW algorithm operates on $2^n \times 2^n$ density matrices.
- Quantum circuit to prepare $\sigma$: $O(4^n)$ gates (generic state synthesis).

## Caveat

The exponential classical runtime makes this practical only when the observable set itself is exponentially large ($S = \mathcal{P}(n)$). For polynomial-size sets like $k$-body fermionic operators, the [[Fractional Coloring of Commutation Graphs|fractional coloring approach]] is strictly better. The technique has been applied to bosonic displacement operators by King, Wan, and McClean (2024).

## Related notes
- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]]
- [[Bell Difference Sampling for Magnitude Estimation]]
- [[Fractional Coloring of Commutation Graphs]]
- [[Clique Bound from Uncertainty Principle]]
