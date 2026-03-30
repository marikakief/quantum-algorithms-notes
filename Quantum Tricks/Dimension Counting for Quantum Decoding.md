# Dimension Counting for Quantum Decoding

> **Source:** Nayak, arXiv:quant-ph/9904093 (1999)
> **Tags:** #trick #quantum-information #dimension-argument #decoding #holevo-bound

## What it does
Bounds the probability of correctly decoding a quantum encoding by the dimension of the codespace, without going through entropy or mutual information. Often gives tighter results than Holevo's theorem for specific decoding problems.

## The trick

Suppose $n$-bit strings $x$ are encoded as mixed states $\rho_x = \sum_i q_{x,i} |\phi_{x,i}\rangle\langle\phi_{x,i}|$ over $m$ qubits. For any decoding measurement $\{P_x\}$ (projections in the codespace plus ancilla):

$$\Pr[D(Y) = X] = \sum_x p_x \sum_i q_{x,i} \|P_x |\phi_{x,i}\rangle\|^2 \leq \sum_x p_x \|P_x |\phi_x\rangle\|^2$$

where $|\phi_x\rangle$ is the codeword maximising $\|P_x |\phi_x\rangle\|^2$.

**Key claim:** $\sum_x \|P_x |\phi_x\rangle\|^2 \leq 2^m$.

**Proof:** Let $E = \text{span}\{|\phi_x\rangle\}$ with $\dim(E) \leq 2^m$. Pick an orthonormal basis $\{|e_i\rangle\}$ for $E$ and $\{|\hat{e}_{x,j}\rangle\}$ for $\text{range}(P_x)$. Then $\|P_x |\phi_x\rangle\|^2 \leq \sum_j \|Q |\hat{e}_{x,j}\rangle\|^2$ where $Q$ is the projector onto $E$. Summing: $\sum_x \|P_x |\phi_x\rangle\|^2 \leq \sum_i \sum_{x,j} |\langle e_i | \hat{e}_{x,j}\rangle|^2 = \sum_i 1 = \dim(E) \leq 2^m$.

**Conclusion:** For a random variable $X$ encoded into $m$ qubits:

$$\Pr[\text{correct decoding}] \leq P(X, 2^m)$$

where $P(X, 2^m)$ is the total probability of the $2^m$ most likely values of $X$. When $X$ is uniform over $2^n$ strings: $\Pr[\text{correct}] \leq 2^{m-n}$.

## When to reach for it

- Any time you need a decoding probability bound and going through Holevo's theorem + Fano's inequality would lose tightness
- Communication complexity arguments where one party's actions are oblivious to their input (combined with the Kremer-Yao lemma that restricts the effective subspace to dimension $2^m$)
- Settings where the encoding is into orthogonal states and you want an exact bound, not just an entropy-based one

## Complexity
The bound is tight: for any distribution $X$ and $m$, there exists an encoding and decoding achieving $P(X, 2^m)$.

## Caveat
Does not generalise to entanglement-assisted communication (unlike Holevo's theorem). The proof critically uses the fact that the codespace is contained in $(\mathbb{C}^2)^{\otimes m}$ with no external entanglement.

## Related notes
- [[Optimal Lower Bounds for Quantum Automata and Random Access Codes (Nayak 1999) — Paper Notes]]
- [[Entropy Accumulation via Holevo's Theorem]]
- [[Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) — Paper Notes]]
