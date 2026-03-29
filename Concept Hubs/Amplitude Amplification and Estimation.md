**Concept hub** — 42+ wikilink refs across the vault.

---

## The core idea

Amplitude amplification converts a quantum algorithm with success probability $a$ into one with success probability $\sim 1$, using $O(1/\sqrt{a})$ repetitions. Amplitude estimation extracts the value of $a$ to precision $\varepsilon$ in $O(1/\varepsilon)$ queries. Both exploit the same geometric structure: the Grover operator $Q = -AS_0 A^{-1} S_\chi$ is a rotation by $2\theta_a$ (where $\sin\theta_a = \sqrt{a}$) in a 2D subspace.

These are among the most widely used primitives in quantum algorithms. Nearly every quantum speedup for search, sampling, or estimation has amplitude amplification somewhere inside.

---

## The family of amplification variants

Over the years, several variants have emerged, each solving a different operational problem. They share the core rotation-in-a-2D-subspace structure, but differ in what they assume and what they guarantee.

### 1. Standard amplitude amplification

> **Reference:** [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard, Høyer, Mosca & Tapp (2002)]]
> **Trick card:** [[Standard Amplitude Amplification]]

The textbook version. Given $A|0\rangle$ with success amplitude $\sqrt{a}$, construct $Q = -AS_0A^{-1}S_\chi$ and iterate $m \approx \pi/(4\theta_a)$ times. Success probability goes to $\sim 1$ after $O(1/\sqrt{a})$ applications of $A$.

**Requires:** Access to $A^{-1}$, the ability to reflect about good states ($S_\chi$), and knowledge of $a$ (or use the randomised schedule below). The reflection $S_0$ is about the *initial state* $A|0\rangle$ — so you need to know what state you started from.

**Limitation:** If $a$ is unknown, the iteration count can overshoot and *reduce* success probability. Also, the reflection about $A|0\rangle$ means you can't use this when amplifying a subroutine that must work for arbitrary inputs.

### 2. Oblivious amplitude amplification (OAA)

> **Reference:** [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|Berry, Childs, Cleve, Kothari & Somma (2014)]]
> **Trick card:** [[Oblivious Amplitude Amplification (Robust)]]

The key innovation: the reflection is about the *ancilla register* being $|0^\mu\rangle$, not about the full initial state. This makes the amplification work for **any** input state simultaneously — hence "oblivious."

Given $U|0^\mu\rangle|\psi\rangle = \sin\theta\,|0^\mu\rangle V|\psi\rangle + \cos\theta\,|\Phi^\perp\rangle$ where $\sin^2\theta$ is the same for *every* $|\psi\rangle$:

$$S^\ell U|0^\mu\rangle|\psi\rangle = \sin((2\ell+1)\theta)|0^\mu\rangle V|\psi\rangle + \ldots$$

where $S = -URU^\dagger R$ and $R = 2(|0^\mu\rangle\langle 0^\mu| \otimes I) - I$.

**The $p = 1/4$ sweet spot:** When $\sin^2\theta = 1/4$ (common in [[Linear Combination of Unitaries (LCU)|LCU]] constructions), a single round ($\ell = 1$) gives $\sin(3 \cdot \pi/6) = 1$ — exact amplification with just 3 queries.

**Why it works:** The 2D subspace lemma. Because $\sin^2\theta$ is the same for every $|\psi\rangle$, the operator $Q = (\langle 0^\mu| \otimes I)U^\dagger \Pi U(|0^\mu\rangle \otimes I) = \sin^2\theta \cdot I$, forcing the "orthogonal partner" state to live entirely in the $\Pi = 0$ subspace. The reflection $R$ then has the right eigenstructure.

**When to use:** After any LCU or block-encoding construction. This is the standard workhorse for [[Hamiltonian simulation]], [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|quantum linear systems]], and any algorithm that implements a target operation via block-encoding.

**Robust version:** When $U$ is only approximately unitary ($\|U - U_{\text{ideal}}\| \le \delta$), the robust OAA construction $A = -WRW^\dagger RW$ still works, with error *linear* in $\delta$. Introduced in [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Berry et al. (2015)]].

**Connection to QSVT:** OAA is an early instance of [[QSVT Meta-Template|quantum singular value transformation]]. The composition $S^\ell U$ implements $\sin\theta \to \sin((2\ell+1)\theta)$, which is a Chebyshev polynomial of the singular values.

### 3. Fixed-point amplitude amplification

> **Reference:** Grover (2005), Yoder, Low & Chuang (2014)
> **Trick card:** [[Fixed-Point Amplitude Amplification with Eigenstate Filtering]]

Standard AA has a nasty property: if you run too many iterations, the success probability *drops* back toward zero. The success probability oscillates as $\sin^2((2m+1)\theta)$. If $a$ is unknown, this is dangerous.

Fixed-point AA replaces the Chebyshev-like iteration with a polynomial that **monotonically** approaches 1 for all $\theta$ above a threshold $\theta_{\min}$. No overshoot, no oscillation.

**How:** Instead of uniform rotations, use a sequence of Grover-like iterations with *varying* phase angles. The resulting transformation implements a polynomial $p(\sin^2\theta)$ that is $\ge 1 - \varepsilon$ for all $\sin^2\theta \ge \gamma_{\min}$ and $\le \varepsilon$ for $\sin^2\theta \le$ some lower cutoff.

The polynomial is constructed via the [[QSVT Meta-Template|QSP/QSVT framework]] — Yoder, Low & Chuang (2014) showed that fixed-point AA is equivalent to designing a specific QSP phase sequence.

**Cost:** $O(\log(1/\varepsilon)/\sqrt{\gamma_{\min}})$ iterations. The extra $\log(1/\varepsilon)$ compared to standard AA is the price of monotonicity.

**When to use:** When the success amplitude varies across a superposition and you need uniform amplification. Common in eigenstate filtering, ground state preparation, and any setting where different branches of a quantum computation have different (but bounded-below) success probabilities.

**Key difference from standard AA:** Standard AA is optimal ($O(1/\sqrt{a})$) when $a$ is known. Fixed-point AA pays a $\log(1/\varepsilon)$ overhead but is robust to unknown or varying $a$.

### 4. Variable-time amplitude amplification (VTAA)

> **Reference:** [[Variable-Time Amplification for Condition-Number Reduction|Ambainis (2010)]]
> **Trick card:** [[Variable-Time Amplification for Condition-Number Reduction]]

The idea: different branches of a quantum computation take different amounts of time to succeed. Rather than running everyone for the worst-case time, let fast branches finish early and amplify globally.

**The setup:** You have a subroutine that, for eigenvalue $\lambda$, takes time $O(1/|\lambda|)$ to produce a good state. Running everything for the worst case (smallest $|\lambda|$, i.e., condition number $\kappa$) costs $O(\kappa)$ per query. With VTAA, you partition the spectrum into $O(\log\kappa)$ dyadic buckets, run each for the appropriate time, and amplify across all branches simultaneously.

**Result:** Reduces the $\kappa$-dependence in QLSA from $O(\kappa^2)$ (HHL) to $\tilde{O}(\kappa)$.

**When to use:** Quantum linear systems, or any algorithm where the per-branch cost varies significantly across the spectrum of the problem.

### 5. Recursive amplitude amplification

> **Reference:** [[Recursive Amplitude Amplification (Høyer-Mosca-de Wolf)|Høyer, Mosca & de Wolf (2003)]]

The problem: what if the reflection about the good subspace is only *approximate*? Standard AA accrues a $\log(1/\varepsilon)$ multiplicative overhead from error in the reflection.

Recursive AA eliminates this: apply amplitude amplification *recursively*, using a coarser amplification at the inner level to produce a better reflection for the outer level. The log factor gets absorbed into the recursion depth with controlled error.

**Result:** Exact (up to $\varepsilon$) amplitude amplification in $O(1/\sqrt{a})$ queries — no log overhead.

**Primary application:** Walk-based quantum search. In [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes|Magniez et al. (2007)]], the approximate reflection comes from phase estimation on the walk operator. Recursive AA removes the log factor, achieving $O(1/\sqrt{\delta})$ walk search cost (where $\delta$ is the spectral gap) cleanly.

### 6. Randomised iteration count (unknown $a$)

> **Reference:** [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes|Boyer, Brassard, Høyer & Tapp (1998)]]
> **Trick card:** [[Randomised Iteration Count for Grover Search]]

When the success probability $a$ is unknown, the simplest fix: randomise the number of Grover iterations. Drawing $j$ uniformly from $\{0, \ldots, m-1\}$ averages over the oscillation, guaranteeing $\ge 1/4$ success probability whenever $m \ge 1/\sin(2\theta)$. Geometrically increasing $m$ until success gives $O(1/\sqrt{a})$ expected queries — matching the known-$a$ case up to a constant.

**When to use:** Any time you'd use standard AA but don't know $a$ and don't need the precision of amplitude estimation or the monotonicity of fixed-point AA.

---

## Comparison table

| Variant | Assumes | Iteration count | Overshoot? | Works for arbitrary input? | Key application |
|---|---|---|---|---|---|
| **Standard** | Known $a$, access to $A^{-1}$ and $S_\chi$ | $O(1/\sqrt{a})$ | Yes, if wrong count | No (reflects about $A|0\rangle$) | General search |
| **Oblivious** | $\sin^2\theta$ same for all inputs | $O(1/\sqrt{a})$ | Yes, if wrong count | **Yes** (reflects about ancilla) | LCU, block-encoding, Hamiltonian sim |
| **Fixed-point** | Lower bound $\gamma_{\min}$ on $a$ | $O(\log(1/\varepsilon)/\sqrt{\gamma_{\min}})$ | **No** | Depends on construction | Eigenstate filtering, ground state prep |
| **Variable-time** | Different branches have different costs | $\tilde{O}(\kappa)$ total | N/A (structured) | Yes | QLSA ($\kappa$ reduction) |
| **Recursive** | Approximate reflection | $O(1/\sqrt{a})$ (no log) | Yes, if wrong count | Depends | Walk-based search |
| **Randomised schedule** | Unknown $a$ | $O(1/\sqrt{a})$ expected | Averaged out | No (still reflects about $A|0\rangle$) | Search with unknown $t$ |

---

## Amplitude estimation

Amplitude estimation uses [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]] on $Q$ to extract $\theta_a$ (and thus $a = \sin^2\theta_a$). Cost: $O(1/\varepsilon)$ for additive error $\varepsilon$ — quadratically better than classical sampling.

### Variants

- **Standard (Brassard et al. 2002):** Phase estimation on $Q$ with $O(1/\varepsilon)$ controlled applications. Requires QFT.
- **Kaiser-window AE:** Better spectral leakage control, useful for multi-eigenvalue settings. See [[Kaiser-Window Amplitude Estimation]].
- **QFT-free / iterative AE:** Various methods avoid the QFT by using classical post-processing of multiple short runs (Suzuki et al. 2020, Aaronson & Rall 2020). Same $O(1/\varepsilon)$ query complexity, different circuit structure.

### Key applications
- **Quantum Monte Carlo:** Estimate $\mathbb{E}[f(X)]$ to error $\varepsilon$ with $O(1/\varepsilon)$ samples vs. classical $O(1/\varepsilon^2)$.
- **Approximate counting:** Estimate the number of marked items $t$ within $\sqrt{t}$ using $O(\sqrt{N})$ queries.
- **Financial derivatives pricing:** See [[Quantum computational finance — Monte Carlo pricing of financial derivatives (Rebentrost-Gupt-Bromley 2018) — Paper Notes|Rebentrost et al. (2018)]].

---

## The QSVT perspective

All amplitude amplification variants are special cases of [[QSVT Meta-Template|quantum singular value transformation]]. The Grover iterate $Q$ interleaves a block-encoding with reflections — exactly the QSP/QSVT circuit structure. The different variants correspond to different target polynomials:

| Variant | Polynomial |
|---|---|
| Standard AA ($\ell$ rounds) | $U_{2\ell}(\cos\theta) \cdot \sin\theta$ (Chebyshev) |
| Fixed-point AA | Monotone threshold polynomial |
| OAA | Same Chebyshev, but on singular values of the block-encoding |

This unification is one of the conceptual wins of [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]].

---

## Practical notes

- **The 1/4 sweet spot:** Many LCU constructions naturally produce $\sin^2\theta \approx 1/4$. Engineering this (via the "Segment Lemma" — diluting with an extra ancilla rotation) means a single OAA round gives exact amplification. Three queries instead of one, no residual error.

- **When NOT to amplify:** If you're inside a larger coherent algorithm and can tolerate the failure probability, it may be cheaper to handle failure at a higher level. Amplitude amplification is multiplicative — nesting it carelessly leads to $O((\log(1/\delta))^k)$ overheads.

- **Measurement-free requirement:** All variants require the base algorithm $A$ to be measurement-free (unitary). If $A$ involves mid-circuit measurements, you need to defer them or find a different approach.

---

## Primary references

- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard, Høyer, Mosca & Tapp (2002)]] — the foundational paper
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — original search algorithm
- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|Berry et al. (2014)]] — oblivious AA introduced
- [[Recursive Amplitude Amplification (Høyer-Mosca-de Wolf)|Høyer, Mosca & de Wolf (2003)]] — recursive AA
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]] — QSVT unification

---

## Trick cards in the vault

- [[Standard Amplitude Amplification]]
- [[Oblivious Amplitude Amplification (Robust)]]
- [[Fixed-Point Amplitude Amplification with Eigenstate Filtering]]
- [[Variable-Time Amplification for Condition-Number Reduction]]
- [[Randomised Iteration Count for Grover Search]]
- [[Amplitude Estimation via Phase Estimation on Q]]
- [[Kaiser-Window Amplitude Estimation]]
- [[Quadratic Speedup for Classical Heuristics via Amplitude Amplification]]
- [[Postselection Removal via Approximate Success-Amplitude Amplification]]
- [[Amplitude-Amplified State Preparation inside Walk Operators]]

---

## Related concept hubs

- [[Hamiltonian simulation]]
- [[Quantum Phase Estimation]]
- [[Product Formulas]]
