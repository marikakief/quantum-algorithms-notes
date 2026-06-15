# How to Factor 2048 Bit RSA Integers in 8 Hours Using 20 Million Noisy Qubits (Gidney-Ekera 2021)

> **Source:** Craig Gidney and Martin Ekerå, *How to factor 2048 bit RSA integers in 8 hours using 20 million noisy qubits*, arXiv:1905.09749, Quantum 5:433 (2021)
> **Links:** [arXiv](https://arxiv.org/abs/1905.09749) · [Quantum](https://quantum-journal.org/papers/q-2021-04-15-433/)
> **Tags:** #quantum-algorithms #Shor #factoring #discrete-log #resource-estimation #fault-tolerance #quantum-arithmetic

---

## The computational problem

The paper gives compiled resource estimates for the quantum arithmetic core of Shor-style attacks on:

1. **RSA factoring.** Input an $n$-bit RSA modulus $N=pq$ with unknown similarly sized primes. Output $p,q$.
2. **Finite-field discrete logarithms.** Given a generator $g$ of an order-$r$ subgroup of $\mathbb F_p^*$ (equivalently $\mathbb Z_p^*$) for prime $p$, and $x=g^d$, output $d=\log_g x$.

The primary cost measure is not query complexity. It is a fault-tolerant implementation cost under a surface-code model: logical qubits, Toffoli/CCZ count, measurement depth, physical qubits, runtime, and expected spacetime volume after retry risk.

For RSA, the implementation uses [[Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekerå-Håstad 2017) — Paper Notes|Ekerå--Håstad]] rather than vanilla [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor factoring]]. Given random $g\in\mathbb{Z}_N^*$, compute $y=g^{N+1}$ classically, then compute the short logarithm

$$
d = \log_g y \equiv p+q \pmod r.
$$

For random RSA moduli, $r>p+q$ with overwhelming probability, so $d=p+q$ and $p,q$ are the roots of $X^2-dX+N=0$.

## What the paper does

This is the resource-estimation paper that made the headline number credible: RSA-2048 in roughly 8 hours using about 20 million noisy physical qubits, assuming a planar superconducting architecture, physical gate error $10^{-3}$, surface-code cycle time $1\,\mu\mathrm{s}$, and classical reaction time $10\,\mu\mathrm{s}$.

Technically, it is a careful composition of arithmetic optimisations rather than a new quantum algorithm: [[Coset Representation for Approximate Modular Arithmetic]], nested lookup/window arithmetic, [[Oblivious Carry Runway|oblivious carry runways]], [[QROM (Quantum Read-Only Memory)|QROM]], [[Measurement-Based QROM Uncomputation]], CCZ factories, and a spacetime layout tuned for reaction-limited additions. My assessment: the main contribution is not the asymptotic formula; it is the end-to-end accounting discipline. The paper forces all the constants, retry probabilities, and layout bottlenecks into the same model.

## The algorithm / construction

### 1. Replace RSA factoring by short discrete log

For RSA $N=pq$, use the Ekerå--Håstad reduction:

1. Choose random $g\in\mathbb{Z}_N^*$.
2. Compute $y=g^{N+1}\bmod N$ classically.
3. Quantumly compute $d=\log_g y$ with two exponent registers of lengths $2m$ and $m$, where $m\approx n/2$ and $p+q<2^m$.
4. Use lattice post-processing from [[Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekerå-Håstad 2017) — Paper Notes|Ekerå--Håstad]] / Ekerå to recover $d$ with success probability above $99\%$ from a correct run.
5. Recover $p,q$ from $X^2-dX+N=0$.

The quantum function is

$$
f(e_1,e_2)=g^{e_1}y^{-e_2}\bmod N,
$$

with total exponent length

$$
n_e = 3m = 1.5n + O(1),
$$

instead of $2n$ for Shor's original order-finding formulation.

### 2. Start from controlled modular multiplication

A reference modular exponentiation would apply $n_e$ controlled modular multiplications. Each controlled multiplication by a classical constant $k$ is decomposed into two controlled scaled additions:

$$
y \mathrel{+}= xk \pmod N,\qquad x \mathrel{+}= y(-k^{-1}) \pmod N,
$$

followed by a register swap. Each scaled addition is a sequence of controlled modular additions. With exact modular additions built from several ordinary additions, the reference Toffoli count is about

$$
20 n_e n^2.
$$

This is the baseline the paper optimises away.

### 3. Use coset-represented modular integers

Following Zalka, represent $k\bmod N$ as

$$
\frac{1}{\sqrt{2^{c_{\mathrm{pad}}}}}
\sum_{j=0}^{2^{c_{\mathrm{pad}}}-1}\lvert jN+k\rangle.
$$

Then ordinary non-modular addition acts as approximate modular addition. The comparison/subtract machinery disappears, reducing the leading Toffoli term from $20n_en^2$ to $8n_en^2$. The approximation error is handled with the approximate encoded-permutation analysis from [[Approximate Encoded Permutations and Piecewise Quantum Adders (Gidney 2019) — Paper Notes]].

### 4. Apply two-level windowing

The paper windows at two levels:

- **Exponent windowing.** Group $c_{\exp}$ exponent qubits so one multiply handles a table of $2^{c_{\exp}}$ classical constants.
- **Multiplication windowing.** Group $c_{\mathrm{mul}}$ factor bits so multiple controlled additions become one lookup addition.

A lookup addition consists of:

1. QROM lookup of the classically precomputed addend using $c_{\exp}+c_{\mathrm{mul}}$ address bits.
2. Ordinary addition into the target register.
3. Measurement-based uncomputation of the lookup output.

The number of lookup additions is approximately

$$
\operatorname{LookupAdditionCount}(n,n_e)
= \frac{2nn_e}{c_{\exp}c_{\mathrm{mul}}}\cdot \frac{c_{\mathrm{sep}}+1}{c_{\mathrm{sep}}}
+ O\!\left(\frac{n_e\log n}{c_{\exp}c_{\mathrm{mul}}}\right).
$$

For the simple parameter choice $c_{\exp}=c_{\mathrm{mul}}=5$ and $c_{\mathrm{sep}}=1024$, this is about $0.1n_en$.

### 5. Split additions with oblivious carry runways

Insert carry runways every $c_{\mathrm{sep}}$ bits. Each addition is performed piecewise, with each piece running in parallel. The runway length and coset padding are both taken as

$$
c_{\mathrm{pad}} = 2\lg n + \lg n_e + 10
$$

in the simple analysis, because both error mechanisms are unwanted carries into padding. Carry-runway removal at the end can be skipped quantumly: measure the encoded register and decode classically.

The paper also notes an interaction cost: when a register with runways is used as the multiplicand rather than the target, the runways must be temporarily reduced to single carry qubits. This adds lookup-addition work but keeps the layout usable.

### 6. Layout the lookup-addition loop in surface code

The repeated unit is:

```text
QROM lookup  →  ripple add through a moving operating area  →  measurement unlookup
```

The target and lookup-output rows are interleaved. CCZ factories feed AutoCCZ states into a Cuccaro-style MAJ/UMA sweep. With $d=27$, level-1 distance $17$, and $c_{\mathrm{sep}}=1024$, a 2048-bit computation fits roughly into a $226\times 63$ logical-qubit rectangle in the simplified analysis. The final title number comes from the ancillary optimiser rather than these rounded values.

### 7. Optimise physical parameters and include retries

For each $n,n_e$, the optimiser scans:

$$
d_1\in\{15,17,\ldots,23\},\quad d_2\in\{25,27,\ldots,51\},
$$

$$
c_{\mathrm{mul}},c_{\exp}\in\{4,5,6\},\quad
c_{\mathrm{sep}}\in\{512,768,1024,1536,2048\},
$$

and padding offsets $\delta_{\mathrm{off}}=c_{\mathrm{pad}}-2\lg n-\lg n_e\in\{2,\ldots,10\}$. It estimates topological error, approximation error, distillation error, and classical post-processing failure, then reports expected cost after retries. The optimisation target is skewed expected spacetime volume

$$
s^{1.2}\,\frac{t}{1-\epsilon},
$$

where $s$ is physical qubits, $t$ is per-run runtime, and $\epsilon$ is retry risk.

## Key results

### Abstract circuit model

For general $n$ and total exponent length $n_e$, the paper's simple estimates are

$$
\operatorname{ToffoliCount}(n,n_e)
\approx 0.2n_en^2 + 0.0003 n_en^2\lg n,
$$

$$
\operatorname{MeasurementDepth}(n,n_e)
\approx 300n_en + 0.5 n_en\lg n.
$$

For RSA factoring via Ekerå--Håstad, $n_e=1.5n+O(1)$, giving the abstract headline:

$$
\text{logical qubits} = 3n + 0.002n\lg n,
$$

$$
\text{Toffoli+T/2 count} = 0.3n^3 + 0.0005n^3\lg n,
$$

$$
\text{measurement depth} = 500n^2+n^2\lg n.
$$

### RSA-2048 physical estimate

Under the paper's main physical assumptions (source table for the headline RSA-2048 estimate):

| Task | Physical qubits | Runtime per run | Retry risk | Expected volume |
|---|---:|---:|---:|---:|
| Factor RSA-2048 via Ekerå--Håstad | $20$ million | $5.1$ hours | $31\%$ | $5.9$ megaqubit-days |

The title rounds this to 20 million noisy qubits and 8 hours; the table gives $5.1$ hours per run and $5.9$ expected megaqubit-days after retry risk.

### Finite-field DLP estimates

For Schnorr groups and safe-prime groups with short exponents, the same arithmetic gives much lower runtime than full-length safe-prime DLP because $n_e=6z$ rather than $\Theta(n)$. For $n=2048$ and NIST's $z=112$ security model, the Ekerå/Ekerå--Håstad table reports about $20$ million physical qubits and $1.2$ hours per run for short-exponent/Schnorr settings, versus $26$ million physical qubits and $11$ hours per run for full-length safe-prime DLP under Ekerå's order/log algorithm.

### Approximation error

With $c_{\mathrm{pad}}=2\lg n+\lg n_e+10$ and $c_{\mathrm{sep}}=1024$,

$$
\operatorname{TotalDeviation}(n,n_e)
\leq \operatorname{LookupAdditionCount}(n,n_e)\frac{n}{c_{\mathrm{sep}}2^{c_{\mathrm{pad}}}}
\approx 10^{-7}.
$$

The trace distance from the ideal output is at most $2\sqrt{\epsilon}$, so the approximation error is around $0.1\%$, below the dominant physical error sources.

## Comparison with prior work

### Abstract arithmetic costs for RSA factoring

| Construction | Logical qubits | Measurement depth | Toffoli+T/2 count | Count at $n=2048$ |
|---|---:|---:|---:|---:|
| Vedral--Barenco--Ekert (1996) | $7n+1$ | $80n^3+O(n^2)$ | $80n^3+O(n^2)$ | $690$B |
| Zalka basic (1998) | $3n+O(1)$ | $12n^3+O(n)$ | $12n^3+O(n^2)$ | $100$B |
| Fowler et al. (2012) | $3n+O(1)$ | $40n^3+O(n^2)$ | $40n^3+O(n^2)$ | $340$B |
| Häner--Roetteler--Svore (2016/2017) | $2n+2$ | $52n^3+O(n^2)$ | $64n^3\lg n+O(n^3)$ | $5200$B |
| Gidney--Ekerå (this paper) | $3n+0.002n\lg n$ | $500n^2+n^2\lg n$ | $0.3n^3+0.0005n^3\lg n$ | $2.7$B |

### Surface-code estimates for RSA-2048

| Work | Physical qubits | Expected runtime | Expected volume | Main bottleneck |
|---|---:|---:|---:|---|
| Van Meter et al. (2009) | $6500$M | $410$ days | $2{,}600{,}000$ megaqubit-days | distillation-limited carry lookahead |
| Jones et al. (2010) | $620$M | $10$ days | $6200$ megaqubit-days | distillation-limited carry lookahead |
| Fowler et al. (2012) | $1000$M | $1.1$ days | $1100$ megaqubit-days | reaction-limited ripple carry |
| Gheorghiu--Mosca (2019) | $170$M | $1$ day | $170$ megaqubit-days | reaction-limited ripple carry |
| Gidney--Ekerå, parallel | $20$M | $0.31$ days | $5.9$ megaqubit-days | reaction-limited carry runways |

The comparison is partly model-dependent. The paper is careful about that; changing cycle time or physical gate error can move the volume by large constants. Still, the order-of-magnitude drop against comparable surface-code models is real.

## Limits / caveats

1. **The estimates are ballpark, not forecasts.** The authors explicitly warn that doubling or halving the physical gate error rate moves the physical qubit count by more than $10\%$.
2. **The title hides retry accounting.** The table gives $5.1$ hours per run for RSA-2048, but a $31\%$ retry risk. Expected time is higher; the title's 8 hours is a readable summary rather than a theorem.
3. **The result is architecture-specific.** The surface-code layout assumes a planar grid, lattice surgery, $1\,\mu\mathrm{s}$ cycles, and $10\,\mu\mathrm{s}$ reaction time. Different hardware can reorder the bottlenecks.
4. **ECDLP is not optimised here.** The paper explicitly does not claim RSA/finite-field DLP are easier than elliptic-curve DLP quantumly; it says the same arithmetic optimisation work has not been done for elliptic curves. Roetteler--Naehrig--Svore--Lauter remains the relevant ECDLP comparison.
5. **Approximate arithmetic must be budgeted.** [[Coset Representation for Approximate Modular Arithmetic]] and [[Oblivious Carry Runway|carry runways]] are not exact; the paper wins because their error is small compared with physical failure, not because the issue disappears.
6. **Asymptotically faster multiplication is left open.** The authors are sceptical that Karatsuba or Schönhage--Strassen helps at $n=2048$, because reversible overhead, workspace, and lost compatibility with windowing can erase the asymptotic gain.

## Reusable ideas

1. [[Nested Windowed Modular Exponentiation for Shor Arithmetic]] — fuse exponent windows and multiplication windows into lookup additions so the arithmetic core is QROM-load plus add, repeated $O(n_en/(c_{\exp}c_{\mathrm{mul}}))$ times.
2. [[Retry-Aware Surface-Code Resource Accounting]] — optimise expected spacetime volume by adding topological, distillation, approximation, and post-processing failure into a single retry model.
3. [[Carry-Runway Partitioning for Distributed Modular Arithmetic]] — the carry-runway structure makes additions local to pieces; only lookup address qubits need to be broadcast across pieces.
4. [[Coset Representation for Approximate Modular Arithmetic]] — replace exact modular addition by ordinary addition into a periodic coset state.
5. [[Oblivious Carry Runway]] — cap carry propagation so additions can run piecewise and in parallel.

## References within this paper

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — factoring and discrete-log algorithms based on period finding.
- [[Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekerå-Håstad 2017) — Paper Notes|Ekerå--Håstad (2017)]] — short-DLP factoring reduction used for RSA estimates.
- Ekerå (2017/2020), *On post-processing in the quantum algorithm for computing short discrete logarithms* — lattice post-processing with $>99\%$ success for a correct run.
- Ekerå (2018/2021), *Quantum algorithms for computing general discrete logarithms and orders with tradeoffs* — finite-field DLP/order variants used in Section 3.
- [[Approximate Encoded Permutations and Piecewise Quantum Adders (Gidney 2019) — Paper Notes|Gidney (2019)]] — approximate encoded permutations, coset-error analysis, and oblivious carry runways.
- Gidney (2019), *Windowed quantum arithmetic* — nested windowing of exponentiation and multiplication.
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney (2018)]] and [[Temporary Logical-AND]] — T-count-optimised addition primitives.
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush--Gidney et al. (2018)]] — QROM lookup machinery.
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry--Gidney--Motta--McClean--Babbush (2019)]] — measurement-based QROM uncomputation.
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro--Draper--Kutin--Moulton (2004)]] — ripple-carry MAJ/UMA adder used inside lookup additions.
- [[Quantum Resource Estimates for Computing Elliptic Curve Discrete Logarithms (Roetteler-Naehrig-Svore-Lauter 2017) — Paper Notes|Roetteler--Naehrig--Svore--Lauter (2017)]] — comparison point for ECDLP resource estimates.
- Fowler--Gidney (2018/2019) — low-overhead lattice surgery, CCZ factories, and AutoCCZ layout used in the surface-code model.

## Cross-links

### Paper notes

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekerå-Håstad 2017) — Paper Notes]]
- [[Approximate Encoded Permutations and Piecewise Quantum Adders (Gidney 2019) — Paper Notes]]
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]]
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes]]
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]]
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]]

### Trick cards

- [[Nested Windowed Modular Exponentiation for Shor Arithmetic]]
- [[Retry-Aware Surface-Code Resource Accounting]]
- [[Carry-Runway Partitioning for Distributed Modular Arithmetic]]
- [[Coset Representation for Approximate Modular Arithmetic]]
- [[Oblivious Carry Runway]]
- [[QROM (Quantum Read-Only Memory)]]
- [[Measurement-Based QROM Uncomputation]]
- [[Temporary Logical-AND]]
