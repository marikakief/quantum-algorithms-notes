# Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017) — Paper Notes

> **Source:** Marc Kaplan, Gaëtan Leurent, Anthony Leverrier, and María Naya-Plasencia, *Quantum Differential and Linear Cryptanalysis*, arXiv:1510.05836v3, IACR Transactions on Symmetric Cryptology (2017)
> **Links:** [arXiv](https://arxiv.org/abs/1510.05836) · [PDF](https://arxiv.org/pdf/1510.05836)
> **Tags:** #quantum-cryptanalysis #symmetric-key #Grover #quantum-walks #differential-cryptanalysis #linear-cryptanalysis

---

## The computational problem

The paper studies how the standard differential, truncated differential, and linear cryptanalytic attacks on block ciphers change when the adversary has a quantum computer.

Setup:

- Block cipher $E$ with block size $n$, key size $k$, and secret key $\kappa^\ast$.
- Encryption oracle $E(x)=E_{\kappa^\ast}(x)$.
- Attack goal: either distinguish $E$ from a random permutation/function, or recover $\kappa^\ast$ faster than exhaustive search.
- Cost measures: data complexity $D$ = oracle queries to the primitive; time complexity $T$ includes both data and quantum post-processing.

They separate two adversary models:

| Model | Oracle access | Meaning here |
|---|---|---|
| Q1 | Classical queries only; quantum computation after data collection | The more conservative model for ordinary deployed ciphers. Data terms do not improve. |
| Q2 | Quantum superposition queries to the keyed primitive | Stronger and mathematically clean; permits quantum speedups in data collection. |

The cryptanalytic inputs are assumed to be known: a good differential characteristic, truncated differential, or linear approximation is given. The paper does **not** give quantum algorithms for finding those characteristics.

## What the paper does

This is an early systematic attempt to quantise *families* of symmetric-key attacks rather than only generic key search. The main message is sharp: simple differential and linear attacks often get a quadratic speedup in the Q2 model, but truncated differentials do not, because their data analysis is collision-finding rather than independent search.

My assessment: the paper is less about new quantum primitives than about getting the accounting right. That matters. It shows why "Grover halves the key length" is an incomplete security story for symmetric-key design, especially when classical attacks depend on data, partial-key filtering, and last-round recovery.

## The algorithm / construction

### 1. Quantum search template with setup and checking

They use [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]] in the setup/checking form from quantum-walk search. If a set $X$ contains a marked fraction $\varepsilon$, and preparing a uniform superposition over $X$ costs $S$ while checking markedness costs $C$, the search cost is

$$
O\!\left(\frac{S+C}{\sqrt{\varepsilon}}\right).
$$

The checking step can itself be quantum. This is the small but useful modelling choice that lets differential key-recovery attacks be written as nested searches; see [[Nested Grover Checking for Cryptanalytic Key Recovery]].

### 2. Simple differential distinguisher

Assume fixed input/output differences $\delta_{\rm in},\delta_{\rm out}$ with

$$
h_S := -\log \Pr_x\left[E(x\oplus\delta_{\rm in}) = E(x)\oplus\delta_{\rm out}\right] < n.
$$

Classically, testing $2^{h_S}$ input pairs gives a distinguisher with

$$
T_C^{\rm s.dist}=D_C^{\rm s.dist}=2^{h_S+1}.
$$

In Q2, search for an $x$ satisfying the differential relation. The marked fraction is $2^{-h_S}$ and each check uses two encryption queries, so

$$
T_{Q2}^{\rm s.dist}=D_{Q2}^{\rm s.dist}=2^{h_S/2+1}.
$$

For a random function, finding such an $x$ with that query count has negligible probability; the proof reduces a successful distinguisher to a too-fast search algorithm, contradicting Zalka optimality of Grover search.

### 3. Simple differential last-round attack

Suppose the differential covers $R$ rounds and $r_{\rm out}$ rounds are appended. Let:

- $D_{\rm fin}$ be the set of final output differences after the appended rounds, $|D_{\rm fin}|=2^{\Delta_{\rm fin}}$;
- $2^{-h_{\rm out}}$ be the probability of back-propagating from $D_{\rm fin}$ to $\delta_{\rm out}$;
- $k_{\rm out}$ be the number of last-round key bits involved;
- $C_{k_{\rm out}}$ be the classical cost of generating compatible partial keys;
- $C^\ast_{k_{\rm out}}$ be the corresponding quantum cost.

Classical attack:

1. Sample $2^{h_S}$ message pairs with input difference $\delta_{\rm in}$.
2. Filter pairs whose ciphertext difference lies in $D_{\rm fin}$; expected surviving count $2^{h_S+\Delta_{\rm fin}-n}$.
3. Generate candidate last-round partial keys for surviving pairs.
4. Complete them by exhaustive search over the remaining key bits.

The time and data complexities are

$$
T_C^{\rm s.att}=2^{h_S+1}+2^{h_S+\Delta_{\rm fin}-n}\left(C_{k_{\rm out}}+2^{k-h_{\rm out}}\right),
$$

$$
D_C^{\rm s.att}=2^{h_S+1}.
$$

Q2 version:

1. Use Grover search to prepare/search among
   $$X=\{x:E(x\oplus\delta_{\rm in})\oplus E(x)\in D_{\rm fin}\}.$$
2. Check a candidate $x$ by generating compatible partial keys, then Grover-searching over completed keys.
3. Repeat the check coherently to output the actual key once a good pair is found.

The result is

$$
T_{Q2}^{\rm s.att}=2^{h_S/2+1}+2^{(h_S+\Delta_{\rm fin}-n)/2}\left(C^\ast_{k_{\rm out}}+2^{(k-h_{\rm out})/2}\right),
$$

$$
D_{Q2}^{\rm s.att}=2^{h_S/2+1}.
$$

Q1 keeps the classical data term but can speed up the key-recovery part:

$$
D_{Q1}^{\rm s.att}=2^{h_S+1},
$$

$$
T_{Q1}^{\rm s.att}=2^{h_S+1}+2^{(h_S+\Delta_{\rm fin}-n)/2}\left(C^\ast_{k_{\rm out}}+2^{(k-h_{\rm out})/2}\right).
$$

For generating all partial keys quantumly, the paper gives the useful bound

$$
C^\ast_{k_{\rm out}}\lesssim 2^{3k_{\rm out}/2-h_{\rm out}}
$$

when the expected number of candidates is $K=2^{k_{\rm out}-h_{\rm out}}$; if $k_{\rm out}<h_{\rm out}$, then $C^\ast_{k_{\rm out}}=2^{k_{\rm out}/2}$.

### 4. Truncated differential distinguisher

Now use sets of differences $D_{\rm in}$ and $D_{\rm out}$, with

$$
|D_{\rm in}|=2^{\Delta_{\rm in}},\qquad |D_{\rm out}|=2^{\Delta_{\rm out}},
$$

and transition probability $2^{-h_T}$ from $D_{\rm in}$ to $D_{\rm out}$.

Classically, structures give many input pairs from few plaintexts. The distinguisher uses $2^{h_T}$ candidate pairs with cost

$$
D_C^{\rm tr.dist}=T_C^{\rm tr.dist}=\max\left\{2^{(h_T+1)/2},\,2^{h_T-\Delta_{\rm in}+1}\right\}.
$$

Quantumly, the pair-finding step is not a pure Grover search. It uses [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis element distinctness]] / collision search inside structures, giving

$$
D_{Q2}^{\rm tr.dist}=T_{Q2}^{\rm tr.dist}=\max\left\{2^{(h_T+1)/3},\,2^{(h_T+1)/2-\Delta_{\rm in}/3}\right\}.
$$

This is the source of the non-quadratic speedup; see [[Collision-Limited Quantum Speedup for Truncated Differentials]].

### 5. Truncated differential last-round attack

Classically,

$$
D_C^{\rm tr.att}=\max\left\{2^{(h_T+1)/2},\,2^{h_T-\Delta_{\rm in}+1}\right\},
$$

$$
T_C^{\rm tr.att}=\max\left\{2^{(h_T+1)/2},\,2^{h_T-\Delta_{\rm in}+1}\right\}+2^{h_T+\Delta_{\rm fin}-n}\left(C_{k_{\rm out}}+2^{k-h_{\rm out}}\right).
$$

Q1 keeps the same data term:

$$
T_{Q1}^{\rm tr.att}=\max\left\{2^{(h_T+1)/2},\,2^{h_T-\Delta_{\rm in}+1}\right\}+2^{(h_T+\Delta_{\rm fin}-n)/2}\left(C^\ast_{k_{\rm out}}+2^{(k-h_{\rm out})/2}\right).
$$

Q2 has two regimes, depending on whether one structure suffices. The paper summarises them as

$$
T_{Q2}^{\rm tr.att}=2^{(h_T+1)/2}\max\left\{2^{-\Delta_{\rm in}/3},\,2^{-(n+1-\Delta_{\rm fin})/6}\right\}
$$

$$
\quad+\max\left\{2^{(h_T+\Delta_{\rm fin}-n)/2},\,2^{(h_T+1)/2-\Delta_{\rm in}}\right\}
\left(C^\ast_{k_{\rm out}}+2^{(k-h_{\rm out})/2}\right).
$$

The paper explicitly notes that this speedup is always less than quadratic.

### 6. Linear cryptanalysis

For a linear approximation with bias $\epsilon$,

$$
\Pr\left[C[\chi_C]=P[\chi_P]\oplus K[\chi_K]\oplus\chi_0\right]=\frac{1+\epsilon}{2},
$$

classical distinguishing needs

$$
D_C^{\rm lin.dist}=T_C^{\rm lin.dist}=1/\epsilon^2.
$$

In Q2, [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes|quantum counting]] estimates the bias with

$$
D_{Q2}^{\rm lin.dist}=T_{Q2}^{\rm lin.dist}=1/\epsilon.
$$

For Matsui Algorithm 1 with $\ell$ independent relations,

$$
T_C^{\rm Mat.1}=\ell/\epsilon^2+2^{k-\ell},
$$

$$
T_{Q1}^{\rm Mat.1}=\ell/\epsilon^2+2^{(k-\ell)/2},
$$

$$
T_{Q2}^{\rm Mat.1}=\ell/\epsilon+2^{(k-\ell)/2}.
$$

For Matsui Algorithm 2 with $k_{\rm out}$ last-round key bits,

$$
T_C^{\rm Mat.2}=2^{k_{\rm out}}/\epsilon^2+2^{k-k_{\rm out}},
$$

$$
T_{Q1}^{\rm Mat.2}=1/\epsilon^2+2^{k_{\rm out}/2}/\epsilon+2^{(k-k_{\rm out})/2},
$$

$$
T_{Q2}^{\rm Mat.2}=2^{k_{\rm out}/2}/\epsilon+2^{(k-k_{\rm out})/2}.
$$

The reusable idea is [[Quantum Counting for Linear-Cryptanalysis Bias Detection]].

## Key results

### Theorem-style summary of the paper's main complexity bounds

| Attack family | Classical | Q1 | Q2 |
|---|---:|---:|---:|
| Simple differential distinguisher | $2^{h_S+1}$ | no data-speedup variant | $2^{h_S/2+1}$ |
| Simple differential last-round | $2^{h_S+1}+2^{h_S+\Delta_{\rm fin}-n}(C_{k_{\rm out}}+2^{k-h_{\rm out}})$ | $2^{h_S+1}+2^{(h_S+\Delta_{\rm fin}-n)/2}(C^\ast_{k_{\rm out}}+2^{(k-h_{\rm out})/2})$ | $2^{h_S/2+1}+2^{(h_S+\Delta_{\rm fin}-n)/2}(C^\ast_{k_{\rm out}}+2^{(k-h_{\rm out})/2})$ |
| Truncated differential distinguisher | $\max\{2^{(h_T+1)/2},2^{h_T-\Delta_{\rm in}+1}\}$ | no data-speedup variant | $\max\{2^{(h_T+1)/3},2^{(h_T+1)/2-\Delta_{\rm in}/3}\}$ |
| Linear distinguisher | $1/\epsilon^2$ | no data-speedup variant | $1/\epsilon$ |
| Matsui Algorithm 1 | $\ell/\epsilon^2+2^{k-\ell}$ | $\ell/\epsilon^2+2^{(k-\ell)/2}$ | $\ell/\epsilon+2^{(k-\ell)/2}$ |
| Matsui Algorithm 2 | $2^{k_{\rm out}}/\epsilon^2+2^{k-k_{\rm out}}$ | $1/\epsilon^2+2^{k_{\rm out}/2}/\epsilon+2^{(k-k_{\rm out})/2}$ | $2^{k_{\rm out}/2}/\epsilon+2^{(k-k_{\rm out})/2}$ |

### Application: LAC

For reduced LBlock in LAC:

- simple differential: $h_S\approx 61.5$, so classical cost $2^{62.5}$ and Q2 cost $2^{31.75}$;
- truncated differential: parameters $n=64$, $\Delta_{\rm in}=12$, $\Delta_{\rm out}=20$, $\tilde h_T\approx55.3$.

They estimate the truncated-differential classical attack at $2^{60.9}$, better than the simple differential classically. In the quantum setting, the simple differential wins: $2^{31.75}$ versus $2^{33.4}$ for the truncated attack. So the best classical attack need not quantise to the best quantum attack.

### Application: KLEIN

For KLEIN-64, using the Lallemand--Naya-Plasencia truncated last-round attack:

$$
h_T=69.5,\quad \Delta_{\rm in}=16,\quad \Delta_{\rm fin}=32,\quad k=64,\quad k_{\rm out}=32,\quad n=64,
$$

$$
C_{k_{\rm out}}=2^{20},\quad h_{\rm out}=45.
$$

The classical attack has $D=2^{54.5}$ and $T\approx 2^{58.2}$, below $2^{64}$. Quantumly, the relevant term becomes $2^{34.75}$, worse than Grover key search at $2^{32}$, so the attack no longer beats the generic quantum attack.

For KLEIN-96, with parameters

$$
h_T=78,\quad \Delta_{\rm in}=32,\quad \Delta_{\rm fin}=32,\quad k_{\rm out}=48,\quad n=64,
$$

$$
C_{k_{\rm out}}=2^{30},\quad h_{\rm out}=52,
$$

the Q2 attack still beats $2^{48}$, and the Q1 attack barely beats it when round-cost constants are included.

## Comparison with prior work

| Work | What it covered | Relation to this paper |
|---|---|---|
| [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] | Generic unstructured search | Baseline quantum exhaustive key search at $2^{k/2}$; also used inside attack constructions. |
| [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2007)]] | Collision finding in $O(N^{2/3})$ queries | Explains why truncated differentials have smaller-than-quadratic speedup. |
| [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes|Brassard-Høyer-Tapp (1998)]] / [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|BHMT (2002)]] | Amplitude amplification and quantum counting | Used for bias estimation in linear cryptanalysis and for search composition. |
| [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] | Period-finding separation later used in symmetric attacks | Related Q2 attacks on Feistel, Even--Mansour, related-key settings, and modes use Simon-style structure, not the quantum-walk template here. |
| [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes|Grassl-Langenberg-Roetteler-Steinwandt (2015)]] | Concrete Grover resources for AES | Complementary: resource estimate for generic search; this paper studies structural cryptanalytic speedups. |
| [[Estimating the Cost of Generic Quantum Pre-Image Attacks on SHA-2 and SHA-3 (Amy-Di Matteo-Gheorghiu-Mosca-Parent-Schanck 2016) — Paper Notes|Amy et al. (2016)]] | Surface-code cost model for generic hash pre-image Grover attacks | Same warning about black-box query counts being only part of the story. |

## Limits / caveats

- The paper assumes the differential characteristics and linear approximations are already known. Finding them may itself be hard, and the paper leaves quantum speedups for that search as an open question.
- Q2 attacks require superposition access to a keyed primitive. That is mathematically useful but physically and protocol-wise stronger than many threat models.
- The attacks are asymptotic and mostly suppress constants. The KLEIN-96 example shows constants can decide whether an attack barely beats Grover.
- The analysis covers basic versions of differential, truncated differential, and linear attacks. It does not treat the more optimised linear-cryptanalysis variants with counter distillation and FFT-based analysis.
- A cipher classically resistant to these differential/linear attacks remains resistant to the corresponding quantum-walk-based versions in the paper: the quantum cost is at least the square root of the classical cost.

## Reusable ideas

1. [[Q1-Q2 Adversary Split for Quantum Cryptanalysis]] — separate data collection from quantum post-processing; do not silently grant superposition access.
2. [[Nested Grover Checking for Cryptanalytic Key Recovery]] — express last-round key recovery as Grover over filtered pairs with a quantum checking routine for partial-key completion.
3. [[Collision-Limited Quantum Speedup for Truncated Differentials]] — when the classical attack gets its data saving from structures and collisions, Ambainis-style pair search gives less than a quadratic speedup.
4. [[Quantum Counting for Linear-Cryptanalysis Bias Detection]] — replace $1/\epsilon^2$ Bernoulli samples by $1/\epsilon$ Q2 queries when the cryptographic oracle can be counted coherently.

## References within this paper

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — generic key search and core search subroutine.
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2007)]] — collision/pair search used for truncated differentials.
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes|Brassard-Høyer-Tapp (1998)]] and [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|BHMT (2002)]] — quantum counting and amplification.
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — background for period-finding attacks on symmetric constructions.
- [[Quantum Attacks Against Iterated Block Ciphers (Kaplan 2015) — Paper Notes|Kaplan (2015), *Quantum attacks against iterated block ciphers*]] — generic quantum meet-in-the-middle and dissection attacks on iterated block ciphers; useful context for the time-space accounting here.
- Kuwakado--Morii (2010, 2012) — Simon-type Q2 attacks on 3-round Feistel and quantum Even--Mansour.
- [[A Note on Quantum Related-Key Attacks (Roetteler-Steinwandt 2013) — Paper Notes|Rötteler--Steinwandt (2013)]] — Simon-style quantum related-key attack using coherent XOR-mask queries.
- [[Using Simon's Algorithm to Attack Symmetric-Key Cryptographic Primitives (Santoli-Schaffner 2017) — Paper Notes|Santoli--Schaffner (2017)]] — Simon attacks on 3-round Feistel and CBC-MAC; complementary Q2 model-separation result.
- Biham--Shamir (1990), Knudsen (1994), Matsui (1993) — classical differential, truncated differential, and linear cryptanalysis.
- Lallemand--Naya-Plasencia (2014), Leurent (2015) — concrete KLEIN and LAC applications.
- Zalka (1999) — optimality of Grover search, used to argue soundness of the random-function case.
- Zhandry (2012), Boneh--Zhandry (2013) — Q2 security model context.

## Cross-links

### Paper notes

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]]
- [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes]]
- [[Estimating the Cost of Generic Quantum Pre-Image Attacks on SHA-2 and SHA-3 (Amy-Di Matteo-Gheorghiu-Mosca-Parent-Schanck 2016) — Paper Notes]]
- [[Quantum Attacks Against Iterated Block Ciphers (Kaplan 2015) — Paper Notes]]

### Trick cards

- [[Q1-Q2 Adversary Split for Quantum Cryptanalysis]]
- [[Nested Grover Checking for Cryptanalytic Key Recovery]]
- [[Collision-Limited Quantum Speedup for Truncated Differentials]]
- [[Quantum Counting for Linear-Cryptanalysis Bias Detection]]
