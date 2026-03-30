# LOCC Indistinguishability from Approximate Randomization

> **Source:** Hayden, Leung, Shor & Winter, arXiv:quant-ph/0307104
> **Tags:** #trick #LOCC #data-hiding #randomization #quantum-correlations

## What it does

Shows that approximate randomization of one subsystem makes any bipartite state indistinguishable from a product state under LOCC measurements, even though quantum correlations (entanglement) survive.

## The trick

Let $R$ be an $\epsilon$-randomizing map on system $A$. For any bipartite state $\rho_{AB}$ and any LOCC POVM $\{X_i \otimes Y_i\}$:

$$\sum_i \left|\operatorname{Tr}[(X_i \otimes Y_i)(R \otimes I)(\rho_{AB})] - \operatorname{Tr}[(X_i \otimes Y_i)(I/d \otimes \rho_B)]\right| \leq \epsilon$$

The proof exploits the separable structure of LOCC POVMs. For rank-1 elements $X_i, Y_i$, and using the identity $(I \otimes Y_i)\Phi(I \otimes Y_i) = \frac{1}{d} Y_i^T \otimes Y_i$, the LOCC measurement effectively measures a product of local operators. The $\epsilon$-randomizing condition on $R$ then bounds each local deviation.

The striking point: for a maximally entangled input $\Phi$, the output $(R \otimes I)(\Phi)$ has rank at most $n = O(d\log d) \ll d^2$, so it is very far from $I/d^2$ in trace norm ($\|R(\Phi) - I/d^2\|_1 \to 2$). The entanglement is still there — a collective measurement can detect it. But LOCC cannot.

## When to reach for it

- **Data hiding.** Encode information using the randomizing map; LOCC measurements see nothing ($\epsilon$-security), but collective operations recover it. Achieves qubit hiding ratio approaching $1:2$.
- **Locked correlations.** Classical correlations of a bipartite state can be locked behind a tiny classical key. Before the key, LOCC extracts almost no mutual information; after the key, it unlocks $\Omega(\log d)$ bits.
- **Understanding the classical-quantum correlation gap.** Classical correlations are always destroyed by local randomization (Lemma III.1). Quantum correlations survive. This is a fundamental distinction.
- **Security arguments for approximate quantum cryptographic protocols.**

## Complexity

No cost beyond applying the randomizing map — the indistinguishability is a consequence of the map's properties, not an additional operation.

## Caveat

The security only holds against LOCC (and more generally, separable superoperators). An adversary with access to the purification of the input state can break the hiding scheme. Also, the operator norm condition on $R$ is required for the proof; it's unknown whether the weaker trace-norm version of $\epsilon$-randomizing suffices.

## Related notes
- [[Randomizing Quantum States (Hayden-Leung-Shor-Winter 2004) — Paper Notes]]
- [[Approximate Randomization with Few Unitaries]]
- [[Large Deviation Bound for Random Unitaries]]
