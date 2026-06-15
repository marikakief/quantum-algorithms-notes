# Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes

> **Source:** Markus Grassl, Brandon Langenberg, Martin Roetteler, Rainer Steinwandt, *Applying Grover's algorithm to AES: quantum resource estimates*, arXiv:1512.04965 (2015)
> **Links:** [arXiv](https://arxiv.org/abs/1512.04965) · [PDF](https://arxiv.org/pdf/1512.04965)
> **Tags:** #quantum-cryptanalysis #Grover #AES #resource-estimation #CliffordT

---

## The computational problem

Given a small set of known AES plaintext-ciphertext pairs
$$(m_1,c_1),\ldots,(m_r,c_r), \qquad c_i = \operatorname{AES}_K(m_i),$$
recover the secret key $K \in \{0,1\}^k$ for $k \in \{128,192,256\}$ using [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]].

The Boolean predicate used by Grover is
$$
f(K') := \bigwedge_{i=1}^r \left(\operatorname{AES}_{K'}(m_i)=c_i\right).
$$

The paper models the cost at the logical circuit level, mainly counting logical qubits, Clifford gates, T gates, T-depth, and overall depth. The authors use a Clifford+T implementation where each Toffoli costs 7 T gates, with no geometry or error-correction layout constraints.

For uniqueness of the marked key, they use the AES strict avalanche heuristic. For a block size $n=128$, they estimate that
$$
r > \left\lceil \frac{2k}{n}\right\rceil
$$
plaintext-ciphertext pairs are enough to make key collisions implausible. Hence the resource estimates use $r=3,4,5$ for AES-128, AES-192, and AES-256 respectively.

## What the paper does

This is one of the early concrete logical-resource estimates for Grover key search against AES. The result is not a new asymptotic algorithm: it is the square-root exhaustive search attack with a hand-built reversible AES oracle. The useful part is the accounting: AES-128 takes only 2,953 logical qubits in their circuit model, but about $9.24\times 10^{25}$ T gates, so the depth is the real blocker.

My assessment: the paper is valuable as a baseline, not as a final estimate. It deliberately ignores surface-code cost, routing, T factories, measurement/feed-forward variants, and later cheaper reversible arithmetic tricks. Still, it cleanly separates the oracle-design question from the Grover iteration count.

## The algorithm / construction

### 1. Turn AES key recovery into a Grover predicate

Prepare the key register in the uniform superposition over $2^k$ keys and use the standard Grover iterate
$$
G = U_f\left(\left(H^{\otimes k}(2|0\rangle\langle 0|-I_{2^k})H^{\otimes k}\right)\otimes I_2\right),
$$
where
$$
U_f|K'\rangle|y\rangle = |K'\rangle|y\oplus f(K')\rangle.
$$

With a unique marked key, run
$$
\ell = \left\lfloor \frac{\pi}{4}2^{k/2}\right\rfloor
$$
Grover iterations and measure the key register.

The diffusion phase uses a $k$-controlled NOT once per Grover iteration. Using Barenco et al.'s construction, the per-iteration Toffoli counts are:

| Key size $k$ | Toffolis for diffusion phase | T-count with phase cancellations |
|---:|---:|---:|
| 128 | 1,000 | 4,012 |
| 192 | 1,512 | 6,060 |
| 256 | 2,024 | 8,108 |

The predicate comparison after AES uses a $128r$-controlled NOT once per Grover iteration. Its per-iteration Toffoli/T counts are:

| AES variant | $r$ | Controls | Toffolis | T-count |
|---|---:|---:|---:|---:|
| AES-128 | 3 | 384 | 3,048 | 12,204 |
| AES-192 | 4 | 512 | 4,072 | 16,300 |
| AES-256 | 5 | 640 | 5,096 | 20,396 |

### 2. Build reversible AES

The reversible AES box computes
$$
|K\rangle|0\rangle \mapsto |K\rangle|\operatorname{AES}_K(m)\rangle.
$$

The construction uses:

- **AddRoundKey:** 128 CNOTs, parallel depth 1, because the current round key is placed on dedicated wires.
- **ShiftRows:** free at the gate-count level, handled as wire relabelling.
- **MixColumns:** in-place linear operation on each 32-bit column; LUP-style decomposition gives 277 CNOT gates and depth 39 per column operation.
- **SubBytes:** dominant non-Clifford cost. The S-box is implemented by computing inversion in $\mathbb{F}_{256}=\mathbb{F}_2[x]/(1+x+x^3+x^4+x^8)$, then applying the AES affine map.

For the S-box inversion, they use the Itoh-Tsujii identity
$$
\alpha^{-1}=\alpha^{254}=\left((\alpha\cdot \alpha^2)\cdot(\alpha\cdot\alpha^2)^4\cdot(\alpha\cdot\alpha^2)^{16}\cdot\alpha^{64}\right)^2.
$$
All squarings and fixed powers are $\mathbb{F}_2$-linear, so they are CNOT-only linear maps. Multiplications use the Maslov--Mathew--Cheung--Pradhan finite-field multiplier: 64 Toffolis and 21 CNOTs for the AES polynomial basis. After removing duplicate multiplications and uncomputing scratch, one S-box costs:

$$
3584\ \text{T gates} + 4569\ \text{Clifford gates}.
$$

They also describe a 9-qubit S-box alternative via permutation-group synthesis. It uses fewer qubits but costs at most 9,695 T gates and 12,631 Clifford gates, so the main estimates use the 40-qubit Itoh-Tsujii design.

### 3. Store only the expensive key-schedule words

The AES key schedule creates 44, 52, or 60 32-bit words for AES-128/192/256. The authors split these into words involving SubBytes and words obtainable by XORing stored words. Since SubBytes is expensive, they store the words that need it and reconstruct cheap XOR-only words on demand. This saves key-schedule storage at the cost of more CNOTs and some depth.

Key expansion costs:

| AES variant | NOT | CNOT | Toffoli | T gates | Depth | Stored qubits | Ancillae |
|---|---:|---:|---:|---:|---:|---:|---:|
| AES-128 | 128 | 176 | 21,448 | 143,360 | 12,636 | 320 | 96 |
| AES-192 | 192 | 136 | 17,568 | 114,688 | 10,107 | 256 | 96 |
| AES-256 | 256 | 215 | 27,492 | 186,368 | 16,408 | 416 | 96 |

### 4. Reuse AES-round storage by partial uncomputation

Each AES round needs fresh space because SubBytes is not in-place in their circuit. To reduce storage, they periodically reverse earlier steps to clear qubits while leaving enough state to continue. AES-128 uses 536 qubits for the round computation; AES-192 and AES-256 use 664. This is a manual checkpointing strategy for a reversible block-cipher oracle.

Single AES-box totals:

| AES variant | T gates | Clifford gates | T-depth | Overall depth | Qubits |
|---|---:|---:|---:|---:|---:|
| AES-128 | 1,060,864 | 1,380,420 | 50,688 | 110,799 | 984 |
| AES-192 | 1,204,224 | 1,567,296 | 44,352 | 96,956 | 1,112 |
| AES-256 | 1,505,280 | 1,956,099 | 59,904 | 130,929 | 1,336 |

### 5. Compose the Grover oracle

For $r$ plaintext-ciphertext pairs, run $r$ AES boxes in parallel, compare all $128r$ output bits with the target ciphertexts, flip the Grover phase/output qubit, then uncompute the AES boxes. The AES boxes are parallel in depth but counted $2r$ times in gate count because of compute/uncompute; this depth reduction is paid for with multiplied qubit use and total gates.

The total qubit counts are:

$$
3s_{128}+1=2953,\qquad 4s_{192}+1=4449,\qquad 5s_{256}+1=6681.
$$

## Key results

### Reversible AES circuit costs

For a single reversible AES computation $|K\rangle|0\rangle\mapsto |K\rangle|\operatorname{AES}_K(m)\rangle$:

$$
\begin{array}{c|r|r|r|r|r}
\text{variant} & T & \text{Clifford} & T\text{-depth} & \text{depth} & \text{qubits} \\
\hline
\text{AES-128} & 1{,}060{,}864 & 1{,}380{,}420 & 50{,}688 & 110{,}799 & 984 \\
\text{AES-192} & 1{,}204{,}224 & 1{,}567{,}296 & 44{,}352 & 96{,}956 & 1{,}112 \\
\text{AES-256} & 1{,}505{,}280 & 1{,}956{,}099 & 59{,}904 & 130{,}929 & 1{,}336
\end{array}
$$

### Full Grover attack estimates

Using $r=3,4,5$ known plaintext-ciphertext pairs and $\lfloor(\pi/4)2^{k/2}\rfloor$ sequential Grover iterations. These are logical Clifford+T counts with no geometry, routing, surface-code cycles, or magic-state factory accounting:

$$
\begin{array}{c|r|r|r|r|r}
k & T & \text{Clifford} & T\text{-depth} & \text{depth} & \text{qubits} \\
\hline
128 & 1.19\cdot 2^{86} & 1.55\cdot 2^{86} & 1.06\cdot 2^{80} & 1.16\cdot 2^{81} & 2{,}953 \\
192 & 3.75\cdot 10^{36} & 1.17\cdot 2^{119} & 1.21\cdot 2^{112} & 1.33\cdot 2^{113} & 4{,}449 \\
256 & 1.41\cdot 2^{151} & 1.83\cdot 2^{151} & 1.44\cdot 2^{144} & 1.57\cdot 2^{145} & 6{,}681
\end{array}
$$

The table in the extracted PDF text has a likely OCR/typographical inconsistency in the AES-192 binary T-count line; the paper's prose gives $3.75\times 10^{36}$ T gates. I keep that decimal value rather than silently forcing a mismatched power of two.

### S-box cost

The main S-box construction costs
$$
3584\ T + 4569\ \text{Clifford}
$$
per byte and dominates the T-count. Since AES-128 uses 296 SubBytes calls and AES-256 uses 420, almost all non-Clifford cost comes from the S-box.

## Comparison with prior work

| Work | Setting | Quantum access model | What is estimated | Relation to this paper |
|---|---|---|---|---|
| [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] | Unstructured search | Boolean oracle | Query count $O(\sqrt{N/M})$ | Supplies the outer search algorithm |
| Boyer--Brassard--Høyer--Tapp (1998) | Unknown number of marked items | Boolean oracle | Expected $O(\sqrt{N/M})$ search | Mentioned as a fallback if the number of matching keys is not known |
| Roetteler--Steinwandt (2015) | Related-key attacks | Quantum related-key access | Attack model, not AES key-search cost | This paper avoids that stronger access model and uses known plaintext-ciphertext pairs |
| This paper | AES exhaustive key search | Reversible AES predicate | Logical Clifford+T counts, depth, qubits | First detailed logical AES Grover estimate in this line |
| [[Estimating the Cost of Generic Quantum Pre-Image Attacks on SHA-2 and SHA-3 (Amy-Di Matteo-Gheorghiu-Mosca-Parent-Schanck 2016) — Paper Notes|Amy-Di Matteo-Gheorghiu-Mosca-Parent-Schanck (2016)]] | SHA-256/SHA3-256 pre-image search | Reversible hash predicate | Surface-code cycles, T factories, logical-qubit-cycles | Adds the fault-tolerant layer that this AES estimate deliberately omits |

## Limits / caveats

1. **This is a logical-gate estimate only.** It does not include surface-code layout, magic-state factories, routing, classical control, syndrome extraction, or wall-clock scheduling.
2. **No locality constraint.** The authors explicitly leave nearest-neighbour placement and routing for later work.
3. **The uniqueness assumption is heuristic.** The $r=3,4,5$ plaintext-ciphertext counts use strict-avalanche reasoning, not a theorem about AES.
4. **Toffoli decomposition choice matters.** They use the deterministic 7-T Toffoli. Jones's 4-T measurement/feed-forward construction would multiply T-counts by $4/7$ and add one ancilla, but changes architectural assumptions.
5. **The S-box is not final.** Later reversible/quantum AES circuits and better Toffoli/T-count tricks can change constants a lot. The Grover exponent remains the main story.
6. **Depth is serial in Grover.** Oracle parallelism does not remove the $\Theta(2^{k/2})$ sequence of Grover iterations.

## Reusable ideas

1. [[Block-Cipher Key Search as a Reversible Grover Predicate]] — package a block-cipher key-recovery attack as a reversible predicate over candidate keys, using multiple plaintext-ciphertext pairs to isolate one marked key.
2. [[Itoh-Tsujii S-Box via Linear Maps and Reversible Field Multiplication]] — implement an AES-style finite-field inverse by pushing all Frobenius powers into CNOT-only linear maps and spending T gates only on field multiplications.
3. [[Checkpointed Key Schedule for Reversible Block-Cipher Oracles]] — store key-schedule words that are expensive to recreate, reconstruct XOR-only words on demand, and periodically uncompute round state to cap qubit count.

## References within this paper

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — outer search algorithm.
- Boyer, Brassard, Høyer, Tapp (1998) — tight search bounds and unknown-$M$ Grover variant.
- Brassard, Høyer, Mosca, Tapp (2002), [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude amplification and estimation]] — alternative handling of unknown numbers of solutions.
- Barenco et al. (1995) — multi-controlled NOT constructions used for diffusion and comparison gates.
- Amy, Maslov, Mosca, Roetteler (2013) — Clifford+T synthesis and T-count optimisation background.
- Maslov (2015) — relative-phase Toffoli methods for multi-controlled gate optimisation.
- Jones (2013) — 4-T probabilistic Toffoli option, not used in the main estimates.
- NIST FIPS 197 (2001) — AES specification.
- Amento, Roetteler, Steinwandt (2013) — finite-field arithmetic circuits related to the S-box construction.
- Kepley, Steinwandt (2015) — lower-gate finite-field multiplication alternative with more qubits.
- Yoder, Low, Chuang (2014), [[Chebyshev Polynomial Design for Fixed-Point Amplitude Amplification]] — fixed-point search variant mentioned as future work.
- Kaplan, Leurent, Leverrier, Naya-Plasencia (2015) — quantum differential and linear cryptanalysis, flagged as another target for resource estimates.

## Cross-links

### Paper notes

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Estimating the Cost of Generic Quantum Pre-Image Attacks on SHA-2 and SHA-3 (Amy-Di Matteo-Gheorghiu-Mosca-Parent-Schanck 2016) — Paper Notes]]
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]]
- [[Optimizing Quantum Circuits for Arithmetic (Häner-Roetteler-Svore 2018) — Paper Notes]]
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]]

### Trick cards

- [[Block-Cipher Key Search as a Reversible Grover Predicate]]
- [[Itoh-Tsujii S-Box via Linear Maps and Reversible Field Multiplication]]
- [[Checkpointed Key Schedule for Reversible Block-Cipher Oracles]]
- [[Standard Amplitude Amplification]]
- [[Randomised Iteration Count for Grover Search]]
- [[Chebyshev Polynomial Design for Fixed-Point Amplitude Amplification]]
