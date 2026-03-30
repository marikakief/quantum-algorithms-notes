> **Source:** Dorit Aharonov and Michael Ben-Or, *Fault-tolerant quantum computation with constant error rate*, SIAM Journal on Computing **38**(4):1207–1282, 2008; originally STOC 1997 (quant-ph/9611025); revised as arXiv:quant-ph/9906129
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/9906129) · [Journal](https://doi.org/10.1137/S0097539799359385)
> **Tags:** #fault-tolerance #threshold-theorem #concatenation #CSS-codes #polynomial-codes #noise-model #foundational

---

## The computational problem

**Fault-tolerant quantum computation with constant noise:** Given a quantum circuit $Q$ with $n$ qubits, $s$ gates, running for $t$ time steps, and subjected to noise with error rate $\eta$ per gate per time step — can $Q$ be simulated by a fault-tolerant circuit $Q'$ with only polylogarithmic overhead in space and time, provided $\eta < \eta_c$ for some constant threshold $\eta_c$?

Prior work by Shor (1996) required the error rate to decrease polylogarithmically with circuit size — a physically unreasonable assumption. The question is whether a *constant* error rate suffices.

---

## What the paper does

Provides the first complete, rigorous proof that fault-tolerant quantum computation is possible with a **constant** error rate. This is the quantum analogue of von Neumann's 1956 result for classical circuits.

The paper is one of the most technically demanding in quantum computing. It proves 13 theorems across 76 pages, building the entire fault-tolerance stack from scratch: quantum error-correcting codes (CSS and a new class of polynomial codes), fault-tolerant procedures for a universal gate set, recursive concatenation, threshold analysis for both probabilistic and general noise models, extension to arbitrary universal gate sets via the [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes|Solovay-Kitaev theorem]], and generalisation to 1D nearest-neighbour architectures.

The main result: if the error rate $\eta$ is below a constant threshold $\eta_c$, then any quantum circuit can be made fault-tolerant with only polylogarithmic blowup: $O(n \log^{c_1}(v/\varepsilon))$ qubits, $O(t \log^{c_2}(v/\varepsilon))$ time, $O(s \log^{c_3}(v/\varepsilon))$ gates, where $v$ is the circuit volume and $\varepsilon$ is the target failure probability.

The calculated threshold is $\eta_c \approx 10^{-6}$ — not optimised, and later work ([[Quantum Computing with Realistically Noisy Devices (Knill 2005) — Paper Notes|Knill 2005]], Steane, Reichardt) improved this by orders of magnitude using different architectures. But the theoretical significance is the existence of a constant threshold at all.

---

## The construction

### Noise model

The paper uses the model of quantum circuits with density matrices (Aharonov-Kitaev-Nisan), where states are mixed and gates are general quantum operations (not necessarily unitary). This is more natural for noisy systems than the pure-state model.

**Probabilistic noise:** Each qubit/gate, each time step, undergoes a fault (arbitrary quantum operation) with independent probability $\eta$.

**General noise:** Each qubit/gate undergoes a quantum operation that is at most $\eta$ far from the identity (in a suitable metric). This includes decoherence, amplitude damping, phase damping, depolarisation, and systematic gate inaccuracies.

**Correlated noise (extension):** The paper also handles exponentially decaying correlations in both space and time. The Markovian and locality assumptions can be relaxed to allow correlations that decay as $e^{-\alpha d}$ where $d$ is the space-time distance.

### Quantum error-correcting codes

**CSS codes (Calderbank-Shor-Steane):** Built from two classical linear codes $C_2 \subset C_1 \subset \mathbb{F}_2^m$ such that both $C_1$ and $C_2^\perp$ correct $t$ errors. The paper gives a simplified proof of CSS code correctness, based on the observation that it suffices to correct bit flips and phase flips separately (Theorem 1), and that phase flips become bit flips in the Fourier basis (Theorem 2).

**Polynomial codes (new):** A new class of CSS codes over $\mathbb{F}_p$ inspired by the Ben-Or-Goldwasser-Wigderson secret sharing scheme. Encode a logical qupit in $m$ physical qupits by:
- Choose a random polynomial $f$ of degree $d$ over $\mathbb{F}_p$
- The secret is $f(0)$
- Each physical qupit $i$ stores the evaluation $f(x_i)$ at a distinct point $x_i \in \mathbb{F}_p$
- Replace "random polynomial" with "superposition of all polynomials" to get a quantum code

**Theorem 3:** A polynomial code of degree $d$ with length $m$ over $\mathbb{F}_p$ corrects $\min\{\lfloor(m-d-1)/2\rfloor, \lfloor d/2\rfloor\}$ errors.

The connection to secret sharing is deep: the code protects quantum information because no subset of $t$ qupits shares any information about the logical state — exactly the security property of a secret sharing scheme.

### Fault-tolerant gate sets

Two universal gate sets, each paired with its code family:

**$G_1$ (for CSS codes):** Based on Shor's procedures, modified to work without classical operations or measurements. Almost all gates are applied bitwise (transversally). The Toffoli gate requires a complex fault-tolerant procedure. (Theorem 4: one error in a qubit or gate affects at most 4 qubits per block.)

**$G_2$ (for polynomial codes):** All gates are applied pit-wise, then a degree reduction step (based on polynomial interpolation) restores the code structure when the gate doubles the polynomial degree. This gives a simple, systematic structure. (Theorem 5: one error affects at most 1 qubit per block.)

**Universality proofs:**
- $G_1$ universality (Theorem 8): reduction to Kitaev's universal set via constructing the $CP$ gate ($|11\rangle \mapsto i|11\rangle$) and Hadamard from $G_1$
- $G_2$ universality (Theorem 9): more involved, using field-theoretic arguments to show that $G_2$ generates matrices with eigenvalues that are not roots of unity, then a geometric lemma: if $A, B$ are non-orthogonal subspaces, then $U(A) \cup U(B)$ generates all of $U(A \oplus B)$

### Recursive concatenation (the core technique)

The heart of the paper. Start with the original circuit $M_0$. Build $M_1$ by encoding each qubit in a block and replacing each gate with a fault-tolerant procedure. If $\eta < \eta_c$, then $M_1$ has lower effective error rate than $M_0$.

Now iterate: build $M_2$ by encoding $M_1$, and so on for $r$ levels, producing $M_r$. The improvement is exponential in the number of levels because each level reduces the effective error rate, and the concatenation compounds this reduction.

The final circuit $M_r$ has a hierarchical structure:
- Largest procedures (level $r$) correspond to gates of the original circuit
- Smaller procedures (lower levels) handle error correction within blocks
- Error corrections at level $l$ are applied more frequently for smaller $l$, matching the faster error accumulation rate at lower levels

### Threshold analysis

The analysis distinguishes between:
- **Fault paths:** the specific locations in space-time where faults occur in a given run
- **Error states:** the resulting corruption in the quantum state

Both are defined recursively via the concatenation hierarchy:
- A level-0 block is "close to correct" if it has few errors
- A level-$l$ block is "close to correct" if few of its level-$(l-1)$ subblocks are far from correct
- A fault path is **sparse** if no high-level procedure has too many damaged sub-procedures

**Theorem 10 (Threshold for probabilistic noise):** There exists a threshold $\eta_c > 0$ such that for $\eta < \eta_c$, the probability of non-sparse (bad) fault paths decays exponentially with the number of concatenation levels $r$, and sparse (good) fault paths keep the error state sparse throughout the computation. The overhead is polylogarithmic: $O(n \log^{c_1}(v/\varepsilon))$ qubits, $O(t \log^{c_2}(v/\varepsilon))$ time.

**Theorem 11 (Threshold for general noise):** The same holds for general (non-probabilistic) noise, including coherent errors, decoherence, and damping. The proof expands the noise operator as identity + error term, where each term in the expansion corresponds to a fault path. The bad-faults-are-rare argument carries over with norm bounds replacing probability bounds.

**Theorem 12 (Full generality):** Fault-tolerant computation is possible with **any** universal gate set, not just $G_1$ or $G_2$. First build the fault-tolerant circuit using $G_1$ or $G_2$, then approximate each gate to constant accuracy using gates from the desired set via the [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes|Solovay-Kitaev theorem]] (Theorem 6).

**Theorem 13 (1D circuits):** The threshold result extends to one-dimensional quantum computers with nearest-neighbour interactions. Swap gates bring distant qubits together without excessive error propagation.

### Threshold estimate

For polynomial codes with no geometry constraints and noiseless classical computation:

$$\eta_c \approx 10^{-6}$$

This was not optimised. The threshold depends on the size of the largest fault-tolerant procedure and the code's correction capacity.

---

## Key results

**Theorem 10 (Main):** For any quantum circuit $Q$ with $v$ locations and any $\varepsilon > 0$, if the probabilistic error rate $\eta < \eta_c$, there exists a fault-tolerant circuit $Q'$ with $O(v \cdot \text{polylog}(v/\varepsilon))$ locations such that the output of $Q'$ with noise is $\varepsilon$-close to the output of $Q$ without noise.

**Theorem 11:** Same for general noise (including coherent errors, decoherence, damping, systematic inaccuracies).

**Theorem 12:** Works with any universal gate set.

**Theorem 13:** Works for 1D nearest-neighbour architectures.

**Threshold value:** $\eta_c \approx 10^{-6}$ (not optimised).

**Overhead:** Polylogarithmic in circuit volume and $1/\varepsilon$:
- Qubits: $O(n \log^{c_1}(v/\varepsilon))$
- Time: $O(t \log^{c_2}(v/\varepsilon))$  
- Gates: $O(s \log^{c_3}(v/\varepsilon))$

---

## Comparison with prior work

| Paper | Error rate assumption | Noise model | Architecture |
|---|---|---|---|
| Shor (1996) | Polylog-small in circuit size | Probabilistic | Concatenated CSS codes |
| **Aharonov-Ben-Or (1996/2008)** | **Constant ($\eta < \eta_c \approx 10^{-6}$)** | **General (incl. correlated)** | **Concatenated CSS + polynomial codes** |
| Kitaev (1997) | Constant | Probabilistic | Concatenated codes |
| Knill-Laflamme-Zurek (1998) | Constant ($\sim 10^{-5}$) | Probabilistic | Concatenated codes |
| [[Quantum Computing with Realistically Noisy Devices (Knill 2005) — Paper Notes\|Knill (2005)]] | Constant ($\sim 3\%$) | Independent depolarising | $C_4/C_6$ + postselection |

Kitaev and Knill-Laflamme-Zurek independently proved threshold results around the same time (1996–1998). Aharonov-Ben-Or's contribution is the most general: it handles non-independent noise, works without measurements/classical operations, extends to arbitrary gate sets and 1D architectures, and provides a complete self-contained proof.

---

## Limits / caveats

- The threshold $\eta_c \approx 10^{-6}$ is very conservative — the paper acknowledges it was not optimised. Later work achieved $10^{-2}$ or higher.
- The general noise model requires local noise (or exponentially decaying correlations). Truly non-local noise can defeat any fault-tolerance scheme.
- The no-measurement, no-classical-computation constraint (interesting theoretically) increases the procedure size and lowers the threshold. Allowing measurements improves it.
- Sequential quantum computation (quantum Turing machines) **cannot** be made fault-tolerant — the paper notes this explicitly. Parallelism is required.
- The ability to input fresh qubits during computation is required to discard entropy. Without it, fault tolerance requires exponential blowup.
- The paper gives general threshold formulas but the constants depend heavily on the specific procedures. Optimising these constants is a separate engineering problem.

---

## Reusable ideas

1. [[Recursive Concatenation for Fault Tolerance]] — The scheme of building a hierarchy of encoded circuits $M_0 \to M_1 \to \cdots \to M_r$ where each level uses a constant-size error-correcting code. Each level reduces the effective error rate, and the improvement compounds exponentially with the number of levels. The final error probability decays superexponentially: $\eta_{\text{eff}} \sim \eta_c(\eta/\eta_c)^{2^r}$ (for distance-3 codes). The polylogarithmic overhead comes from needing only $r = O(\log \log(1/\varepsilon))$ levels.

2. [[Polynomial Codes from Secret Sharing]] — Constructing quantum error-correcting codes by quantising classical secret sharing schemes: replace a random polynomial over $\mathbb{F}_p$ with a superposition of all polynomials. The resulting CSS codes have a natural algebraic structure that makes fault-tolerant gate implementation systematic — all gates are applied pit-wise, with degree reduction via polynomial interpolation handling the cases where gates increase the polynomial degree.

3. [[Sparse Fault Path Analysis]] — Defining "good" and "bad" fault configurations recursively via the concatenation hierarchy, then showing that bad configurations are exponentially unlikely (the easy part) and that good configurations preserve the error state's sparsity (the hard part). This analysis framework — splitting into good/bad parts and arguing separately — extends to general noise by replacing probability bounds with operator norm bounds.

---

## References within this paper

- Shor (1996) — introduced fault-tolerant procedures for concatenated codes; assumed polylogarithmic error rate
- Calderbank-Shor (1996) and Steane (1996) — CSS quantum error-correcting codes
- [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes|Kitaev (1997) / Solovay]] — universality of gate sets with polylog approximation overhead
- Aharonov-Kitaev-Nisan — quantum circuits with density matrices (the computational model used)
- Ben-Or-Goldwasser-Wigderson (1988) — classical secret sharing via polynomial evaluation; inspiration for polynomial codes
- Schumacher-Nielsen — quantum state can be corrected iff no information leaked to environment
- Gottesman (1997) — stabiliser codes
- von Neumann (1956) — classical fault-tolerant computation; the classical analogue of the threshold theorem
- Tsirelson and Gács — classical results on fault tolerance that are generalised to the quantum case
- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes|Deutsch (1985)]] and Barenco et al. — universality of two-qubit gates
- Bennett et al. — it suffices to correct bit and phase flips

---

## Cross-links

### Paper notes
- [[Quantum Computing with Realistically Noisy Devices (Knill 2005) — Paper Notes]] — achieves much higher thresholds ($\sim 3\%$) but without rigorous proof; different architecture
- [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes]] — used in Theorem 12 to extend fault tolerance to arbitrary gate sets
- [[Topological Quantum Computation (Freedman-Kitaev-Larsen-Wang 2003) — Paper Notes]] — alternative approach: topological error protection rather than concatenated codes
- [[Simulation of Topological Field Theories by Quantum Computers (Freedman-Kitaev-Wang 2002) — Paper Notes]] — mentions threshold theorems in context of TQFT error protection
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — Kitaev's work on quantum computation and error correction
- [[Theory of Quantum Error-Correcting Codes (Knill-Laflamme 1997) — Paper Notes]] — the foundational QEC theory (Knill-Laflamme conditions) that this paper's constructions build on

### Trick cards
- [[Recursive Concatenation for Fault Tolerance]]
- [[Polynomial Codes from Secret Sharing]]
- [[Sparse Fault Path Analysis]]
- [[Solovay-Kitaev Recursive Gate Compilation]]
- [[Error-Detecting Codes with Concatenation for Fault Tolerance]]
- [[Error-Correcting Teleportation]]
