# Estimating the Cost of Generic Quantum Pre-Image Attacks on SHA-2 and SHA-3 (Amy-Di Matteo-Gheorghiu-Mosca-Parent-Schanck 2016) — Paper Notes

> **Source:** Matthew Amy, Olivia Di Matteo, Vlad Gheorghiu, Michele Mosca, Alex Parent, John Schanck, *Estimating the cost of generic quantum pre-image attacks on SHA-2 and SHA-3*, arXiv:1603.09383 (2016)
> **Links:** [arXiv](https://arxiv.org/abs/1603.09383) · [PDF](https://arxiv.org/pdf/1603.09383)
> **Tags:** #quantum-cryptanalysis #Grover #SHA2 #SHA3 #resource-estimation #surface-code #CliffordT

---

## The computational problem

Given a target hash value $y \in \{0,1\}^k$ and a hash function treated as a near-random function
$$
f : \{0,1\}^k \to \{0,1\}^k,
$$
find a pre-image $x$ such that $f(x)=y$.

The paper studies the generic [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]] attack, not a structural cryptanalytic attack on SHA. The predicate is
$$
g(x)=1 \iff f(x)=y,
$$
and one Grover iteration uses:

1. a reversible implementation of $f$;
2. a comparison circuit for $f(x)=y$;
3. uncomputation of $f$;
4. the diffusion reflection on the $k$-bit search register.

For a single expected pre-image, the iteration count is
$$
\left\lfloor \frac{\pi}{4}2^{k/2}\right\rfloor.
$$

The main cost measure is not query complexity. The paper uses **logical-qubit-cycles**: if a fault-tolerant computation uses $\ell$ logical qubits for $\sigma$ surface-code cycles, its cost is
$$
\ell\sigma.
$$
They peg one logical-qubit-cycle to the classical cost of one hash-function invocation. This is a modelling choice, but it is the point of the paper: Grover's $2^{128}$ query count for SHA-256 is not the same as a $2^{128}$ classical work factor once the oracle and error correction are costed.

## What the paper does

It gives concrete surface-code-era estimates for Grover pre-image attacks on SHA-256 and SHA3-256. The headline numbers are close: SHA-256 costs about $2^{166.4}$ logical-qubit-cycles, SHA3-256 about $2^{166.5}$ logical-qubit-cycles, even though the black-box query count is only $\Theta(2^{128})$. These numbers are historical, model-dependent estimates, not lower bounds.

My assessment: this is best read as a cost-model paper rather than a final resource estimate. The SHA circuits and distillation assumptions are dated, but the separation between query count, reversible oracle cost, T-factory throughput, and surface-code cycles is exactly the right way to stop misleading square-root-search slogans.

## The algorithm / construction

### 1. Express pre-image search as a reversible Grover oracle

For SHA-256 or SHA3-256 with $k=256$, prepare
$$
\frac{1}{2^{128}}\sum_{x\in\{0,1\}^{256}} |x\rangle.
$$
The phase oracle is implemented by computing the hash value, comparing it to the target $y$, flipping a phase bit, and uncomputing:
$$
|x\rangle|0\rangle|0\rangle
\mapsto
|x\rangle|f(x)\rangle|[f(x)=y]\rangle
\mapsto
(-1)^{[f(x)=y]}|x\rangle|f(x)\rangle|[f(x)=y]\rangle
\mapsto
(-1)^{[f(x)=y]}|x\rangle|0\rangle|0\rangle.
$$

The diffusion step uses $2|0\rangle\langle 0|-I$ on the search register, implemented with Clifford gates and a multi-controlled NOT. The hash computation dominates the logical circuit, while the T gates dominate the surface-code schedule.

### 2. Cost Grover with polynomial overhead

The paper first isolates the finite-size penalty of polynomial overhead. If the total cost of $\Theta(2^{k/2})$ Grover iterations has the form
$$
\approx k^v 2^{k/2}
$$
logical-qubit-cycles, then an adversary with budget $2^C$ can search $k$ bits when
$$
\frac{k}{2}+v\log_2 k \le C. \tag{1}
$$
Solving the equality gives
$$
k(v,C)=\frac{2v}{\ln 2}\, W\!\left(\frac{2^{C/v}\ln 2}{2v}\right), \tag{2}
$$
where $W$ is the Lambert $W$ function.

This is a useful sanity check: asymptotically Grover gives a factor-2 security reduction, but for realistic $k$ and oracle overhead the advantage is noticeably smaller.

### 3. Build the SHA-256 reversible circuit

For one-block SHA-256, the message schedule expands $16$ input words into $64$ words:
$$
W_i = W_{i-16}+\sigma_0(W_{i-15})+W_{i-7}+\sigma_1(W_{i-2}), \qquad 16\le i\le 63.
$$
The compression function then applies $64$ rounds to eight 32-bit state words.

The reversible design uses:

- in-place adders of the [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro-Draper-Kutin-Moulton]] kind, implementing $(a,b,0)\mapsto(a,a+b,0)$;
- CNOT-only circuits for the rotation/XOR functions $\Sigma_0,\Sigma_1,\sigma_0,\sigma_1$;
- a bitwise majority circuit with two Toffoli gates and one CNOT;
- a bitwise choose circuit rewritten as $a(b\oplus c)\oplus c$, using one Toffoli gate.

They expand Toffolis using a T-depth-3 construction and then optimise with T-par. The full SHA-256 circuit after optimisation has:

| Circuit | T / T$^\dagger$ | P / P$^\dagger$ | H | CNOT | T-depth | Depth | Logical qubits |
|---|---:|---:|---:|---:|---:|---:|---:|
| SHA-256 (optimised) | 228,992 | 72,976 | 6,144 | 4,209,072 | 70,400 | 830,720 | 2,402 |

The stretch computation can run in parallel with the round computation, so it does not add to the reported overall depth.

### 4. Build the SHA3-256 reversible circuit

For pre-image length $k=256$, SHA3-256 reduces to one application of the Keccak-p$[1600,24]$ permutation after padding. The state is a $5\times 5$ array of 64-bit lanes, and a round is
$$
R_i = \iota_i\circ \chi\circ \pi\circ \rho\circ \theta.
$$

Rather than use Bennett history storage for all 24 rounds, the paper exploits the fact that each component is invertible. Each round uses one 1600-bit temporary register and the pattern

```text
A ── θ ── ρ,π ── χ ── ι_i ── output
│                         │
└──────── θ^{-1},χ^{-1} ──┘   clears temporary state
```

Here $\rho$ and $\pi$ are wire relabellings, not physical swaps. The only non-linear part is $\chi$ and $\chi^{-1}$, implemented with Toffoli gates and then T-par optimised.

The full SHA3-256 oracle circuit after optimisation has:

| Circuit | X | P / P$^\dagger$ | T / T$^\dagger$ | H | CNOT | T-depth | Depth | Logical qubits |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| SHA3-256 (optimised) | 85 | 46,080 | 499,200 | 168,960 | 34,260,480 | 432 | 11,040 | 3,200 |

The contrast with SHA-256 is interesting: SHA3-256 has far lower T-depth but much higher T-width. That shifts the cost from long serial depth to many parallel T factories.

### 5. Add surface-code and magic-state cost

They assume:

- surface-code computation;
- physical gate error $p_g=10^{-5}$;
- magic-state injection error $p_{\mathrm{in}}=10^{-4}$;
- Reed-Muller 15-to-1 T-state distillation;
- balanced logical/distillation error contribution $\varepsilon=1$;
- one surface-code cycle costs about one classical hash invocation.

For the full Grover computation, the output error per magic state must be at most
$$
p_{\mathrm{out}}\le \frac{1}{T^c_{GA}}.
$$
For both SHA-256 and SHA3-256 this leads to three distillation layers with surface-code distances
$$
\{33,13,7\}.
$$
One distillery uses $3600$ logical qubits, has bottom-layer physical footprint about $5.54\times 10^5$ physical qubits, and produces roughly four magic states every $530$ surface-code cycles.

The difference between SHA-256 and SHA3-256 is factory throughput:

- SHA-256 has average T-width about $3$, so one distillery keeps up.
- SHA3-256 has average T-width $1175$, so about $294$ distilleries are needed to keep each T-depth layer to $530$ cycles.

## Key results

### Grover-level query count

For $k=256$, both attacks use
$$
\left\lfloor \frac{\pi}{4}2^{128}\right\rfloor
$$
Grover iterations.

### SHA-256 T-counts

The SHA-256 oracle $U_g$ uses two SHA-256 evaluations plus a 256-fold comparison:
$$
T^c_{U_g}=2T^c_{\mathrm{SHA\text{-}256}}+T^c_{\mathrm{CNOT\text{-}256}}
=2\cdot 228{,}992+8{,}108=466{,}092.
$$
Adding the diffusion controlled-NOT gives one Grover iteration:
$$
T^c_G=466{,}092+8{,}076=474{,}168.
$$
The full attack has
$$
T^c_{GA}\approx 1.27\times 10^{44}.
$$

Surface-code result:
$$
\sigma_{GA}=\left\lfloor \frac{\pi}{4}2^{128}\right\rfloor\cdot 530\cdot (2T^d_{\mathrm{SHA\text{-}256}})
\approx 2^{153.8}
$$
cycles, and
$$
(N_{\mathrm{SHA\text{-}256}}+\Phi N_{\mathrm{dist}})\sigma_{GA}
=(2402+1\cdot 3600)2^{153.8}
\approx 2^{166.4}
$$
logical-qubit-cycles.

The resulting overhead exponent is
$$
v=\frac{166.4-128}{\log_2 256}=4.8.
$$

### SHA3-256 T-counts

For SHA3-256,
$$
T^c_{U_g}=2\cdot 499{,}200+32\cdot 256-84=1{,}006{,}508,
$$
$$
T^c_G=1{,}006{,}508+32\cdot 255-84=1{,}014{,}584,
$$
and
$$
T^c_{GA}\approx 2.71\times 10^{44}.
$$

The cycle count is
$$
\sigma_{GA}=\left\lfloor \frac{\pi}{4}2^{128}\right\rfloor\cdot 530\cdot (2T^d_{\mathrm{SHA3\text{-}256}})
\approx 1.22\times 10^{44}\approx 2^{146.5}.
$$
With $294$ distilleries,
$$
(N_{\mathrm{SHA3\text{-}256}}+\Phi N_{\mathrm{dist}})\sigma_{GA}
=(3200+294\cdot 3600)2^{146.5}
\approx 2^{166.5}.
$$
The overhead exponent is
$$
v=\frac{166.5-128}{\log_2 256}=4.81.
$$

### Summary table

| Quantity | SHA-256 | SHA3-256 |
|---|---:|---:|
| Full Grover T-count | $1.27\times 10^{44}$ | $2.71\times 10^{44}$ |
| Full Grover T-depth | $3.76\times 10^{43}$ | $2.31\times 10^{41}$ |
| Hash logical qubits | 2,402 | 3,200 |
| Surface-code distance | 43 | 44 |
| Hash physical qubits | $1.39\times 10^7$ | $1.94\times 10^7$ |
| Distilleries | 1 | 294 |
| Logical qubits per distillery | 3,600 | 3,600 |
| Distillery distances | $\{33,13,7\}$ | $\{33,13,7\}$ |
| Total logical qubits | $2^{12.6}$ | $2^{20}$ |
| Surface-code cycles | $2^{153.8}$ | $2^{146.5}$ |
| Total cost | $2^{166.4}$ | $2^{166.5}$ |

## Comparison with prior work

| Work | Target | Cost layer | Main limitation / relation |
|---|---|---|---|
| [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] | Black-box unstructured search | Query complexity | Gives $\Theta(2^{k/2})$ query count, no oracle or fault-tolerance cost. |
| [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] | General amplitude amplification | Query complexity | Framework behind the search iteration. |
| [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes|Grassl-Langenberg-Roetteler-Steinwandt (2015)]] | AES key search | Logical Clifford+T | Strong logical-resource baseline; does not cost surface-code cycles or T factories. |
| This paper | SHA-256 and SHA3-256 pre-image search | Logical + surface-code cycle model | Adds surface-code and magic-state throughput accounting; estimates cost at $2^{166}$ rather than the $2^{128}$ black-box count. |

The paper also uses the T-par Clifford+T optimisation line from Amy-Maslov-Mosca and the T-depth-3 Toffoli from Amy-Maslov-Mosca-Roetteler. Those are circuit-optimisation inputs, not new algorithmic ingredients here.

The near-equality of the SHA-256 and SHA3-256 area-time estimates is a tradeoff, not a claim that the reversible circuits are similar: SHA3 has much lower T-depth but needs many more T factories to supply its wide T layers.

## Limits / caveats

1. **Not a lower bound.** The authors say this directly. Better SHA circuits, better reversible adders, better T factories, lattice surgery, measurement-based Toffoli gadgets, or different codes can change the constants and exponents.
2. **The logical-qubit-cycle metric is a modelling choice.** Equating one surface-code cycle per logical qubit with one classical hash invocation is useful for comparison, but it is architecture-dependent.
3. **Uniform Clifford distribution is a rough assumption.** The cycle estimates assume Clifford gates are spread evenly across qubits and T-depth layers.
4. **The hash functions are treated generically.** The paper estimates generic pre-image search, not a structural attack exploiting SHA-2 or SHA-3 weaknesses.
5. **Parallel quantum search is penalised in area-time cost.** Appendix B notes that splitting Grover across $2^t$ processors reduces time by $2^{t/2}$ but increases area by $2^t$, so total area-time cost rises by $2^{t/2}$.
6. **Circuit data are from 2016.** As a historical benchmark it is very useful; as a current hardware forecast it needs updating.

## Reusable ideas

1. [[Logical-Qubit-Cycle Costing for Grover Attacks]] — measure generic quantum search by area-time, not only by query count.
2. [[Invertible-Round Factoring for Sponge-Function Oracles]] — implement Keccak-style permutations in place using inverse components rather than Bennett histories.
3. [[Magic-State Throughput Matching for T-Heavy Grover Oracles]] — choose the number of distilleries by T-width, not by total T-count alone.

## References within this paper

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994/1997)]] — motivating contrast: hidden-subgroup cryptanalysis gives polynomial-time attacks on factoring and discrete log.
- Boneh and Lipton (1995), “Quantum cryptanalysis of hidden linear functions” — cited as an early quantum cryptanalysis result; this Zoo entry currently has no arXiv-sourced note in the batch inventory.
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — base search primitive.
- Boyer, Brassard, Høyer, Tapp (1998), “Tight bounds on quantum searching” — exact query bounds for search with unknown number of marked items.
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] — amplitude amplification framework.
- [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes|Grassl-Langenberg-Roetteler-Steinwandt (2015)]] — prior concrete symmetric-key resource estimate.
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro-Draper-Kutin-Moulton (2004)]] — in-place ripple-carry adders used in the SHA-256 reversible circuit.
- Bennett (1973, 1989) — reversible computation and history/uncomputation tradeoffs; used as a contrast to the in-place Keccak construction.
- Bravyi and Kitaev (2005) — magic-state distillation via noisy ancillae.
- Fowler et al. (2012/2013) — surface-code timing, physical-resource assumptions, and distillation-cost inputs.

## Cross-links

### Paper notes

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes]]
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes]]
- [[Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) — Paper Notes]]

### Trick cards

- [[Logical-Qubit-Cycle Costing for Grover Attacks]]
- [[Invertible-Round Factoring for Sponge-Function Oracles]]
- [[Magic-State Throughput Matching for T-Heavy Grover Oracles]]
- [[Block-Cipher Key Search as a Reversible Grover Predicate]]
- [[Checkpointed Key Schedule for Reversible Block-Cipher Oracles]]
