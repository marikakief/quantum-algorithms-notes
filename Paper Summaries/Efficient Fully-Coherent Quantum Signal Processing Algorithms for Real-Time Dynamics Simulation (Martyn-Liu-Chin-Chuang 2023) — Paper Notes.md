> **Source:** John M. Martyn, Yuan Liu, Zachary E. Chin, and Isaac L. Chuang, *Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation*, J. Chem. Phys. **158**, 024106 (2023)
> **Links:** [arXiv:2110.11327](https://arxiv.org/abs/2110.11327) · [JCP](https://doi.org/10.1063/5.0124385)
> **Tags:** #hamiltonian-simulation #QSP #QET #QSVT #block-encoding #fully-coherent #one-shot #chemistry #time-dependent
> **MIT report:** MIT-CTP/5334

---

## The computational problem

Simulate $e^{-iHt}$ to error $\epsilon$ with success probability at least $1-\delta$, where the simulation subroutine must be **fully coherent**: it cannot discard the ancilla register via postselection, because it will be embedded inside a larger coherent algorithm or a time-dependent simulation scheme that needs to concatenate the subroutine with other operations.

"Fully coherent" is a precision-of-interface issue, not an asymptotic-theory issue. If you're running [[Hamiltonian simulation]] as a standalone query, postselection is fine. But if you're feeding the output into another block-encoding, or using simulation as a subroutine in a time-dependent scheme (e.g., interaction-picture or Dyson-series based algorithms), you need the failure branch to not contaminate subsequent operations. That makes the problem non-trivial.

The relevant access model is a block-encoding oracle for $H/\alpha$, where $\alpha \ge \|H\|$.

---

## What the paper does

The paper gives a clean comparison of three QSP-based constructions for fully-coherent simulation, two of which are prior art (tidied up) and one of which is genuinely new.

**My assessment:** This is a good, clean technical paper. It's not a foundational leap on the level of [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low-Chuang]] or [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|GSLW]] — those papers created frameworks. This paper solves a specific interface problem (full coherence) using those frameworks, and the one-shot algorithm is a clever new construction. The paper also serves as a bridge to the chemistry community, which is why it's in JCP and has explicit numerics on H₂.

The three constructions studied:

1. **QSP-LCU + conventional amplitude amplification (AA):** Use [[Jacobi-Anger Truncation for QSP Simulation|QSP/QET]] to block-encode $\cos(Ht)$ and $-i\sin(Ht)$ separately, combine via [[Linear Combination of Unitaries (LCU)|LCU]] to get $e^{-iHt}$, then boost success via standard amplitude amplification.

2. **QSP-LCU + robust oblivious amplitude amplification (ROAA):** Same LCU construction but using the $A = -WRW^\dagger RW$ style robust OAA rather than standard AA. This handles the fact that the success amplitude is only approximately known and avoids a multiplicative $\ln(1/\delta)$ overhead.

3. **Coherent one-shot [[Hamiltonian simulation]] (new):** Apply an affine pre-transformation to the Hamiltonian spectrum, then approximate the complex exponential only on the compressed (one-sided) interval. No amplitude amplification needed at all.

---

## The algorithm / construction

### Conventional QSP-LCU simulation (baseline)

Use QET (see [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes|Grand Unification]]) to implement:
- $\cos(H/\alpha \cdot \alpha t) = \cos(Ht)$ as a block-encoded polynomial
- $-i\sin(Ht)$ via a separate QSP sequence

Combine via [[Linear Combination of Unitaries (LCU)|LCU]] using two ancilla qubits. The construction is standard via Jacobi–Anger expansion; see [[Jacobi-Anger Truncation for QSP Simulation]].

Postselection (both ancillas in $|0\rangle$) succeeds with probability $\approx 1/4$ in the worst case. Fully coherent? No, not as stated — if you're inside a larger algorithm, you cannot postselect. This is the baseline QSP-LCU approach, not yet fully coherent.

### Variant 1: QSP-LCU + conventional AA (Theorem 2)

Run standard amplitude amplification on top of the QSP-LCU circuit. Requires $O(\ln(1/\delta))$ rounds. The success amplitude from QSP-LCU is approximately known (it's close to $1/2$), so the amplification round count can be chosen appropriately.

**Theorem 2:** Query complexity
$$\Theta\!\left(\ln(1/\delta)\left[\alpha |t| + \frac{\ln(1/\epsilon)}{\ln\!\bigl(e+\ln(1/\epsilon)/(\alpha |t|)\bigr)}\right]\right).$$

The $\ln(1/\delta)$ factor is multiplicative — you pay a factor for each halving of failure probability.

### Variant 2: QSP-LCU + ROAA (Theorem 3)

Use the robust oblivious AA construction from [[Oblivious Amplitude Amplification (Robust)|OAA (robust)]] — specifically $A = -WRW^\dagger RW$ — which tolerates the success amplitude being only approximately known. Fix $\delta = 2\epsilon$ (failure probability is controlled by the approximation error, so no separate $\delta$-parameter needed).

**Theorem 3:** Query complexity
$$\Theta\!\left(\alpha |t| + \frac{\ln(1/\epsilon)}{\ln\!\bigl(e+\ln(1/\epsilon)/(\alpha |t|)\bigr)}\right).$$

The multiplicative $\ln(1/\delta)$ factor is gone — this achieves $\delta = \Theta(\epsilon)$ without overhead. But the precision scaling still has the log-log correction versus the ideal $\ln(1/\epsilon)$.

### Main construction: Coherent one-shot simulation (Theorem 5)

This is the new result. The key issue to understand first: why can't you just directly apply QSP to approximate $e^{-ix\tau}$ on $[-1,1]$?

**The parity obstruction:** QSP requires the target polynomial to have definite parity (even or odd). The function $e^{-ix\tau} = \cos(x\tau) - i\sin(x\tau)$ mixes even and odd parts — the real part is even, the imaginary part is odd. This prevents direct QSP approximation on all of $[-1,1]$.

**The fix:** Shift the spectrum so it's one-sided.

**Step 1 — Affine pre-transformation (Lemma 4 + Figs. 4–5):**

Given a block-encoding of $H/\alpha$ (spectrum in $[-1,1]$), construct a block-encoding of
$$\tilde{H} = \tfrac{1}{2}(I + \beta H/\alpha),$$
with spectrum in $[(1-\beta)/2, (1+\beta)/2] \subset [0,1]$.

The trick uses Lemma 4 (block-encoding composition): if $V_A$ block-encodes $A$ and $V_B$ block-encodes $B$, then $(I_B \otimes V_A)(V_B \otimes I_A)$ block-encodes $AB$. This is the [[Block-Encoding Composition Algebra|block-encoding composition lemma]]. The $I/2$ shift can be implemented via an additional ancilla circuit (Fig. 5). See [[Affine Spectrum Compression for QSP Parity Bypass]] for the complete recipe.

**Step 2 — Restricted-interval polynomial approximation:**

Because the spectrum of $\tilde{H}$ lies in $[(1-\beta)/2,(1+\beta)/2]$ — strictly positive and away from zero — we can approximate the **even extension of the complex exponential (EECE)**:
$$\mathrm{EECE}(x;\tau) = \cos(\tau x) - i\sin(\tau x)\,\mathrm{sign}(x)$$
only on the restricted interval. On this interval, $\mathrm{sign}(x) = +1$, so $\mathrm{EECE}(x;\tau) = e^{-ix\tau}$ restricted to the spectrum we care about. And crucially, $\mathrm{EECE}(x;\tau)$ is an **even function**, so it has definite parity — no parity obstruction.

**Step 3 — QET on the pre-transformed Hamiltonian (Fig. 6):**

Apply one QET sequence on $\tilde{H}$ using the EECE polynomial. This directly implements $e^{-i\tilde{H}\tau}$ restricted to the relevant spectral interval, which equals $e^{-iHt}$ after appropriate parameter matching.

No amplitude amplification. The circuit is a single-block QET sequence. Success probability $\ge 1 - 2\epsilon$.

**Theorem 5 (Coherent One-Shot Hamiltonian Simulation):** Query complexity
$$\Theta\!\left(\alpha|t| + \ln(1/\epsilon) + \ln(1/\delta)\right).$$

This is **additive** in $\ln(1/\delta)$, not multiplicative. Tuning $\delta = \Theta(\epsilon)$ makes both terms comparable.

### Numerics / applications (Section V)

- **Heisenberg model:** time-independent and time-dependent onsite fields. The paper demonstrates the circuit explicitly at small system sizes.
- **H₂ electronic dynamics:** map the electronic Hamiltonian of hydrogen molecule to a spin Hamiltonian (Appendix D gives the Pauli decomposition), simulate unitary dynamics starting from a product state, track observables. The chemistry motivation is that coherent simulation is needed when the dynamics subroutine appears inside a larger coherent algorithm (e.g., coupled dynamics, feedback-based state preparation).

---

## Key results

| Algorithm | Coherent? | Query complexity |
|---|---|---|
| Plain QSP-LCU (no AA) | ✗ (postselection) | $\Theta(\alpha|t| + \ln(1/\epsilon) / \ln(\cdot))$ per successful shot |
| QSP-LCU + conventional AA (Thm 2) | ✓ | $\Theta\!\left(\ln(1/\delta)\left[\alpha|t| + \frac{\ln(1/\epsilon)}{\ln(e + \ln(1/\epsilon)/(\alpha|t|))}\right]\right)$ |
| QSP-LCU + ROAA (Thm 3) | ✓ | $\Theta\!\left(\alpha|t| + \frac{\ln(1/\epsilon)}{\ln(e + \ln(1/\epsilon)/(\alpha|t|))}\right)$, $\delta = 2\epsilon$ |
| Coherent one-shot (Thm 5) | ✓ | $\Theta\!\left(\alpha|t| + \ln(1/\epsilon) + \ln(1/\delta)\right)$ |

The one-shot construction wins on all three counts: additive $\ln(1/\delta)$, clean $\ln(1/\epsilon)$ precision dependence, and no AA overhead.

---

## Comparison with prior work

| Method | Coherent? | Time scaling | Precision | Notes |
|---|---|---|---|---|
| [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes\|Taylor-LCU + ROAA]] | ✓ | $O(\alpha|t|)$ | $O(\log/\log\log)$ | Robust OAA for coherence; log-log correction |
| [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes\|Low-Chuang QSP]] | Partial | $O(\alpha|t|)$ | $O(\log/\log\log)$ | Not explicitly constructed for full coherence |
| [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes\|Qubitization]] | ✓ | $O(\alpha|t|)$ | $O(\log(1/\epsilon))$ | Block-encoding framework; coherence requires care |
| This paper (one-shot) | ✓ | $O(\alpha|t|)$ | $O(\log(1/\epsilon) + \log(1/\delta))$ | No AA; parity bypass; clean separation of $\epsilon$, $\delta$ |

The main improvement over ROAA-based approaches: clean $\ln(1/\epsilon)$ (no log-log correction) and independent $\delta$ control without multiplicative overhead.

---

## Limits / caveats

- The one-shot construction requires $\beta < 1$ (compression parameter) — in practice $\beta$ is close to 1, but the polynomial degree grows as the spectral interval approaches zero width. There's no free lunch from compression if $H$ is badly conditioned.
- The paper assumes a clean block-encoding oracle. In chemistry, constructing $\alpha$ and the block-encoding efficiently is still a significant engineering task (cf. [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes|sublinear-T block-encodings]]).
- The numerics are small-system demonstrations. The paper doesn't claim resource estimates competitive with near-term methods; it's making a point about the algorithmic structure, not gate counts.
- The JCP venue signals that the audience is chemistry-forward. The algorithm sections (I–IV) are rigorous, but the paper also devotes substantial space to the Heisenberg and H₂ demonstrations, which are more illustrative than pushing boundaries.

---

## Reusable ideas

New trick cards created for this paper:

- [[Affine Spectrum Compression for QSP Parity Bypass]] — shift and rescale the block-encoded spectrum into a one-sided interval, enabling approximation of parity-incompatible target functions on the restricted interval.
- [[Fully-Coherent One-Shot Hamiltonian Simulation]] — complete recipe: pre-transformation → EECE polynomial approximation → single QET sequence. No amplitude amplification.
- [[Postselection Removal via Approximate Success-Amplitude Amplification]] — comparison of conventional AA and ROAA when the success amplitude is approximately known.

Existing trick cards directly used or illustrated:

- [[Jacobi-Anger Truncation for QSP Simulation]] — underlies the QSP-LCU baseline
- [[Oblivious Amplitude Amplification (Robust)]] — the ROAA construction (Theorem 3)
- [[Block-Encoding Composition Algebra]] — Lemma 4; used to build the affine pre-transformation
- [[Linear Combination of Unitaries (LCU)]] — the LCU circuit combining cosine and sine block-encodings
- [[QSVT Meta-Template]] — the one-shot algorithm is an instance of QET with a carefully chosen polynomial
- [[Phased Qubitization Sequence]] — structural relative of the QET sequence used in Fig. 6

---

## References within this paper

- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes|Martyn-Rossi-Tan-Chuang (2021)]] — QET framework and QSP tutorial; pedagogical foundation
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low-Chuang (2016–2017)]] — original QSP-based [[Hamiltonian simulation]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low-Chuang (2019)]] — qubitization and block-encoding framework
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén-Su-Low-Wiebe (2019)]] — QSVT unification; QET is the Hermitian-case restriction
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Berry-Childs-Cleve-Kothari-Somma (2015)]] — Taylor-LCU + ROAA; precursor to the ROAA variant here
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs-Wiebe (2012)]] — LCU foundation

---

## Cross-links

- [[Hamiltonian Simulation — Comparison Tables]]
- [[Paper Index]]
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]]
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]]
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Affine Spectrum Compression for QSP Parity Bypass]]
- [[Fully-Coherent One-Shot Hamiltonian Simulation]]
- [[Postselection Removal via Approximate Success-Amplitude Amplification]]
- [[Jacobi-Anger Truncation for QSP Simulation]]
- [[Oblivious Amplitude Amplification (Robust)]]
- [[Block-Encoding Composition Algebra]]
- [[Linear Combination of Unitaries (LCU)]]
- [[QSVT Meta-Template]]
- [[Phased Qubitization Sequence]]
- [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes]] — same first author; takes the complementary approach: sacrifices coherence (outputs a channel) to halve query complexity. Direct contrast: that paper optimizes for query count, this paper optimizes for coherent output.
- [[Doubling the Efficiency of Hamiltonian Simulation via Generalized Quantum Signal Processing (Berry-Motlagh-Pantaleoni-Wiebe 2024) — Paper Notes]] — alternative approach to the same parity obstruction: uses [[Parity Shifting via Spectral Multiplication in GQSP|spectral multiplication]] + [[Generalized Quantum Signal Processing (GQSP)|GQSP]] rather than [[Affine Spectrum Compression for QSP Parity Bypass|affine compression]]; achieves $2\times$ query reduction
