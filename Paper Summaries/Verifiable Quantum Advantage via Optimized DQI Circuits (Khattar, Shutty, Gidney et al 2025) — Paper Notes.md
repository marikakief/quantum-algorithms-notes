> **Source:** Khattar, Shutty, Gidney, Zalcman, Yosri, Maslov, Babbush, Jordan, *Verifiable Quantum Advantage via Optimized DQI Circuits*, arXiv:2510.10967 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2510.10967)
> **Tags:** #quantum-advantage #quantum-optimization #DQI #Reed-Solomon #coding-theory #resource-estimation #finite-fields #fault-tolerant

---

## The computational problem

The target problem is [[Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) — Paper Notes|Decoded Quantum Interferometry]] applied to [[Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) — Paper Notes|Optimal Polynomial Intersection]] over binary extension fields.

For $q = 2^b$, an OPI instance gives a target set $F_y \subseteq \mathbb{F}_q$ for each nonzero $y \in \mathbb{F}_q^\ast$. The task is to find a polynomial $Q \in \mathbb{F}_q[y]$ of degree at most $n-1$ maximizing

$$
f_{\mathrm{OPI}}(Q) =
\left|\{y \in \mathbb{F}_q^\ast : Q(y) \in F_y\}\right|.
$$

The paper also defines **Twisted Bent Target OPI (TBT-OPI)**. Let $S_k \subseteq \mathbb{F}_2^{2k}$ be the Maiorana-McFarland bent set

$$
S_k =
\left\{(x_1,\ldots,x_{2k}) :
\sum_{i=1}^{k} x_i x_{i+k} = 1 \pmod 2
\right\}.
$$

For each evaluation point $\alpha$, choose an independent random invertible linear map $A \in GL_{2k}(\mathbb{F}_2)$ and set $F_\alpha = \phi^{-1}(A \phi(S_k))$. Given rate $R$ and target fraction $\mu$, $(R,\mu)$-TBT-OPI asks for a polynomial of degree at most $\lceil Rm\rceil$ hitting at least $\lfloor \mu m\rfloor$ target sets, where $m = 2^{2k}-1$.

## What the paper does

This is the engineering and hardness follow-up to [[Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) — Paper Notes|Optimization by Decoded Quantum Interferometry]]. It turns DQI+OPI from a conceptual speedup into a sharper candidate for verifiable quantum advantage: binary extension-field arithmetic, lower-qubit DQI state preparation, reversible Reed-Solomon decoding via the [[In-Register Quotient Storage for Reversible EEA|Extended Euclidean Algorithm]], and a classical-attack analysis tailored to extension fields.

The headline theorem is asymptotic: under the best classical attack the authors know for the constructed OPI family, there is an NP-search / optimization problem requiring $2^N$ classical time but only $\widetilde{O}(N)$ quantum gates. This is a best-known-algorithm statement, not an unconditional lower bound against all classical algorithms.

The concrete estimate is also the point. For an OPI instance $(m,n,b,r) = (4095,70,12,2016)$, the paper estimates $5.72 \times 10^6$ Toffoli gates, $1.52 \times 10^8$ Clifford gates, $1885$ logical qubits, about $1.97 \times 10^{23}$ Extended Prange trials, and about $800{,}000$ physical qubits for roughly one hour under the paper's surface-code assumptions.

## The algorithm / construction

### DQI circuit with lower qubit count

The original DQI construction prepares a superposition over error vectors $e \in \mathbb{F}_q^m$ and their syndromes $B^T e$, which costs an $mb$-qubit error register. This paper removes that large register.

The revised DQI circuit uses four stages:

1. **Sparse Dicke state preparation.** Instead of an $m$-qubit dense Hamming-weight register, prepare a sparse list of at most $\ell$ error locations using $\ell \lceil \log_2 m\rceil$ qubits. See [[Sparse Dicke State Preparation by Combinatorial Unranking]].
2. **Sequential syndrome accumulation.** Sweep through $i=1,\ldots,m$, test whether $i$ is in the sparse locator list, generate one $b$-qubit error value $e_i$, add $B_i^T e_i$ into the syndrome, then measure-uncompute $e_i$ in the $X$ basis. The error register is reused at every step. See [[Sequential Syndrome Accumulation with Measurement-Based Error Uncomputation]] and [[Unary Iteration]].
3. **Reversible Reed-Solomon decoding.** Measurement-based uncomputation leaves a known phase $(-1)^{c \cdot e}$. A reversible RS decoder recovers each decoded error term from the syndrome and fixes the phase. This is a refinement of [[Uncomputation via Syndrome Decoding]]: the decoder no longer clears a live error register; it supplies phase fixups after the register has already been measured.
4. **Inverse QFT.** The syndrome register is the Fourier transform of the desired DQI output state; applying the inverse QFT over $\mathbb{F}_q^n$ produces $\sum_x P(f(x))|x\rangle$.

The space target is $2nb + O(\log^2 n)$ qubits: roughly one $nb$-qubit syndrome register plus another $nb$-scale workspace for decoding.

### Reversible EEA for Reed-Solomon decoding

For OPI, $B$ is a Vandermonde matrix and the dual code is a Reed-Solomon code. The decoder receives the syndrome polynomial $S(z)$ and solves the RS key equation

$$
\sigma(z)S(z) \equiv \Omega(z) \pmod {z^{2\ell}},
$$

where $\sigma$ is the error-locator polynomial and $\Omega$ is the error-evaluator polynomial. Running EEA on $A(z)=z^{2\ell}$ and $B(z)=S(z)$ gives

$$
A(z)u_i(z)+B(z)v_i(z)=r_i(z),
$$

and, modulo $A(z)$,

$$
S(z)v_i(z)\equiv r_i(z) \pmod {z^{2\ell}}.
$$

When the algorithm stops at the first $i$ with $\deg r_i < \ell$, this gives $\sigma(z) \propto v_i(z)$ and $\Omega(z) \propto r_i(z)$.

The paper gives two EEA circuit models:

| Model | Main idea | EEA qubit cost | EEA dominant gate cost | Best use here |
|---|---|---:|---:|---|
| Explicit Bezout | Store $\sigma,\Omega$ explicitly with quotient storage inside shared registers | $2nb+O(\log^2 n)$ | $6n^2$ field multiplications | Best for binary extension-field OPI at the paper's sizes |
| Implicit Bezout | Store a [[Dialog Representation for Implicit Bezout Access|Dialog]]: transition data from a division-free EEA | $2nb+O(\log^2 n)$ | $2n^2$ field multiplications | Good when materializing coefficients is not needed |

For the full RS decoder, the tradeoff changes because Chien search and Forney evaluation dominate when $m \gg n$. In binary extension fields, quantum-classical multiplication is Clifford-only, while quantum-quantum multiplication costs Toffolis. That is why the explicit model wins for the concrete OPI estimates even though the implicit EEA alone has lower multiplication count.

### Classical attacks

The baseline classical attack is Prange's algorithm: choose $n$ constraints, solve the resulting linear system, and hope many of the other constraints are hit at random.

The paper then introduces the extension-field attack [[Extended Prange Attack over Extension Fields]]. Over $\mathbb{F}_{p^b}$, a trial can spend a budget of $nb$ base-field linear constraints across coordinates. Spending $s$ constraints on one coordinate restricts the value to an affine subspace of codimension $s$, and the success bias is controlled by the largest overlap of that affine subspace with the target set. The optimal allocation can be relaxed to a linear program for expectation or attacked with a knapsack-style dynamic program for tail probability.

The target sets matter. For the Maiorana-McFarland bent set $S_k$, the paper proves affine-intersection bounds that limit the bias available to XP:

$$
|A \cap S_k| \le
\begin{cases}
2^d & d < k,\\
2^{d-1}+2^{k-2} & k \le d < 2k-1,\\
2^{2k-2} & d = 2k-1,\\
2^{2k-1}-2^{k-1} & d = 2k.
\end{cases}
$$

This is the reason for the [[Twisted Bent Target Sets for OPI Hardness]] construction.

## Key results

**Theorem I.1.** There is an NP-search / optimization problem where the runtime of the best-known classical algorithm is $2^N$ and which can be solved with a circuit of $\widetilde{O}(N)$ quantum gates.

Assessment: the asymptotic quantum side is clean, but the classical side is "best known attack", not a proof that every classical algorithm needs $2^N$ time.

**Theorem III.4.** For the Maiorana-McFarland bent set $S_k$, affine intersections obey the four-case bound above. This is the technical core behind the XP resistance claim.

**Theorem III.7.** There is a constant $R \approx 1/10$ such that, with

$$
\mu_{\mathrm{DQI}}
= \frac12 + \sqrt{\frac{R}{2}\left(1-\frac{R}{2}\right)},
$$

the number of XP trials required to solve $(R,\mu)$-TBT-OPI is at least $\exp(0.02m)$. In the proof, for $R \approx 0.10557$, the Hoeffding bound gives a lower bound of about $\exp(0.02786m)$ trials.

**Lemma IV.1.** Multipoint polynomial evaluation and half-distance RS decoding can be done reversibly using $\widetilde{O}(m)$ field operations over $\mathbb{F}_q$.

**Theorem IV.2.** For OPI over $\mathbb{F}_q$ with $m$ constraints and $n=Rm$, DQI uses $\widetilde{O}(m)$ elementary gates if:

1. $\log q = O(\mathrm{polylog}(m))$.
2. Each objective oracle $U_i|x\rangle = f_i(x)|x\rangle$ costs $\widetilde{O}(1)$ gates.

**Corollary IV.1.** For general OPI with $m=q-1$ and arbitrary target sets $F_i$, DQI costs $\widetilde{O}(m^2)$ gates because generic constraint encoding costs $\widetilde{O}(m)$ per constraint.

**Concrete logical resource estimates.**

| Instance $(m,n,b,r)$ | Toffolis | Cliffords | Logical qubits | Prange trials | XP trials |
|---|---:|---:|---:|---:|---:|
| $(1023,80,10,496)$ | $4.52\times 10^6$ | $6.04\times 10^7$ | $1769$ | $4.30\times 10^{24}$ | $1.22\times 10^{18}$ |
| $(4095,60,12,2016)$ | $4.64\times 10^6$ | $1.27\times 10^8$ | $1640$ | $2.02\times 10^{23}$ | $4.02\times 10^{20}$ |
| $(4095,70,12,2016)$ | $5.72\times 10^6$ | $1.52\times 10^8$ | $1885$ | $4.75\times 10^{26}$ | $1.97\times 10^{23}$ |
| $(4095,100,12,2016)$ | $9.75\times 10^6$ | $2.41\times 10^8$ | $2606$ | $2.10\times 10^{36}$ | $5.91\times 10^{30}$ |

Physical estimate for $(4095,70,12,2016)$: roughly $800{,}000$ physical qubits and roughly one hour. Assumptions: surface code, nearest-neighbour square grid, $0.1\%$ uniform gate error, $1\,\mu$s surface-code cycle, $10\,\mu$s control reaction time, distance-21 hot patches, 2D yoked surface-code cold storage, and in-place magic state cultivation.

## Comparison with prior work

| Work | Problem | Classical-vs-quantum story | This paper's change |
|---|---|---|---|
| [[Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) — Paper Notes|Jordan, Shutty, Wootters, Babbush et al. (2024)]] | DQI for max-LINSAT, OPI, max-XORSAT | Superpolynomial candidate via OPI; first estimates around $10^8$ Toffolis for prime-field OPI | Moves to binary extension fields, cuts Toffolis to single-digit millions, and analyzes XP |
| [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] | Factoring / discrete log | Verifiable and useful, but gate cost for classical hardness $2^N$ is not $\widetilde{O}(N)$ under GNFS/Pollard comparisons | DQI+TBT-OPI claims $\widetilde{O}(N)$ gates for best-known $2^N$ classical time |
| Regev (2025) / Jacobi factoring | Factoring variants | Improves asymptotic quantum factoring structure | Used as the main "could Shor also become optimal?" comparison |
| [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes|Qualtran]] | Resource-estimation framework | Provides reusable call-graph/gate-count machinery | Used to implement and cost the DQI subroutines |
| Chailloux-Tillich (2025) | Quantum advantage from soft decoders | Uses stronger decoding to push DQI-style advantage | Cited as a future route beyond Berlekamp-Massey half-distance decoding |
| Gu-Jordan (2025) | Algebraic-geometry-code DQI | Smaller alphabets / better code families may improve DQI | Concurrent route for shrinking syndrome-register costs |

## Limits / caveats

1. **The classical hardness is not unconditional.** The theorem uses the runtime of the best-known classical algorithm. A new algebraic attack on OPI would change the claim.
2. **XP is stronger than Prange over extension fields.** The initial binary-extension-field win is partly eaten by XP; the paper has to choose target sets carefully.
3. **Target-set design is still not principled.** The authors say they do not know the ultimate limits of choosing $F_y$ and do not yet have a principled target-set optimizer.
4. **The $\widetilde{O}(m)$ theorem needs structured objectives.** Arbitrary OPI target sets cost $\widetilde{O}(m^2)$ because generic state/oracle preparation dominates.
5. **Physical estimate assumptions are narrow.** The one-hour / $800{,}000$-qubit estimate depends on the surface-code model, yoked cold storage, cultivation assumptions, routing overhead multiplier, and the stated $0.1\%$ physical error rate.
6. **Explicit EEA wins only in this parameter regime.** The Dialog representation is elegant, but for binary extension-field OPI the Chien/Forney stages make implicit access Toffoli-heavy.
7. **Verification is easy only after a candidate solution is returned.** The problem remains verifiable because a polynomial can be checked classically against target sets, but producing the polynomial is the hard part.

## Reusable ideas

1. [[Sparse Dicke State Preparation by Combinatorial Unranking]] — encode a $k$-subset as a sorted list of indices rather than an $m$-bit Hamming-weight string; useful when $k \ll m$.
2. [[Sequential Syndrome Accumulation with Measurement-Based Error Uncomputation]] — avoid an $mb$-qubit error register by generating one error symbol at a time, accumulating its syndrome contribution, and measuring it away.
3. [[In-Register Quotient Storage for Reversible EEA]] — use padding inside shared Euclidean registers to store quotients, making the $2nb+O(\log^2 n)$ space target deterministic.
4. [[Dialog Representation for Implicit Bezout Access]] — represent EEA output as a sequence of small transition matrices rather than explicit Bezout coefficients.
5. [[Extended Prange Attack over Extension Fields]] — model extension-field information-set attacks as budgeted affine-subspace restrictions over the base field.
6. [[Twisted Bent Target Sets for OPI Hardness]] — random linear images of Maiorana-McFarland bent sets limit affine-subspace overlaps and make XP less effective.
7. [[DQI Semicircle Law for Optimization-Decoding Duality]] — still the performance bridge from RS decoding radius to OPI approximation quality.

## References within this paper

- [[Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) — Paper Notes|Jordan, Shutty, Wootters, Babbush et al. (2024)]] — base DQI framework, max-LINSAT reduction, OPI, and semicircle law.
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — comparison point for verifiable quantum advantage and asymptotic gate cost.
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes|Harrigan, Khattar, Babbush, Rubin et al. (2024)]] — resource-estimation software used for logical costs.
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] — source of [[Unary Iteration]], used in the sequential scan over sparse error locators.
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney (2018)]] — cited for measurement-based uncomputation style.
- [[A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) — Paper Notes|Gidney (2025)]] — related recent arithmetic work from the same circuit-engineering line.
- Berlekamp (2015), Sugiyama et al. (1975), Chien (1964), Forney (1965) — classical Reed-Solomon decoding machinery.
- Kaye-Zalka (2004), Proos-Zalka (2004), Roetteler et al. (2017), Häner et al. (2020) — prior reversible EEA/ECC arithmetic circuits.
- Prange (1962) — information-set decoding baseline for OPI attacks.
- Briaud, Dinur, Ghosal, Jain, Lou, Sahai (2025), arXiv:2509.07276 — algebraic attacks on binary OPI-like polynomial systems.
- Chailloux-Tillich (2025), arXiv:2411.12553 — soft decoders as a route to stronger DQI advantage.
- Gu-Jordan (2025), arXiv:2510.06603 — algebraic-geometry-code DQI.

## Cross-links

### Paper notes

- [[Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) — Paper Notes]]
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) — Paper Notes]]
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]]

### Trick cards

- [[Sparse Dicke State Preparation by Combinatorial Unranking]]
- [[Sequential Syndrome Accumulation with Measurement-Based Error Uncomputation]]
- [[In-Register Quotient Storage for Reversible EEA]]
- [[Dialog Representation for Implicit Bezout Access]]
- [[Extended Prange Attack over Extension Fields]]
- [[Twisted Bent Target Sets for OPI Hardness]]
- [[DQI Semicircle Law for Optimization-Decoding Duality]]
- [[Uncomputation via Syndrome Decoding]]
- [[Unary Iteration]]
