> **Source:** Dominic W. Berry, Andrew M. Childs, Richard Cleve, Robin Kothari, Rolando D. Somma, *Simulating Hamiltonian dynamics with a truncated Taylor series*, Phys. Rev. Lett. 114, 090502 (2015). arXiv:1412.4687
> **Links:** [arXiv](https://arxiv.org/abs/1412.4687) · [PRL](https://doi.org/10.1103/PhysRevLett.114.090502)
> **Tags:** #hamiltonian-simulation #LCU #taylor-series #oblivious-amplitude-amplification

---

## The computational problem

**Hamiltonian simulation.** Given a Hamiltonian $H = \sum_{\ell=1}^L \alpha_\ell H_\ell$ (each $H_\ell$ unitary, $\alpha_\ell > 0$), simulate the time evolution $U = e^{-iHt}$ to precision $\varepsilon$.

The cost should scale with $T := |\boldsymbol{\alpha}|_1 t = (\sum_\ell \alpha_\ell) t$ — the $\ell_1$-norm of coefficients times evolution time. This is the right complexity parameter for [[Linear Combination of Unitaries (LCU)|LCU]]-based methods.

---

## What the paper does

Presents the simplest known algorithm achieving **near-optimal** Hamiltonian simulation: $O(T \log(T/\varepsilon) / \log\log(T/\varepsilon))$ queries to the Hamiltonian terms, with only $\log(1/\varepsilon)$ dependence on precision. The method directly implements the truncated Taylor series of $e^{-iHt}$ via [[Linear Combination of Unitaries (LCU)|LCU]] and [[Oblivious Amplitude Amplification (Robust)|error-tolerant oblivious amplitude amplification]].

This paper is the clean, self-contained version of the same authors' earlier STOC 2014 result (arXiv:1312.1414), which achieved the same complexity but used a more indirect approach through fractional queries. Here the algorithm fits in four pages and the reason for logarithmic $\varepsilon$-dependence is immediate: it comes from the super-exponential decay of the Taylor series tail ($(\ln 2)^K / K!$).

---

## The algorithm

### Step 1: Segment the evolution

Divide $t$ into $r = \lceil T / \ln 2 \rceil$ segments. Within each segment, the "normalised time" is $\tau \leq \ln 2$. This choice is not accidental — it makes the coefficient sum equal to 2 (see [[Taylor Series Truncation with ln2 Segmentation]]).

### Step 2: Truncate the Taylor series

Approximate each segment's evolution:

$$\tilde{U} = \sum_{k=0}^{K} \frac{(-i\tau)^k}{k!} H^k$$

Expanding $H^k$ using $H = \sum_\ell \alpha_\ell H_\ell$:

$$\tilde{U} = \sum_{k=0}^{K} \sum_{\ell_1,\ldots,\ell_k} \frac{(-i\tau)^k}{k!} \alpha_{\ell_1} \cdots \alpha_{\ell_k} H_{\ell_1} \cdots H_{\ell_k}$$

Each term $(-i)^k H_{\ell_1} \cdots H_{\ell_k}$ is unitary (product of unitaries times a phase). So $\tilde{U}$ is a [[Linear Combination of Unitaries (LCU)|linear combination of unitaries]] with positive coefficients.

### Step 3: LCU implementation

Two oracles:

**PREPARE** $B$: Maps $|0\rangle$ to

$$B|0\rangle = \frac{1}{\sqrt{s}} \sum_{k=0}^{K} \sum_{\ell_1,\ldots,\ell_k} \sqrt{\frac{\tau^k}{k!} \alpha_{\ell_1} \cdots \alpha_{\ell_k}} \;|k\rangle|\ell_1\rangle \cdots |\ell_k\rangle$$

where $s = \sum_{k=0}^{K} (\ln 2)^k / k! \approx 2$.

**SELECT** $V$: Maps $|k\rangle|\ell_1\rangle \cdots |\ell_k\rangle|\psi\rangle \to |k\rangle|\ell_1\rangle \cdots |\ell_k\rangle \cdot (-i)^k H_{\ell_1} \cdots H_{\ell_k}|\psi\rangle$

Implemented as $K$ sequential controlled-SELECT(H) operations, with the $\kappa$-th operation controlled on the $\kappa$-th bit of the unary-encoded $|k\rangle$ register.

Then $W = (B^\dagger \otimes \mathbb{1}) \cdot \text{SELECT}(V) \cdot (B \otimes \mathbb{1})$ gives:

$$W|0\rangle|\psi\rangle = \frac{1}{s}|0\rangle\tilde{U}|\psi\rangle + \sqrt{1 - 1/s^2}\;|\Phi^\perp\rangle$$

### Step 4: Robust oblivious amplitude amplification

Since $s \approx 2$, a single round of [[Oblivious Amplitude Amplification (Robust)|OAA]] boosts the success amplitude from $1/s \approx 1/2$ to $\sim 1$:

$$A = -W R W^\dagger R W, \qquad R = \mathbb{1} - 2|0\rangle\langle 0| \otimes \mathbb{1}$$

**The error-tolerant version** (this paper's key technical contribution): even though $\tilde{U}$ is not exactly unitary (it's a truncated series), the amplification still works with error linear in the truncation error. Specifically, from Eq. (14):

$$PA|0\rangle|\psi\rangle = |0\rangle\left(\frac{3}{s}\tilde{U} - \frac{4}{s^3}\tilde{U}\tilde{U}^\dagger\tilde{U}\right)|\psi\rangle$$

If $|s - 2| = O(\delta)$ and $|\tilde{U} - U_r| = O(\delta)$, then $|PA|0\rangle|\psi\rangle - |0\rangle U_r|\psi\rangle| = O(\delta)$.

### Step 5: Compose segments

Repeat for all $r$ segments. Each segment introduces $O(\varepsilon/r)$ error; total error is $O(\varepsilon)$ by subadditivity.

---

## Key results

| Parameter | Scaling |
|---|---|
| **Queries** (uses of $H_\ell$) | $O\!\left(\frac{T \log(T/\varepsilon)}{\log\log(T/\varepsilon)}\right)$ |
| **Additional 2-qubit gates** | $O\!\left(\frac{nT \log^2(T/\varepsilon)}{\log\log(T/\varepsilon)}\right)$ |
| **Ancilla qubits** | $O\!\left(\frac{\log(L) \log(T/\varepsilon)}{\log\log(T/\varepsilon)}\right)$ |
| **Truncation order** $K$ | $O\!\left(\frac{\log(T/\varepsilon)}{\log\log(T/\varepsilon)}\right)$ |
| **Number of segments** $r$ | $\lceil T / \ln 2 \rceil$ |

Where $T = |\boldsymbol{\alpha}|_1 t$, $L$ = number of terms, $n$ = number of qubits.

**Precision dependence is exponentially better than all product-formula methods.** Trotter-Suzuki scales as $\text{poly}(1/\varepsilon)$; this scales as $\log(1/\varepsilon)$. Doubling the digits of accuracy only doubles the cost.

---

## Comparison with prior work

| Method | Precision dependence | Approach |
|---|---|---|
| [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] | $O(1/\varepsilon)$ | First-order Trotter |
| [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|BACS (2007)]] | $O(1/\varepsilon^{1/2k})$ | Higher-order Suzuki |
| [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs-Wiebe (2012)]] | Polynomial | LCU, no OAA |
| BCCKS (2014, STOC) [1312.1414] | $\widetilde{O}(\log(1/\varepsilon))$ | Fractional queries |
| **This paper** | $\widetilde{O}(\log(1/\varepsilon))$ | **Direct Taylor + error-tolerant OAA** |
| [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low-Chuang (2017)]] | $O(\log(1/\varepsilon))$ | QSP (optimal) |

This paper matched the STOC 2014 result with a much simpler algorithm. [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low-Chuang (2017)]] later achieved strictly optimal query complexity by removing the $\log\log$ factor via [[QSVT Meta-Template|QSP/QSVT]], but the Taylor series approach remains the more accessible and practical method.

---

## Circuit construction details

### Ancilla encoding

- **Order register** $|k\rangle$: Unary encoding ($|1^k 0^{K-k}\rangle$), $K$ qubits
- **Term registers** $|\ell_1\rangle \cdots |\ell_K\rangle$: Each $\lceil\log L\rceil$ qubits, $K$ registers total

### PREPARE gate $B$

Tensor product of:
1. Single rotation chain on the unary register: $|0^K\rangle \to \sum_k \sqrt{(\tau)^k/k!}\;|1^k 0^{K-k}\rangle$. Cost: $O(K)$ gates (conditional rotations).
2. On each term register: $|0\rangle \to \sum_\ell \sqrt{\alpha_\ell / |\boldsymbol{\alpha}|_1}\;|\ell\rangle$. Cost: $O(L)$ per register.

Total PREPARE cost: $O(LK)$.

### SELECT gate

$K$ sequential controlled-SELECT(H) operations, each applying $H_{\ell_\kappa}$ controlled on the $\kappa$-th unary bit and the $\kappa$-th term register. For Pauli decompositions: cost $O(L(n + \log L))$ per controlled-SELECT(H).

Total SELECT cost: $O(LK(n + \log L))$.

### Cost per segment

One application of $A = -WRW^\dagger RW$ requires 3 SELECT and 6 PREPARE operations (plus reflections).

---

## Why the $\log(1/\varepsilon)$ scaling

The magic comes from Stirling's approximation. The Taylor series tail:

$$\sum_{k > K} \frac{(\ln 2)^k}{k!} \leq \left(\frac{e \ln 2}{K}\right)^K$$

This is super-exponentially small in $K$. So $K = O(\log(T/\varepsilon) / \log\log(T/\varepsilon))$ suffices for precision $\varepsilon/r$. Compare with Trotter, where precision $\varepsilon$ requires $O(1/\varepsilon^{1/2k})$ steps — polynomial no matter how high the order $k$.

---

## Time-dependent extension

The paper outlines an extension to time-dependent $H(t)$ via discretised time-ordered integrals:

$$\tilde{U} = \sum_{k=0}^{K} \frac{(-it/r)^k}{M^k k!} \sum_{j_1,\ldots,j_k} \mathcal{T}\;H(t_{j_k}) \cdots H(t_{j_1})$$

Additional ancillas encode discrete time points $|t_{j_1}\rangle \cdots |t_{j_k}\rangle$. The resulting algorithm matches the time-independent cost up to $\log(M)$ factors, where $M$ is polynomial in $1/\varepsilon$ and $\max_t |dH/dt|$.

This was later developed into full-fledged algorithms by [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová-Scherer-Berry (2018)]] and [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes|Berry-Childs-Su-Wang-Wiebe (2020)]].

---

## Reusable ideas

All the reusable techniques from this paper already have trick cards in the vault:

1. **[[Taylor Series Truncation with ln2 Segmentation]]** — the $\ln 2$ segment length choice and truncation order analysis
2. **[[Oblivious Amplitude Amplification (Robust)]]** — the error-tolerant version that handles non-unitary $\tilde{U}$; this paper's Eq. (14) is the cleanest proof
3. **[[Linear Combination of Unitaries (LCU)]]** — the PREPARE/SELECT framework as used here; this paper gave the clean formulation that became standard
4. **[[Unary Encoding for Sequential Controlled Operations]]** — the unary $|k\rangle$ register trick for sequential application of $H_{\ell_1} \cdots H_{\ell_k}$

---

## Limits / caveats

- **Not quite optimal.** The $\log\log(T/\varepsilon)$ denominator means this is near-optimal but not tight. [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low-Chuang (2017)]] removed this factor via QSP.
- **Ancilla overhead.** $O(K \log L)$ ancillas — modest, but grows with both precision and number of terms.
- **PREPARE cost scales with $L$.** For Hamiltonians with many terms (e.g., molecular Hamiltonians after Jordan-Wigner), the cost of preparing the coefficient state dominates. Later work on [[Standard-Form Encoding (Prepare + Signal Oracle)|double factorization and tensor hypercontraction]] reduces this.
- **No commutator structure.** Unlike Trotter methods, this approach treats all terms symmetrically. It doesn't benefit from near-commutativity of $H_\ell$'s — the cost is always $|\boldsymbol{\alpha}|_1 t$, even when the Hamiltonian is nearly diagonal.
- **Single-round OAA needs $s = 2$.** If the coefficient sum drifts from 2 (e.g., the last segment), an extra ancilla qubit is needed for [[Amplitude Dilution for Exact OAA|amplitude dilution]].

---

## Why this matters

This is the paper that made [[Linear Combination of Unitaries (LCU)|LCU]]-based Hamiltonian simulation practical and comprehensible. The STOC 2014 predecessor achieved the same asymptotics but via a complicated fractional-query reduction. Here the algorithm is direct: write down the Taylor series, implement it as an LCU, amplify with OAA. The entire paper is four pages.

The $\log(1/\varepsilon)$ precision scaling it demonstrated — exponentially better than any product formula — established LCU as the dominant paradigm for high-precision quantum simulation, later refined by [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|qubitization]] and [[QSVT Meta-Template|QSVT]] but never conceptually replaced. If you want to understand why modern quantum algorithms use block-encoding instead of Trotter, start here.

---

## References within this paper

- Feynman (1982) — original motivation for quantum simulation
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — first product-formula simulation [Ref 2]
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|BACS (2007)]] — higher-order Suzuki [Ref 5]
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs-Wiebe (2012)]] — LCU primitive [Ref 12]
- [[Exponential Improvement in Precision for Hamiltonian-Evolution Simulation (Berry-Cleve-Somma 2013) — Paper Notes|Berry-Cleve-Somma (2013)]] — first polylog-precision simulation via compressed Trotter (precursor, arXiv:1308.5424)
- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|BCCKS (2014, STOC)]] — full fractional-query version with OAA and lower bounds [Ref 18]
- Kothari (2014), Ph.D. thesis — detailed LCU analysis [Ref 21]
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL (2009)]] — application of simulation to linear systems [Ref 20]

---

## Cross-links

### Paper notes
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]] — first simulation algorithm; exponentially worse precision scaling
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]] — earlier product-formula approach by overlapping authors
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]] — the LCU primitive this paper builds on
- [[Exponential Improvement in Precision for Hamiltonian-Evolution Simulation (Berry-Cleve-Somma 2013) — Paper Notes]] — first polylog-precision simulation
- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes]] — full STOC version with OAA and lower bounds; this paper simplifies it
- [[Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) — Paper Notes]] — combines walk + LCU for optimal sparsity scaling
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] — achieved strictly optimal scaling, removing the $\log\log$ factor
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — reformulated via block-encoding + QSP
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — generalised the polynomial framework
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]] — extended the Taylor/LCU approach to time-dependent $H(t)$
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes]] — further refinement of time-dependent simulation
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — uses Hamiltonian simulation as a subroutine
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]] — applies LCU methods to ODEs
- [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes]] — quantum chemistry application that benefits from improved simulation
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — tight Trotter bounds; complementary approach

### Trick cards
- [[Taylor Series Truncation with ln2 Segmentation]] — the segment length choice
- [[Oblivious Amplitude Amplification (Robust)]] — error-tolerant OAA for non-unitary targets
- [[Linear Combination of Unitaries (LCU)]] — the PREPARE/SELECT framework
- [[Unary Encoding for Sequential Controlled Operations]] — unary $|k\rangle$ encoding for term indexing
- [[Standard-Form Encoding (Prepare + Signal Oracle)]] — the oracle abstraction
- [[Amplitude Dilution for Exact OAA]] — handling $s \neq 2$ in the last segment
- [[Block-Encoding Composition Algebra]] — the broader framework this feeds into
