> **Source:** Thomas G. Draper, *Addition on a Quantum Computer*, arXiv:quant-ph/0008033 (2000)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0008033)
> **Tags:** #quantum-arithmetic #QFT #adder #circuit-primitive #Fourier-transform

---

## The computational problem

Add two $n$-bit integers on a quantum computer. This note uses Draper's convention where the second register is preserved and the first register is updated:

$$|a\rangle|b\rangle \mapsto |a+b\rangle|b\rangle.$$

Equivalent variants can add into the other register by swapping labels. The cost measures are qubit count and gate count.

At the time, the standard classical reversible adders used for comparison (Vedral-Barenco-Ekert, Beckman-Chari-Devabhaktuni-Preskill) required $3n$ qubits because they used $n$ temporary carry qubits. Later ripple-carry constructions, especially Cuccaro-Draper-Kutin-Moulton, changed this baseline, so the $3n$ comparison should be read historically.

## What the paper does

Introduces the **QFT-based adder**: transform $|a\rangle$ into its [[Quantum Fourier Transform Circuit|QFT]] representation $|F(a)\rangle$, apply a sequence of controlled phase rotations conditioned on $|b\rangle$ to evolve $|F(a)\rangle \to |F(a+b)\rangle$, then invert the QFT. This eliminates all carry qubits, reducing the qubit count from $3n$ to $2n$. The gate count is $O(n^2)$ in the exact version and $O(n \log n)$ using the approximate QFT.

This is the first quantum adder that has no classical analogue — it works entirely in the Fourier basis and exploits the phase structure of the QFT.

## The algorithm / construction

### Setup

Write $a = a_n 2^{n-1} + \cdots + a_1 2^0$ in binary. The QFT maps:

$$|a\rangle \xrightarrow{F_{2^n}} \frac{1}{2^{n/2}} \sum_{k=0}^{2^n - 1} e^{2\pi i a k / 2^n} |k\rangle$$

This factorises into a tensor product (as shown by Cleve-Ekert-Macchiavello-Mosca):

$$|F(a)\rangle = |\phi_n(a)\rangle \otimes \cdots \otimes |\phi_1(a)\rangle$$

where

$$|\phi_k(a)\rangle = \frac{1}{\sqrt{2}}\left(|0\rangle + e^{2\pi i \cdot 0.a_k a_{k-1} \cdots a_1}|1\rangle\right)$$

and $0.a_k \cdots a_1$ is a binary fraction. Each qubit $|\phi_k(a)\rangle$ encodes the lower $k$ bits of $a$ in its phase.

### The QFT circuit

The standard QFT circuit uses Hadamard gates and controlled rotations $R_k$:

$$R_k = \text{diag}(1, 1, 1, e^{2\pi i / 2^k})$$

applied as a controlled phase gate. The circuit transforms $|a_n\rangle$ by applying $H$ followed by $R_2$ (controlled on $a_{n-1}$), $R_3$ (controlled on $a_{n-2}$), ..., $R_n$ (controlled on $a_1$), yielding $|\phi_n(a)\rangle$. Each subsequent qubit follows the same pattern with fewer rotations. Total gate count: $\frac{1}{2}n(n+1)$.

### The addition step

Given $|F(a)\rangle = |\phi_n(a)\rangle \otimes \cdots \otimes |\phi_1(a)\rangle$ and a register $|b\rangle = |b_n \cdots b_1\rangle$, the addition is a sequence of controlled rotations:

- On qubit $|\phi_n(a)\rangle$: apply $R_1$ controlled on $b_n$, then $R_2$ controlled on $b_{n-1}$, ..., then $R_n$ controlled on $b_1$.
- On qubit $|\phi_{n-1}(a)\rangle$: apply $R_1$ controlled on $b_{n-1}$, ..., $R_{n-1}$ controlled on $b_1$.
- Continue down to $|\phi_1(a)\rangle$: apply $R_1$ controlled on $b_1$.

The effect on $|\phi_n(a)\rangle$ is:

$$|\phi_n(a)\rangle \xrightarrow{R_1(b_n)} \frac{1}{\sqrt{2}}\left(|0\rangle + e^{2\pi i(0.a_n \cdots a_1 + 0.b_n)}|1\rangle\right)$$
$$\xrightarrow{R_2(b_{n-1})} \frac{1}{\sqrt{2}}\left(|0\rangle + e^{2\pi i(0.a_n \cdots a_1 + 0.b_n b_{n-1})}|1\rangle\right)$$
$$\xrightarrow{\cdots} |\phi_n(a+b)\rangle$$

After all rotations, $|F(a)\rangle$ has become $|F(a+b)\rangle$. Applying $F^{-1}$ recovers $|a+b\rangle$.

### Full procedure

$$|a\rangle|b\rangle \xrightarrow{F \otimes I} |F(a)\rangle|b\rangle \xrightarrow{\text{add}} |F(a+b)\rangle|b\rangle \xrightarrow{F^{-1} \otimes I} |a+b\rangle|b\rangle$$

### Gate count

The addition step alone uses $\frac{1}{2}n(n+1)$ controlled rotation gates — the same order as the QFT itself. Including the forward and inverse QFT, the total is $\frac{3}{2}n(n+1)$ gates.

### Approximate version

Barenco, Ekert, Suominen, and Torma showed that for the QFT, rotations $R_k$ with $k > O(\log n)$ can be dropped with negligible error (the approximate QFT, or AQFT). The same applies to the addition step. Truncating rotations at $k = O(\log n)$ reduces the abstract rotation count from $O(n^2)$ to $O(n \log n)$ for each of the three stages (QFT, addition, inverse QFT). Fault-tolerant Clifford+T synthesis changes the practical cost because these small-angle rotations are not native Clifford gates.

### Classical-to-quantum addition

When adding a classical number $b$ (known at compile time) to a quantum register $|a\rangle$, the $b$-register does not need to be encoded as qubits. The controlled rotations become unconditional rotations whose angles are precomputed classically. This halves the qubit requirement from $2n$ to $n$.

### Parallelism

All controlled rotations in the addition step commute with each other (unlike the QFT, which has non-commuting Hadamard gates). If the hardware supports $n/2$ simultaneous two-qubit gates, the addition step can be parallelised to $O(n)$ time slices. With AQFT-style truncation, this drops to $O(\log n)$ time slices for the addition rotations. The depth of the full QFT-add-inverse-QFT adder also includes the forward and inverse QFT implementations, and depends on connectivity and whether extra workspace or semiclassical variants are allowed.

## Key results

**Qubit count:**
$$\text{QFT adder: } 2n \quad \text{vs.} \quad \text{Classical reversible adder: } 3n$$

**Gate count (exact):**
$$O(n^2) \text{ controlled rotations (addition step alone: } \tfrac{1}{2}n(n+1)\text{)}$$

**Gate count (approximate, AQFT-style truncation):**
$$O(n \log n)$$

**Parallel depth of addition step only (with AQFT truncation):**
$$O(\log n) \text{ time slices}$$

**Application to [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]]:** in the historical Zalka/Draper setting, replaces the $3n$-qubit adder in Zalka's optimised implementation and brings the arithmetic register count down to $2n$ while maintaining $O(n^3)$ abstract gate count. Later Beauregard and Toffoli-based constructions use different qubit/gate tradeoffs.

## Comparison with prior work

| Construction | Qubits | Gates | Carry qubits | Notes |
|---|---|---|---|---|
| Vedral-Barenco-Ekert (1996) | $3n$ | $O(n)$ | $n$ | Classical ripple-carry |
| Beckman-Chari-Devabhaktuni-Preskill (1996) | $3n$ | $O(n)$ | $n$ | Optimised classical reversible |
| Gossett (1998) carry-save | $> 3n$ | $O(n \log n)$ | $> n$ | Classical carry-save, faster |
| **Draper (2000) QFT adder** | $2n$ | $O(n^2)$ or $O(n \log n)$ approx | **0** | No carry bits; Fourier basis |
| Cuccaro-Draper-Kutin-Moulton (2004) | $2n+1$ | $O(n)$ | 1 | Ripple-carry, $O(n)$ gates |
| [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney (2018)]] | $2n-1$ | $4n-4$ T gates | $n-1$ temporary | Lowest T-count |

The QFT adder saves qubits at the cost of gate count. Classical reversible adders use $O(n)$ gates but need $n$ extra qubits. The QFT adder uses $O(n^2)$ gates (or $O(n \log n)$ approximate) but needs zero carry qubits. The tradeoff is qubit-count vs. circuit depth.

**Assessment:** This paper introduced a genuinely new idea — doing arithmetic in the Fourier basis rather than mimicking classical circuits. The $O(n^2)$ gate count is worse than classical-style adders for raw speed, but the qubit savings matter when qubits are the bottleneck (e.g., minimising Shor's algorithm qubit count). The deeper legacy is the concept itself: the Fourier basis as a computational workspace, not just a subroutine endpoint. Later work by Cuccaro, Draper, Kutin, and Moulton (2004) and by Beauregard (2002) built on this idea to construct hybrid adders with better tradeoffs.

## Limits / caveats

1. **Gate count:** $O(n^2)$ is quadratically worse than the $O(n)$ gate count of classical reversible adders. The approximate version ($O(n \log n)$) improves this but is still worse than $O(n)$.

2. **Rotation precision:** The exact QFT adder requires exponentially small rotation angles ($2\pi / 2^n$). On fault-tolerant hardware, each such rotation must be synthesised into Clifford+T gates at cost $O(\log(1/\varepsilon))$ per rotation, inflating the total gate count. The AQFT truncation mitigates this.

3. **No native fault-tolerant implementation:** The controlled rotations are not Clifford gates. On a fault-tolerant architecture where T gates are expensive, the QFT adder's cost in T gates is substantially higher than ripple-carry adders that use only Toffoli gates. This is why later work ([[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney 2018]]) counts T gates rather than abstract rotations.

4. **Arithmetic modulo $2^n$:** The addition is inherently modulo $2^n$. Detecting overflow requires an additional qubit and modifications to the circuit.

## Reusable ideas

1. [[Arithmetic in the Fourier Basis]] — Perform addition by applying controlled phase rotations to a QFT-encoded register, bypassing carry propagation entirely. The QFT converts positional (binary) representation to a phase representation where addition is a sequence of commuting rotations.

2. [[Fourier State as Eigenstate of Addition]] — The QFT state $|F(a)\rangle$ responds to the addition operation via phase kickback: adding $b$ multiplies each Fourier-basis qubit's phase by $e^{2\pi i b / 2^k}$. This eigenvalue property is the mechanism behind [[Phase Gradient Rotation Synthesis]] and [[Phase Gradient State for Controlled Rotations]].

3. [[Commutativity of Fourier-Basis Addition for Parallelism]] — All controlled rotations in the QFT addition step commute, unlike those in the QFT circuit itself. This allows the addition to be massively parallelised when the hardware supports simultaneous two-qubit gates.

## References within this paper

- Barenco, Ekert, Suominen, Torma (1996), "Approximate quantum Fourier transform and decoherence", quant-ph/9601018 — proves AQFT with $O(\log n)$ rotation cutoff suffices; the basis for the $O(n \log n)$ approximate adder
- Beckman, Chari, Devabhaktuni, Preskill (1996), "Efficient networks for quantum factoring", Phys. Rev. A 54, 1034 — classical reversible adder for factoring
- [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes|Cleve, Ekert, Macchiavello, Mosca (1998)]], "Quantum algorithms revisited", quant-ph/9708016 — proves the QFT output is unentangled (tensor product of single-qubit states)
- Coppersmith (1994), "An approximate Fourier transform useful in quantum factoring", IBM Research Report RC19642 — early description of the approximate QFT
- Gossett (1998), "Quantum carry-save arithmetic", quant-ph/9808061 — carry-save quantum adder
- Vedral, Barenco, Ekert (1996), "Quantum networks for elementary arithmetic operations", Phys. Rev. A 54, 147 — first quantum adder circuit
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — the factoring algorithm whose qubit count this paper reduces
- Zalka (1998), "Fast versions of Shor's quantum factoring algorithm", quant-ph/9806084 — shows Shor's algorithm can run in $3n$ qubits; Draper's adder reduces this to $2n$

## Cross-links

### Paper notes
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes]] — Draper's own ripple-carry adder (with Cuccaro, Kutin, Moulton); single-ancilla alternative to the QFT approach
- [[A Logarithmic-Depth Quantum Carry-Lookahead Adder (Draper-Kutin-Rains-Svore 2004) — Paper Notes]] — Draper's carry-lookahead adder (with Kutin, Rains, Svore); $O(\log n)$ depth using $O(n)$ ancillae
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — later work optimising T-count for fault-tolerant adders; comparison table includes Draper's QFT adder
- [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes]] — proves the QFT factorisation used here
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — the QFT adder reduces Shor's qubit count from $3n$ to $2n$
- [[Applying Quantum Algorithms to Constraint Satisfaction Problems (Campbell-Khurana-Montanaro 2019) — Paper Notes]] — uses Draper-Kutin-Rains-Svore adders (descendants of this work)
- [[Trading T Gates for Dirty Qubits in State Preparation and Unitary Synthesis (Low-Kliuchnikov-Schaeffer 2024) — Paper Notes]] — uses Fourier-basis addition via phase gradient states
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]] — replaces the QFT adder with a Toffoli-based constant adder for $2n + 2$ qubit Shor's algorithm; avoids rotation synthesis overhead

### Trick cards
- [[Arithmetic in the Fourier Basis]] — the core technique from this paper
- [[Fourier State as Eigenstate of Addition]] — the phase kickback property underlying the construction
- [[Commutativity of Fourier-Basis Addition for Parallelism]] — parallelisation of the addition step
- [[Phase Gradient Rotation Synthesis]] — downstream application: uses addition into a Fourier state for rotation synthesis
- [[Phase Gradient State for Controlled Rotations]] — the resource state that makes addition-based rotation work; directly descends from the Draper adder idea
