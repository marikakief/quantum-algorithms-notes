> **Source:** Iain Burge, Michel Barbeau, and Joaquin Garcia-Alfaro, *Quantum CORDIC — Arcsin on a Budget*, arXiv:2411.14434 (2024)
> **Links:** [arXiv](https://arxiv.org/abs/2411.14434) · [GitHub](https://github.com/iain-burge/QuantumCORDIC)
> **Tags:** #quantum-arithmetic #arcsine #CORDIC #digital-to-analog #state-preparation #HHL #reversible-computation

---

## The computational problem

**Quantum digital-to-amplitude conversion:** Given an $n$-qubit register encoding a value $h_k \in [0,1]$ in superposition, produce the transformation

$$U|\phi\rangle = \sum_k \alpha_k |h_k\rangle_{\text{in}} \left(\sqrt{1-h_k}\,|0\rangle + \sqrt{h_k}\,|1\rangle\right)_{\text{out}}$$

This encodes computational-basis values into probability amplitudes of an output qubit. The core difficulty reduces to computing $\arcsin(\sqrt{h_k})$ reversibly — once you have the angle, a sequence of controlled $R_y$ rotations transfers it to the output qubit's amplitude.

## What the paper does

Adapts the classical CORDIC algorithm — a staple of FPGA and embedded computing since the 1950s — to work reversibly in a quantum circuit. CORDIC computes trig functions using only bit shifts and additions, no multiplications. The quantum version achieves:

- **$O(n)$ qubits** ($5n - 1$ minimum)
- **$O(n \log n)$ circuit depth** (layer count)
- **$O(n^2)$ CNOT count**

for $n$ bits of precision. This gives a direct quantum implementation of arcsine that's cheaper than prior approaches (piecewise polynomial [Häner-Roetteler-Svore 2018] or binary search [Wang et al. 2020]), though still much more expensive than the [[Inequality-Test Amplitude Transduction|inequality-test trick]] of [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes|Sanders-Low-Scherer-Berry (2019)]] when arcsine can be avoided entirely.

## The algorithm

### Classical CORDIC for arcsine (background)

CORDIC works by iteratively rotating a 2D vector $(x_i, y_i)$ toward a goal direction. For arcsine of input $t \in [-1, 1]$, the goal vector is $(\sqrt{1-t^2}, t)$, which has angle $\theta = \arcsin(t)$.

Each iteration:

1. **Direction decision:** Compute $d_i$ — a Boolean indicating whether to rotate clockwise or counterclockwise — using only sign bits of $x_i$, $y_i$, and $t_i - y_i$. The formula is $d_i = [s(x_i) \wedge s(t_i - y_i)] \oplus s(x_i) \oplus [s(x_i) \wedge s(y_i)] \oplus s(t_i - y_i)$.
2. **Conditional swap:** If $d_i = 1$, swap $x_i$ and $y_i$.
3. **Pseudo-rotation:** Apply $(x, y) \mapsto (x - 2^{-i}y,\; y + 2^{-i}x)$ twice. This rotates by $\approx 2\arctan(2^{-i})$ but also stretches by a factor of $(1 + 4^{-i})$.
4. **Un-swap:** If $d_i = 1$, swap back.
5. **Stretch compensation:** Multiply the target $t$ by $(1 + 2^{-2i})$ to match the accumulated stretch.
6. **Angle update:** $\theta_{i+1} = \theta_i + (-1)^{d_i} \cdot 2\arctan(2^{-i})$.

After $n$ iterations, $\theta_n$ approximates $\arcsin(t)$ to $n$ bits. The key property: each iteration uses only $\sim 5$ bit-shifts and $\sim 7$ additions. No multiplications.

### Making CORDIC reversible (the quantum version)

The paper addresses three challenges in lifting CORDIC to a quantum circuit:

**Step 1 — Direction bit $d_i$:** Computed reversibly via one subtraction ($t \leftarrow t - y$), two Toffoli gates and two CNOTs on sign bits, then uncomputing the subtraction ($t \leftarrow t + y$). The $d_i$ bits are stored in an $(n-1)$-qubit register and never erased during the forward pass.

**Step 3 — Pseudo-rotation without multiplication:** The subtraction $x \leftarrow x - 2^{-i}y$ is just an addition of a right-shifted register. But multiplication by $(1 + 2^{-2i})$ in Step 5 is non-trivial. The paper introduces a [[Fibonacci-Accelerated Reversible Multiplication|Fibonacci-accelerated multiplication]] routine (Algorithm 2, `Mult`):

- To compute $z \mapsto (1 + 2^{-m})z$ reversibly using only additions and right-shifts
- Uses a Fibonacci-indexed sequence of shifted additions/subtractions
- The inverse (`Div`, Algorithm 3) computes $(1 + 2^{-m})^{-1}z$ and converges exponentially fast — after $J$ iterations, the error is $O(2^{-m\phi^{J-1}})$ where $\phi$ is the golden ratio
- At iteration $i$ of CORDIC, `Mult` is called with $m = 2i$, so it needs only $\lfloor\sqrt{5}\phi^{-2i}n\rfloor + 2$ additions, which decreases rapidly with $i$

**Step 6 — Angle update:** Uses controlled negation of the angle register (via bitwise NOT controlled on $d_i$), then adds the precomputed constant $2\arctan(2^{-i})$, then un-negates.

**Representation:** All values use $n$-bit fixed-point two's complement in $[-2, 2)$. This works because $|x_i|, |y_i|, |t_i| \leq \prod_{i=1}^n (1 + 4^{-i}) < \sqrt[3]{e} < 2$.

### Full digital-to-amplitude conversion

The paper uses the identity $\arcsin(\sqrt{x}) = \frac{\arcsin(2x-1)}{2} + \frac{\pi}{4}$ to avoid computing square roots. The full pipeline for converting $h_k \in [0,1]$ to amplitudes:

1. Transform $t \leftarrow 2h_k - 1$ so $t \in [-1, 1)$
2. Run CORDIC (without the angle register), storing only the direction bits $d_1, \ldots, d_{n-1}$
3. Uncompute the $x, y, \text{mult}$ garbage by running CORDIC steps in reverse (skipping swaps and angle updates)
4. Apply controlled rotations: for each $j$, apply $R_y(\pm 2\arctan(2^{-j}))$ controlled on $d_j$, plus a final $R_y(\pi/4)$
5. Clean up $d$ register by re-running and un-running CORDIC

The direction bits $d_j$ encode the arcsine in a "unary-angle" representation — each bit tells which way to rotate at scale $2^{-j}$.

## Key results

| Metric | Complexity | Notes |
|---|---|---|
| Qubits | $5n - 1$ | Registers: $t, x, y, \text{mult}$ ($n$ each), $d$ ($n-1$), out ($1$) |
| Additions | $< 14n$ | Per run of CORDIC |
| Layer count | $O(n \log n)$ | Using parallel addition with $O(n)$ ancillas |
| CNOT count | $O(n^2)$ | $n$ additions × $O(n)$ CNOTs each |
| Precision | $n$ bits | Arbitrary, confirmed by simulation |

## Comparison with prior work

| Method | Approach | Gate cost | Qubits | Notes |
|---|---|---|---|---|
| **Häner-Roetteler-Svore (2018)** | Piecewise polynomial approx. | $>4800$ Toffoli ($n=17$) | More | Needs multiplications, square roots for $|x| > 0.5$ |
| **Wang et al. (2020)** | Binary search | Many squarings | More | Very general but expensive |
| **This paper (CORDIC)** | Iterative pseudo-rotations | $O(n^2)$ CNOT | $5n - 1$ | Only shifts + additions |
| **[[Inequality-Test Amplitude Transduction\|Inequality test]]** | Comparator trick | $n$ Toffoli | $2n + 1$ | **Avoids arcsine entirely** |

The Sanders-Low-Scherer-Berry comparator wins overwhelmingly when the goal is amplitude loading from fixed-point coefficients — $n$ Toffolis vs. $O(n^2)$ CNOTs. But the comparator trick only works for the specific problem of encoding a stored value into an amplitude. CORDIC computes the actual arcsine function, which is needed when:
- The angle itself must be computed (not just used for amplitude transduction)
- You need arcsine as an intermediate step in a larger reversible computation
- The input isn't a simple fixed-point coefficient (e.g., eigenvalue reciprocals in [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]])

## Applications

**[[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL algorithm]]:** The eigenvalue inversion step requires encoding $\lambda_j^{-1}$ into amplitudes via $R_y(2\arcsin(C/\lambda_j))$. If computing this rotation with explicit arcsine, CORDIC is cheaper than polynomial approximation. (Though modern implementations often bypass arcsine via [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] or similar.)

**[[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|Quantum Monte Carlo]]:** Montanaro's quantum speedup needs a transformation $|x\rangle|0\rangle \mapsto |x\rangle(\sqrt{1-\Phi(x)}|0\rangle + \sqrt{\Phi(x)}|1\rangle)$. The paper shows how to compose CORDIC with a reversible implementation of $\Phi$ to achieve this.

**Shapley value estimation:** The authors' earlier work (Burge-Barbeau-Garcia-Alfaro 2023) uses this transformation for quantum algorithms approximating Shapley values in cooperative game theory.

## Limits / caveats

- **The inequality-test trick is almost always better for amplitude loading.** If you just need to prepare a quantum state with amplitudes proportional to stored values, [[Inequality-Test Amplitude Transduction|the comparator approach]] costs $n$ Toffolis vs. CORDIC's $O(n^2)$ CNOTs. CORDIC is only preferable when you genuinely need the arcsine value in a register.
- **Low-precision regime.** CORDIC is best suited for moderate $n$. For high precision ($n > 32$), polynomial methods like Häner et al. may scale better asymptotically. The authors acknowledge this.
- **No formal lower bound comparison.** The paper doesn't compare against information-theoretic lower bounds for reversible arcsine computation. It's unclear how close to optimal this is.
- **Constant factors.** The $5n - 1$ qubit count is non-trivial. For near-term devices, this is a real constraint. The paper is targeting fault-tolerant regimes.
- **Simulation only.** Results are from classical simulations of the quantum circuit, not hardware experiments.
- **Modern alternatives for HHL.** The original HHL algorithm's arcsine step has been superseded in many contexts by [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]]-based approaches that implement the eigenvalue inversion as a polynomial transformation, avoiding arcsine entirely.

## Reusable ideas

1. **[[Fibonacci-Accelerated Reversible Multiplication]]** — Reversible multiplication by $(1 + 2^{-m})$ using only additions and right-shifts, with Fibonacci-indexed convergence achieving $O(2^{-m\phi^{J-1}})$ error after $J$ steps. The key insight: the geometric series $\sum_{k=0}^{K} (-2^{-m})^k$ computes $(1+2^{-m})^{-1}$, and by cleverly ordering the partial sums along Fibonacci indices, the accumulation is reversible with exponentially fast convergence.

2. **[[CORDIC as Reversible Angle Computation]]** — The direction-bit encoding of CORDIC: instead of computing $\arcsin(t)$ as a single number, extract the $n$ binary direction decisions $d_1, \ldots, d_{n-1}$ that encode the angle as $\sum_j (-1)^{d_j} \arctan(2^{-j})$. These bits can drive controlled rotations directly, avoiding the need to store or manipulate the angle as a fixed-point number. This "implicit angle" representation is naturally suited to quantum circuits.

## References within this paper

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow-Hassidim-Lloyd (2009)]] — HHL, primary application for arcsine
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|Montanaro (2015)]] — quantum Monte Carlo speedup, another consumer of amplitude loading
- Häner-Roetteler-Svore (2018, arXiv:1805.12445) — piecewise polynomial quantum arithmetic for arcsine; main comparison point
- Mitarai-Kitagawa-Fujii (2019) — quantum analog-digital conversion; formalises the problem this paper solves
- Wang et al. (2020) — binary search method for transcendental functions on quantum circuits
- Mazenc-Merrheim-Muller (1993) — classical CORDIC arcsine algorithm that this paper adapts
- Volder (1959) — original CORDIC algorithm
- Walther (1971) — generalised CORDIC to unified elementary function algorithm
- Takahashi-Tani-Kunihiro (2009) — parallel quantum addition circuits, used for depth analysis
- Burge-Barbeau-Garcia-Alfaro (2023) — authors' prior work on quantum Shapley value estimation

## Cross-links

### Paper notes
- [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes]] — the key comparison: when you can avoid arcsine entirely
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — HHL uses arcsine rotation
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]] — quantum Monte Carlo needs amplitude loading
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — modern alternative that avoids arcsine in HHL-type algorithms
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] — amplitude estimation context

### Trick cards
- [[Inequality-Test Amplitude Transduction]] — the comparator alternative that avoids arcsine (almost always preferable for amplitude loading)
- [[Fibonacci-Accelerated Reversible Multiplication]] — reversible $(1+2^{-m})$ multiplication extracted from this paper
- [[CORDIC as Reversible Angle Computation]] — direction-bit angle encoding extracted from this paper
