> **Source:** Chien Hung Cho, Dominic W. Berry, Min-Hsiu Hsieh, *Doubling the order of approximation via the randomized product formula*, arXiv:2210.11281, Phys. Rev. A **109**, 062431 (2024)
> **Links:** [arXiv](https://arxiv.org/abs/2210.11281) · [PRA](https://doi.org/10.1103/PhysRevA.109.062431)
> **Tags:** #hamiltonian-simulation #product-formulas #randomization #trotter #diamond-norm

---

## The computational problem

Given $H = \sum_{j=1}^L H_j$ with $\Lambda := \max_j \|H_j\|$, simulate $V = e^{-iHt}$ to diamond-norm error $\varepsilon$ using a randomized product-formula channel. The complexity measure is the total number of exponentials $e^{\pm i H_j \tau}$ in the circuit.

## What the paper does

Takes a standard $(2k)$-th order [[Order-Condition Cancellation in Product Formulas|Suzuki product formula]] $S_{2k}$ and constructs a randomized channel that achieves $(4k+1)$-th order approximation — more than doubling the order of the error from $2k+1$ to $4k+2$. The construction inserts a random correction unitary $U_h^{(l)} = e^{\alpha_{h,l} H_h^{(l)}}$ between two half-steps of $S_{2k}$:

$$\mathcal{E}(\rho) = \sum_{l,h} p_{h,l} \bigl[S_{2k}(\lambda/2)\, U_h^{(l)}\, S_{2k}(\lambda/2)\bigr] \rho \bigl[\cdots\bigr]^\dagger$$

The correction terms are products of the original Hamiltonian summands $H_j$, so they're efficiently implementable when $H$ is a sum of Pauli strings (as in quantum chemistry).

The number of exponentials to achieve error $\varepsilon$ is $O\bigl(tL^2 (tL/\varepsilon)^{1/(4k+1)}\bigr)$, which improves over both the deterministic Suzuki formula and (in certain regimes) the [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes|Childs-Ostrander-Su randomized formula]].

## The algorithm / construction

### Step 1: Expand the error of $S_{2k}$

Write $S_{2k}(\lambda/2) = V(\lambda/2) + D(\lambda/2)$ where $D$ captures the deviation. The key identity:

$$S_{2k}(\lambda/2)\bigl(\mathbb{1} - V^\dagger D - D V^\dagger\bigr)S_{2k}(\lambda/2) = V(\lambda) + O(\lambda^{4k+2})$$

This works because $(V^\dagger S_{2k})^{-1}(S_{2k} V^\dagger)^{-1}$ is unitary and satisfies time-reversibility. Since $S_{2k}$ is a symmetric formula, the BCH expansion of this product contains only odd-order terms in $\lambda$ starting at order $2k+1$. The even-order error terms at orders $2k+2, 2k+4, \ldots, 4k$ vanish automatically — a direct consequence of the [[Order-Condition Cancellation in Product Formulas|symmetric structure]] and the Yoshida time-reversibility lemma.

### Step 2: Identify the correction terms

The correction $V^\dagger D + D V^\dagger$ decomposes as:

$$V^\dagger D + D V^\dagger = \sum_{l \in \gamma} \frac{\lambda^l}{2^l} \mathcal{H}_l + O(\lambda^{4k+2})$$

where $\gamma = \{2k+1, 2k+3, \ldots, 4k+1\}$ (only odd orders) and each $\mathcal{H}_l = \sum_{h=1}^{L_l} \beta_h^{(l)} H_h^{(l)}$ is a Hermitian operator built from products of $l$ individual $H_j$ terms.

### Step 3: Construct the randomized channel

For each correction term $H_h^{(l)}$, define:
- Correction unitary: $U_h^{(l)} = \exp\bigl(-\mathrm{sgn}(\varepsilon_{h,l}) \cdot A / \|H_h^{(l)}\| \cdot H_h^{(l)}\bigr)$
- Sampling probability: $p_{h,l} = |\varepsilon_{h,l}| / A$

where $\varepsilon_{h,l} = (\lambda/2)^l \beta_h^{(l)} \|H_h^{(l)}\|$ and $A = \sum_{l,h} |\varepsilon_{h,l}|$.

This ensures $\sum p_{h,l} \alpha_{h,l} H_h^{(l)} = -\sum (\lambda/2)^l \beta_h^{(l)} H_h^{(l)}$, so the average evolution cancels the error up to order $4k+2$.

### Step 4: Apply mixing lemma

The [[Diamond-Norm Channel Averaging for Random Compilers|Hastings-Campbell mixing lemma]] converts the average-channel guarantee into a diamond-norm bound: $\|\mathcal{E} - \mathcal{V}\|_\diamond \le a^2 + 2b$, where $a$ bounds the worst-case individual-circuit error and $b$ bounds the average-evolution error.

## Key results

**Theorem 2.** The diamond-norm error of the modified randomized channel satisfies:

$$\|\mathcal{V}(\lambda) - \mathcal{E}(\lambda)\|_\diamond \le a^2 + 2b$$

with

$$a \le \frac{4[(5^{k-1} + 1/2)|\lambda| L\Lambda]^{2k+1}}{(2k+1)!} \exp\bigl((5^{k-1} + 1/2)|\lambda| L\Lambda\bigr)$$

$$b \le \frac{2[(5^{k-1} + 1/2)|\lambda| L\Lambda]^{4k+2}}{(4k+2)!} \exp\bigl((5^{k-1} + 1/2)|\lambda| L\Lambda\bigr) + \frac{A^2}{2} e^A + 3\|D\|^2 + 2\|D\|^3$$

**Asymptotic scaling.** For $r$ segments with $\lambda = -it/r$ and $r \ge (5^{k-1} + 1/2)tL\Lambda$:

$$\|\mathcal{V}(-it) - \mathcal{E}^r(-it/r)\|_\diamond \le O\!\left(\frac{(tL)^{4k+2}}{r^{4k+1}}\right)$$

To reach error $\varepsilon$, the number of segments is $r = O\bigl(tL(tL/\varepsilon)^{1/(4k+1)}\bigr)$, giving total exponentials:

$$g_{4k+1}^m = O\!\left(tL^2 \left(\frac{tL}{\varepsilon}\right)^{\!1/(4k+1)}\right)$$

## Comparison with prior work

| Method | Number of exponentials | Error measure |
|---|---|---|
| $(2k)$-th order Suzuki | $O\bigl(tL^2 (tL/\varepsilon)^{1/(2k)}\bigr)$ | Spectral norm |
| $(2k)$-th order randomized (Childs et al.) | $\max\bigl\{O\bigl(tL^2 (tL/\varepsilon)^{1/(4k+1)}\bigr),\, O\bigl(tL^2 (t/\varepsilon)^{1/(2k)}\bigr)\bigr\}$ | Diamond norm |
| **$(4k+1)$-th order modified randomized (this paper)** | $O\bigl(tL^2 (tL/\varepsilon)^{1/(4k+1)}\bigr)$ | Diamond norm |

The advantage over [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes|Childs-Ostrander-Su]] is the elimination of the second term in the max. When $L = o\bigl((t/\varepsilon)^{1+1/(2k)}\bigr)$, that second term dominates and Cho-Berry-Hsieh does strictly better. For the other regime where $L$ is large relative to $t/\varepsilon$, both methods match.

The improvement over deterministic Suzuki is in all parameters simultaneously.

## Limits / caveats

1. **Pauli-string structure required.** The correction terms $H_h^{(l)}$ are products of $l$ individual Hamiltonian terms. For arbitrary $H_j$, implementing $e^{\alpha H_h^{(l)}}$ may be hard. The method is natural when $H$ is a sum of Pauli strings (each correction is also a Pauli string) but less obviously useful for, say, sparse Hamiltonians accessed via an oracle.

2. **Output is a channel, not a unitary.** Like all randomized simulation methods, you get a quantum channel approximation in diamond norm, not a single coherent unitary. This matters if the simulation needs to be a subroutine inside coherent amplification — you can't directly plug this into [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]] or similar.

3. **Prefactor growth.** The $5^{k-1}$ factor in the error bounds comes from the Suzuki recursion's exponential count. At high $k$, the prefactor becomes large. The paper doesn't discuss optimised base formulas (e.g., [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes|Morales et al. 2025]] or Yoshida-type) that could tighten this. In principle the method works for any symmetric order-$2k$ formula — the $5^{k-1}$ is specific to Suzuki.

4. **No numerical experiments.** 12 pages, no figures, no numerical validation. The paper is purely analytical, so the practical tightness of the bounds is unknown. Compare with [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su]] and [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes|Bagherimehrab-Berry]] which provide extensive numerics showing their bounds are often much tighter than the asymptotic analysis suggests.

5. **Comparison with corrected product formulas.** The [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes|Bagherimehrab-Berry (2024)]] corrected product formulas achieve a similar goal — injecting corrections to cancel error terms — but deterministically and ancilla-free. CPFs gain two orders in error for a single unitarily implementable correction at the boundary. The randomised approach here gains more orders $(2k \to 4k+1)$ but at the cost of producing a channel rather than a unitary. This is a genuine trade-off, not a strict domination in either direction.

## My assessment

The central idea is clean: exploit the symmetric structure of Suzuki formulas to identify which error terms survive, then kill them with randomized corrections via the mixing lemma. The order-doubling is real and the construction is general.

But the paper sits in an awkward competitive landscape. It appeared in 2022, between the [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes|Childs-Ostrander-Su (2019)]] randomized formula (which already gets the same $tL$ scaling in one regime) and the later [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes|corrected product formulas]] and [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes|Stochastic QSP]] (which offer deterministic and more general improvements respectively). The advantage window — when $L$ is moderate and $t/\varepsilon$ is large, but you don't want to leave the product-formula framework — is real but narrow.

The connection to [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes|Faehrmann et al. (2022)]] is interesting: both papers use randomization to boost the effective order of a product-formula-based method, but Faehrmann et al. average over different step counts (multi-product structure) while Cho-Berry-Hsieh average over correction unitaries within a single step count. The underlying machinery — mixing lemma, diamond-norm analysis — is shared.

The Pauli-string requirement is the biggest practical limitation. Quantum chemistry fits naturally, but this cuts out oracle-based sparse Hamiltonian simulation, which is where much of the theoretical interest in product formulas lies.

## Reusable ideas

1. [[Randomized Correction to Double Product-Formula Order]] — the core construction: sandwich a random correction unitary between two half-steps of a symmetric product formula to double the error order, using the mixing lemma for the diamond-norm guarantee
2. [[Time-Reversibility Parity Trick for Error Cancellation]] — the observation that $(V^\dagger S_{2k})^{-1}(S_{2k} V^\dagger)^{-1}$ contains only odd-order error terms (via Yoshida's time-reversibility lemma), automatically killing half the correction orders

## References within this paper

- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — first-order product formula simulation
- Suzuki (1991) — systematic $(2k)$-th order product formulas
- [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes|Childs, Ostrander, Su (2019)]] — randomized product formulas via permutation averaging; the direct predecessor
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|Campbell (2019)]] — qDRIFT protocol
- Campbell (2017, PRA 95 042306) — the [[Diamond-Norm Channel Averaging for Random Compilers|mixing lemma]] used throughout the analysis
- Hastings (2016, arXiv:1612.01011) — mixing lemma precursor
- Yoshida (1990) — time-reversibility of symmetric product formulas; the key structural lemma enabling even-order cancellation
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry, Ahokas, Cleve, Sanders (2007)]] — operator-norm bounding technique used in the appendix
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs, Su, Tran, Wiebe, Zhu (2021)]] — tight commutator-based Trotter bounds; comparison point

## Cross-links

### Paper notes
- [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes]] — direct predecessor; this paper improves the scaling in one regime
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — the [[Trotter Commutator-Scaling Bound|commutator scaling]] framework; deterministic bounds that this method beats
- [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes]] — deterministic corrections achieving similar goals without randomization
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]] — parallel work using randomization for multi-product formulas; shared mixing-lemma machinery
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] — the Suzuki recursion framework this paper builds on
- [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes]] — later work using randomization (Stochastic QSP) for more general order-halving; same mixing-lemma philosophy
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]] — alternative randomized simulation; different regime (short time / many terms)
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] — symmetry kicks as a different randomized error-reduction strategy; both papers target practical Trotter cost reduction beyond what norm bounds suggest
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] — showed empirically that norm Trotter bounds overestimate by $10^{16}$; the order-doubling here addresses the same regime of practical improvement
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — concrete Trotter resource estimates for condensed-phase Hamiltonians; demonstrates the kind of simulation where doubled effective order would reduce gate costs
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]] — optimized high-order product formulas; using those as the base formula instead of Suzuki would tighten the $5^{k-1}$ prefactor in this paper's bounds

### Trick cards
- [[Randomized Correction to Double Product-Formula Order]] — the main construction from this paper
- [[Time-Reversibility Parity Trick for Error Cancellation]] — the symmetric-formula parity argument
- [[Diamond-Norm Channel Averaging for Random Compilers]] — the mixing lemma machinery
- [[Mixing-Lemma Upgrade from Average-Channel Error to Diamond Norm]] — the analysis framework
- [[Order-Condition Cancellation in Product Formulas]] — the Suzuki symmetric structure exploited here
- [[Permutation Averaging for Product-Formula Error Cancellation]] — related randomization technique from Childs-Ostrander-Su
- [[Symplectic Corrector Injection for Product Formulas]] — deterministic alternative from Bagherimehrab-Berry
