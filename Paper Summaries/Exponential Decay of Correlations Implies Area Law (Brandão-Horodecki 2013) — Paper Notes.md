> **Source:** Fernando G. S. L. Brandão and Michał Horodecki, *Exponential decay of correlations implies area law*, Communications in Mathematical Physics 333:761–798, 2015 (originally arXiv:1206.2947, 2012)
> **Links:** [arXiv:1206.2947](https://arxiv.org/abs/1206.2947) · [CMP](https://doi.org/10.1007/s00220-014-2213-8)
> **Tags:** #area-law #entanglement #correlations #1D-systems #MPS #data-hiding #single-shot-entropy #entanglement-distillation

---

## The computational problem

Given a pure state $|\psi\rangle_{1,\ldots,n}$ on a 1D chain of $n$ qubits with $(\xi, l_0)$-exponential decay of correlations — meaning for all regions $X, Y$ separated by $l \geq l_0$ sites:

$$\mathrm{Cor}(X:Y) := \max_{\|M\| \leq 1, \|N\| \leq 1} |\mathrm{tr}((M \otimes N)(\rho_{XY} - \rho_X \otimes \rho_Y))| \leq 2^{-l/\xi}$$

— what can be said about the entanglement entropy $S(\rho_X)$ across any cut?

---

## What the paper does

Proves that exponential decay of correlations alone, without assuming any Hamiltonian structure, implies an area law in 1D. The entropy is bounded by $S(X) \leq c' l_0 \exp(c \log(\xi) \cdot \xi)$ for universal constants $c, c'$. This is the partial converse to [[An Area Law for One-Dimensional Quantum Systems (Hastings 2007) — Paper Notes|Hastings (2007)]]:

- **Hastings:** gapped Hamiltonian → exponential decay of correlations → area law (uses the gap)
- **Brandão-Horodecki:** exponential decay of correlations → area law (no Hamiltonian needed)

Combined with the fact (Hastings 2004) that gapped Hamiltonians have exponentially decaying correlations, this completes the logical picture:

$$\text{spectral gap} \implies \text{exponential decay of correlations} \implies \text{area law}$$

As a corollary, any 1D state with finite correlation length can be approximated by an [[MPS Canonical Form for Efficient State Tracking|MPS]] of polynomial bond dimension, giving an equivalence between injective MPS and states with finite correlation length.

The result also implies that quantum computations maintaining exponentially decaying correlations throughout can be efficiently classically simulated — so quantum advantage requires at least algebraically decaying correlations, which is associated with critical phases of matter.

---

## The algorithm / construction

### Main result

**Theorem 1.** For a state $|\psi\rangle_{1,\ldots,n}$ on a ring with $(\xi, l_0)$-exponential decay of correlations and $n \geq C l_0/\xi$, for any connected region $X$ and any $l \geq 8\xi$:

$$H_{\max}^{2^{-l/(8\xi)}}(X) \leq c' l_0 \exp(c \log(\xi) \cdot \xi) + l$$

where $H_{\max}^\epsilon(X) = \min_{\tilde{\rho} \in B_\epsilon(\rho_X)} \log(\mathrm{rank}(\tilde{\rho}))$ is the $\epsilon$-smooth max-entropy (logarithm of the approximate support of $\rho_X$).

**Corollary 2** (area law for von Neumann entropy): $S(X) \leq c' l_0 \exp(c \log(\xi) \cdot \xi)$.

**Corollary 3** (MPS approximation): there exists an MPS $|\phi_k\rangle$ with bond dimension $k = \mathrm{poly}(n\xi\log(\xi), \delta)$ such that $|\langle\psi|\phi_k\rangle| \geq 1 - \delta$.

### Proof sketch

The proof uses an entanglement distillation argument that exploits exponential decay of correlations in *multiple different partitions simultaneously*. This is what gets around the data-hiding obstruction.

**Step 1: The key intuition from entanglement distillation**

For a tripartite pure state $|\psi\rangle_{ABC}$, the [[Negative Conditional Entropy as Entanglement Gain|hashing bound]] says that a random measurement on $A$ with $\sim 2^{I(A:B)}$ outcomes can distill $\sim 2^{-H(A|C)}$ EPR pairs between $A$ and $C$. If $H(C) > H(B)$ (i.e., $H(A|C) < 0$ by purity), then $A$ and $C$ share distillable entanglement, implying:

$$\mathrm{Cor}(A:C) \geq 2^{-I(A:B)}$$

Contrapositing: if correlations between $A$ and $C$ are small, then $H(C) \leq H(B)$ (area law for $C$ if $B$ is a thin boundary).

The problem: we don't know that $I(A:B) \leq l/\xi$ (the mutual information could be large even with decaying correlations).

**Step 2: Finding a region with small entropy (saturation of mutual information)**

Using a modified version of a result from [[An Area Law for One-Dimensional Quantum Systems (Hastings 2007) — Paper Notes|Hastings' area law proof]], the paper shows (Lemma 7) that there exists a region $X_{C,l}$ of size $l \leq l_0 \exp(O(\log(1/\epsilon)/\epsilon))$, at distance $\leq l_0 \exp(O(\log(1/\epsilon)/\epsilon))$ from the cut, such that the smooth max-mutual information satisfies:

$$I_{\max}^\delta(X_{C,l} : X_{L,l/2} X_{R,l/2}) \leq \frac{8\epsilon l}{\delta} + \delta$$

This is the single-shot analogue of Hastings' mutual information saturation lemma, and its proof is considerably harder — it uses the quantum substate theorem (Jain-Radhakrishnan-Sen) and the quantum equipartition property.

**Step 3: Correlations vs. entropies (Lemma 6)**

This is the technical core — three relations between correlation functions and smooth entropies for a tripartite pure state $|\psi\rangle_{ABC}$:

1. If $H_{\min}^\delta(A|B) \geq -2\log(\delta) + 5$, then $\mathrm{Cor}(A:C) \geq (\frac{1}{256} - 26\sqrt{\delta}) \cdot 2^{-(I_{\max}^\delta(A:B) - 2\log(\delta) + 5)}$

2. If $H_{\max}^\nu(C) \geq 3 H_{\max}^\delta(B)$, then $\mathrm{Cor}(A:C) \geq (\text{const}) \cdot 2^{-3(H_{\max}^\delta(B) + O(\log(1/\delta)))}$

3. $H_{\max}^{2\gamma}(A) \leq H_{\max}^\delta(A) + 2\log|B| + \log(2/\gamma^2)$ where $\gamma = \mathrm{Cor}(A:C)^{1/2}(1/2 - \delta)^{-1/2}$

Part 1 comes from the single-shot entanglement distillation protocol. Part 2 is novel: it shows that even when EPR pairs cannot be distilled (because $H_{\min}^\delta(A|B) \leq 0$), a random measurement on $A$ still generates correlations with $C$ if $C$ has much larger max-entropy than $B$. Part 3 bounds how much random measurements can reduce the max-entropy.

**Step 4: Putting it together**

Apply Lemma 7 twice (to both boundaries of region $X$) to find regions $Y, \tilde{Y}$ with controlled max-mutual information. Apply Part 1 of Lemma 6 combined with exponential decay of correlations to show that $H_{\max}(Y_C)$ and $H_{\max}(\tilde{Y}_C)$ must be small. Then apply Part 2 to bound $H_{\max}(C) < 3 H_{\max}(B)$ where $B = Y_C \cup \tilde{Y}_C$. Combine with subadditivity to get the area law for $X$. Finally, apply Part 3 to improve the smoothing parameter.

In total, exponential decay of correlations is used four times in different partitions.

---

## Key results

**Area law:** $S(X) \leq c' l_0 \exp(c \log(\xi) \cdot \xi)$ for any connected region $X$ of a 1D state with correlation length $\xi$.

**MPS approximation:** Bond dimension $k = \mathrm{poly}(n\xi\log\xi, \delta)$ suffices for $|\langle\psi|\phi_k\rangle| \geq 1 - \delta$.

**Classical simulability:** 1D quantum circuits maintaining finite correlation length are efficiently classically simulable.

**Mixed state extension (Theorem 4):** $H_{\max}^{2^{-l/(8\xi)}}(X) \leq c' l_0 \exp(c\log(\xi)\cdot\xi)(1 + H_{\max}(\rho)) + l$.

---

## Comparison with prior work

| Result | Assumptions | Entropy bound |
|---|---|---|
| [[An Area Law for One-Dimensional Quantum Systems (Hastings 2007) — Paper Notes\|Hastings (2007)]] | Gapped 1D Hamiltonian | $\exp(\xi \ln D)$ |
| Arad-Kitaev-Landau-Vazirani (2013) | Gapped 1D Hamiltonian | $\mathrm{poly}(1/\Delta E)$ (improved) |
| **Brandão-Horodecki (2013)** | **Exponential decay of correlations only** | **$\exp(\xi \log \xi)$** |
| Quantum expanders (Hastings 2007) | MPS with fast mixing | No area law (but also not ground states of local Hamiltonians) |

The generality is the main advance: no Hamiltonian is assumed. The entropy bound matches Hastings' in its exponential dependence on $\xi$. Quantum expander states show this exponential dependence is essentially tight for the general setting (they have correlation length $O(\log(D)/\log(d))$ and entropy $\approx \log(D)$, matching the linear dependence of $l_0$ in the bound).

---

## Limits / caveats

- The result is **1D only**. Whether exponential decay of correlations implies an area law in higher dimensions remains open.

- The entropy bound is **exponential in $\xi \log \xi$** — loose compared to the polynomial bound of AKLV for gapped Hamiltonians specifically. But the exponential is tight for the general setting (quantum expander states achieve it).

- The proof uses the **pure state** assumption heavily (purity relates $H(AC)$ to $H(B)$). The mixed-state extension (Theorem 4) has an extra factor of $H_{\max}(\rho)$, and the maximally mixed state shows this is necessary.

- The **minimum correlation length** $l_0$ enters linearly in the bound. This is also tight (quantum expander states with $l_0 \sim \log D$ have entropy $\sim \log D$).

- The single-shot entropy technology makes the proof technically demanding. The connection to entanglement distillation is elegant but the details involve multiple layers of smoothing parameters.

---

## Reusable ideas

1. [[Single-Shot Entanglement Distillation for Correlation Lower Bounds]] — Use the operational interpretation of smooth min-entropy as a single-shot entanglement distillation rate to convert entropy imbalances into correlation lower bounds. A random measurement on one part with $\sim 2^{I_{\max}(A:B)}$ outcomes generates correlations with the third part.

2. [[Saturation of Max-Mutual Information in Correlated States]] — For any 1D state with exponential decay of correlations, there exists a nearby region where the max-mutual information with its boundary is small. Uses the quantum substate theorem and quantum equipartition property to lift Hastings' von Neumann mutual information saturation to the single-shot setting.

---

## References within this paper

- [[An Area Law for One-Dimensional Quantum Systems (Hastings 2007) — Paper Notes|Hastings (2007)]] — the original area law proof for gapped 1D systems; Lemma 7 is a single-shot version of a technique from this paper
- Hastings (2004) — exponential decay of correlations in gapped systems
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes|Vidal (2003)]] — MPS classical simulation; Corollary 3 strengthens Vidal's condition
- [[Randomizing Quantum States (Hayden-Leung-Shor-Winter 2004) — Paper Notes|Hayden-Leung-Shor-Winter (2004)]] — data hiding states as obstruction to naive area law arguments
- Hastings (2007, cond-mat/0701055) — quantum expander states: MPS with decaying correlations and large entropy
- Fannes-Nachtergaele-Werner (1992) — injective MPS have finite correlation length (the converse direction)
- Jain-Radhakrishnan-Sen (2003) — quantum substate theorem
- Tomamichel-Colbeck-Renner (2009) — smooth entropy framework
- Devetak-Winter (2005), Horodecki-Oppenheim-Winter (2005) — entanglement distillation achieving the hashing bound
- König-Renner-Schaffner (2009) — operational meaning of smooth min-entropy
- Arad-Kitaev-Landau-Vazirani (2013) — improved polynomial area law bound for gapped Hamiltonians

---

## Cross-links

### Paper notes
- [[An Area Law for One-Dimensional Quantum Systems (Hastings 2007) — Paper Notes]] — the original area law for gapped 1D systems; this paper generalises from gapped Hamiltonians to any state with exponential decay of correlations
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]] — MPS simulation; this paper's Corollary 3 gives a physical condition (finite correlation length) that guarantees MPS approximability
- [[On the Role of Entanglement in Quantum-Computational Speed-Up (Jozsa-Linden 2003) — Paper Notes]] — entanglement necessary for quantum speedup; this paper strengthens the connection: finite correlation length → bounded entanglement → classical simulability
- [[Quantum State Merging and Negative Information (Horodecki-Oppenheim-Winter 2005) — Paper Notes]] — state merging and negative conditional entropy; the entanglement distillation protocol in the proof is a single-shot version of state merging
- [[Randomizing Quantum States (Hayden-Leung-Shor-Winter 2004) — Paper Notes]] — data hiding states; the paper shows why data hiding doesn't obstruct the area law (it requires correlations in multiple partitions)

### Trick cards
- [[Single-Shot Entanglement Distillation for Correlation Lower Bounds]]
- [[Saturation of Max-Mutual Information in Correlated States]]
- [[Entanglement as Simulation Complexity Measure]]
- [[MPS Canonical Form for Efficient State Tracking]]
- [[Approximate Ground State Projector from Local Operators]]
