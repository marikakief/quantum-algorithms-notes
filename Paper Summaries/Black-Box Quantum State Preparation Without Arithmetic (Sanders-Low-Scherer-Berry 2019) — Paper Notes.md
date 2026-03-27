> **Source:** Yuval R. Sanders, Guang Hao Low, Artur Scherer, and Dominic W. Berry, *Black-box quantum state preparation without arithmetic*, arXiv:1807.03206, Phys. Rev. Lett. **122**, 020502 (2019)
> **Links:** [arXiv](https://arxiv.org/abs/1807.03206) · [PRL](https://doi.org/10.1103/PhysRevLett.122.020502)
> **Tags:** #state-preparation #amplitude-loading #comparator #Toffoli-count #LCU #quantum-walk

---

## The computational problem

**Input:** An oracle `amp` returning $n$-bit approximations $\alpha_\ell^{(n)} = \lfloor 2^n \alpha_\ell \rfloor$ of target coefficients $\alpha_0, \ldots, \alpha_{d-1} \in [0,1)$.

**Output:** A quantum state approximating $|\text{target}\rangle = \frac{1}{|\vec{\alpha}|_2} \sum_\ell \alpha_\ell |\ell\rangle$ (linear coefficients) or $\frac{1}{|\sqrt{\vec{\alpha}}|_2} \sum_\ell \sqrt{\alpha_\ell} |\ell\rangle$ (root coefficients).

**Prior cost (Grover):** $n$ Toffoli gates per oracle call + cost of computing $\arcsin(\alpha_\ell^{(n)}/2^n)$ on a quantum computer, which is $O(n^2)$ Toffolis (or worse) per round of amplitude amplification.

**New cost (this paper):** $n$ Toffoli gates per round of amplitude amplification. Period. No arithmetic.

---

## What the paper does

Replaces the arcsine computation in Grover's black-box state preparation with an inequality test against a uniform superposition. The result: 286–375× fewer Toffoli gates at realistic precisions (17–30 bits), with the improvement factor growing with precision.

This matters because black-box state preparation is a core subroutine in [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|qubitization]], [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|LCU]], and quantum walk-based algorithms. Every time you need a PREPARE oracle — encoding coefficients $\alpha_\ell$ into amplitudes — you pay for state preparation. This paper makes that payment drastically cheaper.

---

## The algorithm

### Grover's original approach (what they improve on)

1. Query `amp` to load $\alpha_\ell^{(n)}$ into a data register
2. Compute $\theta_\ell \approx \arcsin(\alpha_\ell^{(n)}/2^n)$ — **expensive quantum arithmetic**
3. Apply rotation $\text{rot}|\xi\rangle|0\rangle = |\xi\rangle(\sin\theta|0\rangle + \cos\theta|1\rangle)$
4. Uncompute $\theta_\ell$
5. Amplitude amplification to boost the flagged component

The arcsine in step 2 dominates: $>11000$ Toffoli gates at 30-bit precision.

### The new approach: inequality test

Replace steps 2–4 with a comparator against a uniform superposition:

1. **Prepare uniform superposition** on a new $n$-qubit `ref` register: $H^{\otimes n}|0^n\rangle = \frac{1}{\sqrt{2^n}} \sum_{x=0}^{2^n-1} |x\rangle$

2. **Compare** `ref` against the oracle output: apply `comp`, which sets a flag qubit to $|0\rangle$ if $x < \alpha_\ell^{(n)}$, else $|1\rangle$

After comparison, the state is:

$$|\psi_{\text{comp}}\rangle = \frac{1}{\sqrt{2^n d}} \sum_\ell |l\rangle_{\text{out}} |\alpha_\ell^{(n)}\rangle_{\text{data}} \otimes \left(\sum_{x=0}^{\alpha_\ell^{(n)}-1} |x\rangle_{\text{ref}} |0\rangle_{\text{flag}} + \sum_{x=\alpha_\ell^{(n)}}^{2^n-1} |x\rangle_{\text{ref}} |1\rangle_{\text{flag}}\right)$$

3. **Unprepare** the `ref` register with $H^{\otimes n}$

This collapses the `ref` register. The amplitude on the $|0\rangle^{\otimes n}_{\text{ref}} |0\rangle_{\text{flag}}$ subspace is proportional to $\alpha_\ell^{(n)}/2^n \approx \alpha_\ell$ — exactly what we wanted.

4. **Amplitude amplification** to boost the flagged component, as before.

### Linear vs root coefficients

| Problem | Target state | Method |
|---|---|---|
| Linear coefficients | $\sum_\ell \alpha_\ell |\ell\rangle$ | Compare + unprepare `ref` + amplify |
| Root coefficients | $\sum_\ell \sqrt{\alpha_\ell} |\ell\rangle$ | Compare + keep `ref` entangled (skip unprepare) + amplify + unif$^{-1}$ at the end |

For root coefficients: the number of terms in the $|0\rangle_{\text{flag}}$ superposition is $\alpha_\ell^{(n)}$, so the amplitude on $|\ell\rangle_{\text{out}} |0\rangle_{\text{flag}}$ is proportional to $\sqrt{\alpha_\ell^{(n)}}$. Skip unpreparing `ref` during amplification; use a `unif`$^{-1}$ operation once at the end.

### Complex coefficients (polar and Cartesian forms)

- **Polar form:** Use magnitude oracle for amplitudes + argument oracle for phases. Controlled phase operations transduce the phases.
- **Cartesian form (linear):** Extend flag to 2 qubits. Condition on real/imaginary oracle. Use $Z$ gate for signs. Hadamard on the new flag qubit combines real and imaginary parts.
- **Cartesian form (root):** Cannot entirely avoid arithmetic — need to test $4x^2(x^2 \pm a_\ell) < b_\ell^2$. But still much cheaper than full arcsine.

---

## Key results

| Precision $n$ | comp Toffolis (this paper) | Arcsine Toffolis (Häner-Roetteler-Svore) | Improvement factor |
|---|---|---|---|
| 17 | 17 | 4872 | 286× |
| 23 | 23 | 7784 | 338× |
| 30 | 30 | 11264 | 375× |

The comparator costs exactly $n$ Toffoli gates. The arcsine costs $\Omega(n^2)$ with impractically large constants for advanced multiplication methods. The improvement grows with precision.

---

## Why this matters for [[Hamiltonian simulation]]

Black-box state preparation is the PREPARE oracle in:
- **[[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Qubitization]]** — preparing $\sum_\ell \sqrt{|\alpha_\ell|/\lambda} |\ell\rangle$ for LCU coefficients
- **[[LCU Origins (Childs-Wiebe 2012) — Paper Notes|LCU]]** — same
- **Quantum walk approaches** — discrete-time walks require state preparation for the coin operator

The paper cites Babbush et al. (2018, 1805.03662) as a key consumer: their fault-tolerant chemistry simulation uses this state preparation as a subroutine. Dropping the Toffoli count by 2–3 orders of magnitude directly impacts the total resource estimates.

---

## Limits / caveats

- **Oracle model.** The $n$-Toffoli count is per oracle call. If preparing the oracle output itself is expensive (e.g., QROM lookup), that cost adds on top.
- **Root coefficients need `unif`$^{-1}$.** Inverting the uniform superposition conditioned on the coefficient value requires $O(n^2)$ Toffolis for the most straightforward implementation. This is paid once (not per amplification round), so it doesn't dominate, but it's not free.
- **Cartesian root coefficients still need some arithmetic.** The $4x^2(x^2 \pm a)$ test requires a few multiplications — much cheaper than arcsine, but not arithmetic-free.
- **Fixed-point truncation error.** The prepared state matches $\alpha_\ell^{(n)} = \lfloor 2^n \alpha_\ell \rfloor$, not $\alpha_\ell$ exactly. Error is $O(2^{-n})$ — choose $n$ to match overall algorithm precision.

---

## Reusable ideas

1. **[[Inequality-Test Amplitude Transduction]]** — Replace arcsine-based rotation with a comparator against a uniform superposition. The number of terms passing the inequality is proportional to the coefficient value, so the amplitude is proportional to the coefficient (linear) or its square root (root). Cost: $n$ Toffoli gates for $n$-bit precision, independent of the number of states $d$.

---

## References within this paper

- [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes|Grover & Rudolph (2002)]] — state preparation for structured (integrable) distributions; this paper's black-box approach handles the complementary unstructured case
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] — amplitude amplification
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low-Chuang (2019)]] — qubitization uses state preparation as PREPARE oracle
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]] — QSVT uses PREPARE
- Babbush et al. (2018, 1805.03662) — fault-tolerant chemistry simulation consuming this state preparation
- Häner-Roetteler-Svore (2018, 1805.12445) — optimised quantum arithmetic; Table II provides the arcsine Toffoli counts
- Gidney (2018) — Toffoli gate constructions
- [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes|Berry-Kieferová-Scherer-Sanders-Low-Wiebe-Gidney-Babbush (2018)]] — shares three authors; uses state preparation for eigenstate preparation in chemistry

---

## Cross-links

### Paper notes
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — primary consumer of this state preparation primitive
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]] — LCU framework requires PREPARE oracle
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — QSVT block encodings use PREPARE
- [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes]] — same research group; shares three authors
- [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes]] — later work using comparator-based amplitude loading (SELECT-SWAP)
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] — amplitude amplification subroutine

### Trick cards
- [[Inequality-Test Amplitude Transduction]] — the main trick from this paper
- [[Comparator-Based Direct Amplitude Sampling from Fixed-Point Coefficients]] — later generalisation of the same idea
- [[Standard Amplitude Amplification]] — used for boosting success probability
