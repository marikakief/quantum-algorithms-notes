> **Source:** Gregory D. Kahanamoku-Meyer, John Blue, Thiago Bergamaschi, Craig Gidney, Isaac L. Chuang, *A log-depth in-place quantum Fourier transform that rarely needs ancillas*, arXiv:2505.00701 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2505.00701)
> **Tags:** #QFT #circuit-depth #optimistic-circuits #fault-tolerant #factoring #quantum-arithmetic

---

## The computational problem

Implement the $n$-qubit [[Quantum Fourier Transform Circuit|quantum Fourier transform]] (QFT) as a quantum circuit, minimising three resources simultaneously: circuit depth, number of ancilla qubits, and gate locality (interaction range in a 1D qubit layout). Prior constructions could achieve $O(\log n)$ depth but required $O(n)$ ancillae (Cleve–Watrous 2000, Hales 2002) or measurement and feedforward (Bäumer–Sutter–Woerner 2025). No prior construction achieved logarithmic depth, zero ancillae, and locality simultaneously.

## What the paper does

Introduces the concept of **optimistic quantum circuits** — circuits that approximate a unitary well on all but an $O(\varepsilon)$-fraction of the Hilbert space — and builds an optimistic in-place QFT with depth $O(\log(n/\varepsilon))$, zero ancilla qubits, nearest-neighbour gates in 1D, and no measurements. The paper also provides a [[Worst-to-Average-Case Reduction for Optimistic Circuits|worst-to-average-case reduction]] that converts any optimistic circuit into one with bounded error on all inputs, yielding the first approximate QFT at asymptotically optimal depth $O(\log(n/\varepsilon))$ with sublinear ancillae.

Combined with QFT-based fast arithmetic (Kahanamoku-Meyer–Yao 2024), the optimistic QFT gives a factoring circuit with depth $O(n^{1+\epsilon})$ using only $2n + O(n/\log n)$ qubits — by far the best depth at this qubit count.

## The algorithm / construction

### Optimistic quantum circuits (framework)

An optimistic circuit $\widetilde{U}$ for a unitary $U$ on Hilbert space $\mathcal{H}$ has error parameter $\varepsilon$ if

$$\frac{\|\widetilde{U} - U\|_F^2}{\dim \mathcal{H}} \leq \varepsilon$$

where $\|\cdot\|_F$ is the Frobenius norm. This is equivalent to requiring that the average error over any complete basis is at most $\varepsilon$. By Lemma 1, the number of basis states with $O(1)$ error is at most $O(\varepsilon) \cdot \dim \mathcal{H}$ — the "bad" subspace is small.

The definition is basis-independent (unlike average-case classical complexity, which depends on an input distribution). This basis independence comes from two choices: measuring error via the length of the error vector (not fidelity), and taking the average (not maximum) over basis states.

### The optimistic QFT

Divide the $n$-qubit register into blocks of $m = O(\log(n/\varepsilon))$ qubits each. Let $X_i$ denote the $m$-bit integer value of block $i$.

The standard approximate QFT (Coppersmith 1994) truncates inter-block interactions to nearest neighbours, yielding a "blockwise" construction: iterate through blocks, applying a local $m$-bit QFT to block $i$, then a phase rotation $\omega^{X_{i+1} Y_i / 2^m}$ between blocks $i$ and $i+1$. This has linear depth because block $i-1$ needs access to $X_i$ before block $i$ can be processed.

The optimistic QFT breaks this serial dependency using [[Quantum Phase Estimation on Blocks for QFT Parallelisation|block-wise quantum phase estimation]]:

**Algorithm (5 steps):**
1. Apply $\text{QFT}_{2^m}$ to even-indexed blocks.
2. Apply phase rotation $\exp(2\pi i X_{i+1} Y_i / 2^{2m})$ for all even $i$.
3. Apply $\text{QFT}_{2^m}$ to odd blocks and $\text{QFT}_{2^m}^\dagger$ to even blocks.
4. Apply phase rotation $\exp(2\pi i X_{i+1} Y_i / 2^{2m})$ for all odd $i$.
5. Apply $\text{QFT}_{2^m}$ to even blocks.

The idea: after step 1, even blocks are in the Fourier basis. Applying $\text{QFT}^\dagger$ (step 3) performs quantum phase estimation, recovering an approximation $|\widetilde{X}_i\rangle$ of the original block value $X_i$. This approximation is good enough to control the phase rotation on the neighbouring odd block in step 4 — except when $X_i$ is near $0$ or $2^m$, where the QPE distribution wraps around modulo $2^m$. These "wraparound" states are the bad subspace, but they constitute only an $O(\varepsilon)$ fraction.

**Alternative form (Algorithm 2):** The same circuit expressed using only QFT blocks:
1. Apply $\text{QFT}_{2^{2m}}$ spanning blocks $i$ and $i+1$ for all even $i$.
2. Apply $\text{QFT}_{2^m}^\dagger$ to all blocks.
3. Apply $\text{QFT}_{2^{2m}}$ spanning blocks $i$ and $i+1$ for all odd $i$.

This form is simpler to implement — any exact QFT circuit can be plugged into the blocks.

### Worst-to-average-case reduction

The reduction converts an optimistic circuit into one with bounded error on all inputs:

1. Draw $V$ from a unitary 1-design on $\mathcal{H}$.
2. Apply $V$, then $\widetilde{U}$, then $\hat{V}^\dagger = U V^\dagger U^\dagger$.

The pre-randomisation by $V$ turns any input into an effectively random state (where the optimistic circuit has low error), and $\hat{V}^\dagger$ undoes the randomisation. By Theorem 1, the expected error is at most $\varepsilon$ for any input state.

For the QFT, a natural 1-design is the Weyl–Heisenberg group: $V(r_1, r_2) = X_{2^n}^{r_1} Z_{2^n}^{r_2}$, where $X_{2^n}$ adds $r_1$ into the register and $Z_{2^n}$ applies a phase gradient $e^{2\pi i r_2 x / 2^n}$. The conjugated operator is simply $\hat{V}^\dagger(r_1, r_2) = V(r_2, -r_1)$ — the QFT swaps the roles of $X$ and $Z$ in the Weyl–Heisenberg group.

**Derandomisation via purification (Theorem 2):** Encode the 1-design into a quantum control register: $V' = \sum_i |i\rangle\langle i| \otimes V_i$. Applying $\hat{V}'^\dagger (\mathbb{I} \otimes \widetilde{U}) V'$ to $\frac{1}{\sqrt{k}} \sum_i |i\rangle \otimes |\psi\rangle$ achieves error $\leq \varepsilon$ deterministically.

### Application to factoring

For [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]], the optimistic QFT can be used directly (without the reduction) for the modular multiplications. The key argument (Theorem 7): if the initial state includes a random $r < N$, the intermediate values $y_i$ in modular exponentiation are random elements of the multiplicative group mod $N$, so the optimistic QFT has low expected error. The reduction is needed only if one demands worst-case guarantees on all inputs.

The final QFT in quantum phase estimation should use the standard (linear-depth) construction with qubit recycling, since the modular exponentiation already requires linear depth.

## Key results

**Optimistic QFT (Theorem 3):**
$$\text{Depth} = O(\log(n/\varepsilon)), \quad \text{Ancillae} = 0, \quad \text{Gate range} = O(\log(n/\varepsilon))$$
No measurements needed. Error $\varepsilon$ in the Frobenius sense (Definition 2).

**Randomized approximate QFT (Theorem 4):**
$$\text{Depth} = O(\log(n/\varepsilon)), \quad \text{Ancillae} = O(n/\log(n/\varepsilon)), \quad \text{Gates} = O(n \log(n/\varepsilon))$$
Expected error $\varepsilon$ on any input. Uses classical-quantum addition (Takahashi–Kunihiro 2008) with block size $m$.

**Unitary (derandomized) approximate QFT (Theorem 5):**
$$\text{Depth} = O(\log(n/\varepsilon)), \quad \text{Total qubits} = 3n + O(n/\log(n/\varepsilon)), \quad \text{Gates} = O(n \log(n/\varepsilon))$$
Deterministic, no measurements. Uses quantum-quantum addition (Takahashi–Tani–Kunihiro 2009) for controlled Weyl–Heisenberg operators.

**Factoring (combining with Kahanamoku-Meyer–Yao 2024):**
$$\text{Depth} = O(n^{1+\epsilon}), \quad \text{Qubits} = 2n + O(n/\log n)$$

## Comparison with prior work

| Construction | Depth | Total qubits | Ancillae | Locality | Measurements |
|---|---|---|---|---|---|
| Standard approximate (Coppersmith 1994) | $O(n \log n)$ | $n$ | 0 | Long-range | No |
| Cleve–Watrous (2000) | $O(\log n)$ | $n + O(n \log n)$ | $O(n \log n)$ | Long-range | No |
| Hales (2002) | $O(\log n)$ | $2n$ | $O(n)$ | Long-range | No |
| Bäumer–Sutter–Woerner (2025) | $O(\log n)$ | $2n$ | $O(n)$ | Nearest-neighbour | Yes |
| **Optimistic QFT (this paper)** | $O(\log(n/\varepsilon))$ | $n$ | **0** | **NN in 1D** | **No** |
| **Randomized approx QFT** | $O(\log(n/\varepsilon))$ | $n + O(n/\log(n/\varepsilon))$ | $O(n/\log(n/\varepsilon))$ | Long-range | No |
| **Unitary approx QFT** | $O(\log(n/\varepsilon))$ | $3n + O(n/\log(n/\varepsilon))$ | $2n + O(n/\log(n/\varepsilon))$ | Long-range | No |

The optimistic QFT achieves something provably impossible for a standard approximate QFT: simultaneous logarithmic depth, zero ancillae, and locality — at the cost of allowing small error on a small fraction of inputs. The randomized reduction is the first to achieve optimal depth with sublinear ancillae. The unitary version is the first deterministic circuit at this depth with $3n + o(n)$ total qubits.

## Limits / caveats

1. **Optimistic ≠ approximate.** The optimistic QFT has $O(1)$ error on $O(\varepsilon) \cdot 2^n$ basis states. For most applications this is fine (Shor's algorithm works with high probability), but some algorithms may need guaranteed error bounds on all inputs. The reduction addresses this but adds ancillae.

2. **Bad subspace structure.** The states with large error are exactly those where some block $X_i$ is near $0$ or $2^m$ — the QPE wraparound states. These are the same states that require long-range information propagation in the QFT (the $|000\ldots0\rangle$ / $|111\ldots1\rangle$ counterexample). This is not a defect of the construction but a fundamental feature of the QFT.

3. **Long-range gates in the reduction.** The optimistic QFT itself uses only nearest-neighbour gates, but the randomized/unitary reductions require the integer addition $X_{2^n}^{r_1}$, which uses long-range gates (or ancillae for carry propagation). Using [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes|Häner et al.'s]] classical-quantum adder keeps locality but adds depth; using the QFT-based adder adds long-range gates. The block size can be tuned — using $m = 2\sqrt{\log(n/\varepsilon)}$ with optimised Toffoli ladders (Remaud–Vandaele 2025) reduces ancillae to $O(n / 2^{\sqrt{\log(n/\varepsilon)}})$.

4. **Not yet optimised for implementation.** The paper notes several practical optimisations left unexplored: using [[Phase Gradient State for Controlled Rotations|phase gradient states]] for the local QFT blocks, tuning block sizes differently for odd and even blocks, and exploiting locality for routing cost reduction in factoring circuits.

## Reusable ideas

1. [[Optimistic Quantum Circuits]] — Formalisation of circuits with small average error (Frobenius norm), allowing $O(\varepsilon)$-fraction of inputs to have large error. Basis-independent. Often sufficient as drop-in replacements for exact unitaries (shown explicitly for Shor's algorithm).

2. [[Worst-to-Average-Case Reduction for Optimistic Circuits]] — Generic conversion of optimistic circuits to standard approximate circuits via pre-randomisation with a unitary 1-design and conjugated post-correction. The randomised version adds the cost of implementing two 1-design elements; derandomisation via purification adds a control register.

3. [[Quantum Phase Estimation on Blocks for QFT Parallelisation]] — Using inverse QFT on a block of the Fourier-transformed register to approximately recover the input value, enabling parallel phase rotations on neighbouring blocks. Fails near wraparound boundaries but produces small average error.

## References within this paper

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1997)]] — the factoring algorithm whose multiplier benefits from this QFT. [doi:10.1137/S0097539795293172](https://doi.org/10.1137/S0097539795293172)
- Cleve and Watrous (2000) — first $O(\log n)$-depth QFT, using $O(n \log n)$ ancillae. [doi:10.1109/SFCS.2000.892140](https://doi.org/10.1109/SFCS.2000.892140)
- Hales (2002) — improved the ancilla count to $O(n)$ while maintaining $O(\log n)$ depth. [arXiv:quant-ph/0212002](https://arxiv.org/abs/quant-ph/0212002)
- Coppersmith (1994/2002) — the approximate QFT via binary fraction truncation. [arXiv:quant-ph/0201067](https://arxiv.org/abs/quant-ph/0201067)
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes|Draper (2000)]] — QFT-based adder used as a subroutine. [arXiv:quant-ph/0008033](https://arxiv.org/abs/quant-ph/0008033)
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes|Häner, Roetteler, Svore (2017)]] — classical-quantum adder with Toffoli gates only; used as an alternative to QFT-based adder in the reduction. [QIC 17(7-8):673–684](https://doi.org/10.26421/QIC17.7-8-7)
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney (2018)]] — halving the T-cost of addition; phase gradient technique cited for practical QFT block implementation. [Quantum 2, 74](https://doi.org/10.22331/q-2018-06-18-74)
- Kahanamoku-Meyer and Yao (2024) — fast quantum integer multiplication with zero ancillae, the arithmetic construction that the optimistic QFT enables. [arXiv:2403.18006](https://arxiv.org/abs/2403.18006)
- Takahashi and Kunihiro (2008) — fast quantum addition circuit with few qubits; used for the CQ adder in the randomised reduction. [QIC 8(6):636–649]
- Takahashi, Tani, Kunihiro (2009) — quantum addition with unbounded fan-out; used for QQ adder in the derandomised reduction. [arXiv:0910.2530](https://arxiv.org/abs/0910.2530)
- Bäumer, Sutter, Woerner (2025) — concurrent work: approximate QFT on a line in $O(\log n)$ depth using measurements and $O(n)$ ancillae. [arXiv:2504.20832](https://arxiv.org/abs/2504.20832)
- Linden and de Wolf (2022) — average-case verification of the QFT; the reduction in this paper generalises that verification protocol. [Quantum 6, 872](https://doi.org/10.22331/q-2022-12-07-872)
- Beauregard (2003) — Shor's algorithm with $2n+3$ qubits using qubit recycling; the one-qubit-at-a-time QPE that makes the $2n + O(n/\log n)$ factoring qubit count possible.
- Fowler, Devitt, Hollenberg (2004) — nearest-neighbour implementation of QFT via SWAP insertion; used inside the local QFT blocks. [arXiv:quant-ph/0402196](https://arxiv.org/abs/quant-ph/0402196)
- Remaud and Vandaele (2025) — ancilla-free quantum adder with sublinear depth; alternative for reducing ancilla count in the reduction. [arXiv:2501.16802](https://arxiv.org/abs/2501.16802)

## Cross-links

### Paper notes
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — the factoring algorithm this QFT accelerates
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes]] — QFT-based adder used as subroutine
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — T-count optimisation; phase gradient technique applicable to local QFT blocks
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]] — Toffoli-based CQ adder alternative for the reduction
- [[A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) — Paper Notes]] — latest CQ adder; could replace Takahashi–Kunihiro in the reduction

### Trick cards
- [[Quantum Fourier Transform Circuit]] — the base QFT construction used inside blocks
- [[Phase Gradient State for Controlled Rotations]] — practical implementation of the local QFT blocks
- [[Optimistic Quantum Circuits]] — the framework formalised in this paper
- [[Worst-to-Average-Case Reduction for Optimistic Circuits]] — the generic reduction technique
- [[Quantum Phase Estimation on Blocks for QFT Parallelisation]] — the core parallelisation trick
- [[Order-Finding via QFT and Continued Fractions]] — the QPE step in Shor's algorithm where this QFT is used
- [[Dirty Ancilla Constant Addition]] — related CQ adder construction
- [[Carry Venting via X-Basis Measurement]] — alternative CQ adder technique
