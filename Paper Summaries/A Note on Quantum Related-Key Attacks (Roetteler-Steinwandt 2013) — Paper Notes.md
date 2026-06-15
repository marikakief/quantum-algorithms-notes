# A Note on Quantum Related-Key Attacks (Roetteler-Steinwandt 2013) — Paper Notes

> **Source:** Martin Rötteler and Rainer Steinwandt, *A note on quantum related-key attacks*, arXiv:1306.2301v2 (2013)
> **Links:** [arXiv](https://arxiv.org/abs/1306.2301) · [PDF](https://arxiv.org/pdf/1306.2301)
> **Tags:** #quantum-cryptanalysis #symmetric-key #related-key #Simon #block-ciphers

---

## The computational problem

The paper studies key recovery for a block cipher under a Q2/coherent related-key oracle that applies XOR masks to the secret key and can be queried in superposition.

A block cipher with key length $k$ and block length $n$ is a family of permutations

$$
\{E_K:\{0,1\}^n\to\{0,1\}^n\}_{K\in\{0,1\}^k}.
$$

A secret key $K\in\{0,1\}^k$ is chosen uniformly. The related-key encryption oracle is

$$
E(L,m)=E_{K\oplus L}(m),
$$

where $L\in\{0,1\}^k$ is a chosen XOR mask. The classical related-key model also gives decryption access

$$
E^{-1}(L,c)=E^{-1}_{K\oplus L}(c),
$$

but the attack in this paper only needs encryption.

The attacker is also given $r$ plaintext-ciphertext pairs

$$
(m_1,c_1),\ldots,(m_r,c_r),\qquad c_i=E_K(m_i),
$$

such that $K$ is the unique key consistent with all pairs. The paper uses the standard known-plaintext unicity estimate

$$
r>\lceil k/n\rceil
$$

as the heuristic reason this is plausible for ordinary block ciphers. Cost is measured as a function of the key length $k$; the block cipher is assumed to have polynomial-size reversible circuits.

## What the paper does

Rötteler and Steinwandt reduce this related-key key-recovery problem to [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon’s problem]]. If the adversary can query the related-key oracle coherently on a superposition of XOR masks, the secret key is recovered in expected polynomial time.

My assessment: this is a clean warning shot rather than a practical break. The assumption of coherent related-key access is very strong, but the reduction is sharp and became part of the standard Simon-style toolkit for symmetric-key quantum attacks.

## The algorithm / construction

Write

$$
\vec m=(m_1,
\ldots,m_r),\qquad E_x(\vec m)=(E_x(m_1),\ldots,E_x(m_r))\in\{0,1\}^{rn}.
$$

Let the unknown secret key be $s$. First test whether $s=0^k$ by computing $E_{0^k}(\vec m)$ and comparing with the known ciphertext tuple $E_s(\vec m)$. If this fails, assume $s\ne 0^k$.

The core hiding function is

$$
f_s:\{0,1\}^k\to 2^{\{0,1\}^{rn}},
\qquad
f_s(x)=\{E_x(\vec m),E_{s\oplus x}(\vec m)\}.
$$

This is a two-to-one Simon function with hidden period $s$:

$$
f_s(x)=f_s(x')\iff x'=x\oplus s
$$

for distinct $x,x'$. The proof uses the uniqueness of the key determined by the plaintext-ciphertext tuple: if $E_x(\vec m)=E_{x'}(\vec m)$ then $x=x'$, and if $E_x(\vec m)=E_{s\oplus x'}(\vec m)$ then $x=s\oplus x'$.

To evaluate $f_s$ coherently:

1. Prepare $x$ in superposition.
2. Compute $E_x(\vec m)$ using the public block-cipher circuit with key input $x$.
3. Query the related-key oracle on mask $x$ to obtain $E_{s\oplus x}(\vec m)$.
4. Encode the unordered pair as a canonical ordered pair
   $$
   (\min(E_x(\vec m),E_{s\oplus x}(\vec m)),\max(E_x(\vec m),E_{s\oplus x}(\vec m))).
   $$
   The paper suggests interpreting the $rn$-bit strings as unsigned integers, comparing them reversibly, conditionally swapping, and uncomputing the comparison garbage.
5. Run Simon’s algorithm: after querying $f_s$, apply $H^{\otimes k}$ to the input register and measure. Each sample gives a random $y\in\{0,1\}^k$ satisfying
   $$
   y\cdot s=0\pmod 2.
   $$
6. After $O(k)$ samples, solve the resulting linear system over $\mathbb F_2$ to recover $s$.

The circuit in the paper is just the Simon Fourier-sampling circuit with an implementation of the hiding function: block-cipher evaluation, related-key oracle calls, reversible comparison, controlled copy/swap into the canonical output registers, and full uncomputation.

## Key results

### Simon subroutine quoted in the paper

Let $g(k)$ be an upper bound for solving a $k\times k$ linear system over $\mathbb F_2$, and let $t_f(k)$ be the time needed to evaluate the Simon oracle $f$ on superpositions. Simon’s problem can be solved in expected time

$$
O(k\,t_f(k)+g(k)).
$$

So if $t_f(k)$ is polynomial, the hidden string is found in expected polynomial time.

### Theorem 2 of the paper

For every nonzero $s\in\{0,1\}^k$, the function

$$
f_s(x)=\{E_x(\vec m),E_{s\oplus x}(\vec m)\}
$$

satisfies Simon’s promise with hidden period $s$, and $t_{f_s}(k)$ can be chosen polynomial under the assumptions that the block cipher and related-key oracle are efficiently evaluable in superposition.

### Combined attack bound

The related-key attack runs in expected time

$$
O(k\,t_{f_s}(k)+g(k)),
$$

using $O(k)$ coherent evaluations of the related-key hiding function, hence a polynomial number of related-key encryption queries and polynomial classical post-processing.

## Comparison with prior work

| Work | Main idea | Relation to this paper |
|---|---|---|
| [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] | Recover a hidden XOR period from two-to-one oracle samples | Direct engine of the attack. |
| Brassard--Høyer (1997) | Exact polynomial-time Simon algorithm | Cited as a refined Simon implementation. |
| Mosca--Ekert (1998) | HSP/eigenvalue-estimation framing | Places the circuit in the abelian-HSP Fourier-sampling family. |
| Winternitz--Hellman (1987) | Classical chosen-key / related-key model with bit flips | Supplies the XOR-mask attack model. |
| Kelsey--Schneier--Wagner (1996) and later classical related-key work | Practical and formal related-key cryptanalysis | This paper shows that the basic XOR-mask version is far stronger with coherent quantum access. |
| [[Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017) — Paper Notes|Kaplan--Leurent--Leverrier--Naya-Plasencia (2017)]] | Q1/Q2 accounting for differential and linear attacks | Later systematises the same warning: coherent access changes the model, not only the running time. |
| [[Using Simon's Algorithm to Attack Symmetric-Key Cryptographic Primitives (Santoli-Schaffner 2017) — Paper Notes|Santoli--Schaffner (2017)]] | Simon attacks on 3-round Feistel and CBC-MAC | Same Q2 period-finding playbook applied to modes and Feistel structure rather than related keys. |

## Limits / caveats

- The attack needs coherent queries over related keys. That is a much stronger assumption than ordinary access to an encryption device or API.
- The plaintext-ciphertext tuple must identify the secret key uniquely. Ciphers with redundant key bits or pathological key schedules can violate this.
- The all-zero key case is handled separately; Simon’s promise is stated for nonzero period $s$.
- This is an asymptotic key-recovery result, not a concrete AES resource estimate. The paper does not count gates for any specific block cipher.
- The output encoding of $f_s$ uses reversible comparison and conditional swaps. This is polynomial-time, but the construction is not free in a fault-tolerant cost model.

## Reusable ideas

1. [[Simon Reduction for XOR Related-Key Oracles]] — turn XOR-related-key access into a two-to-one Simon oracle whose hidden period is the secret key.
2. [[Canonical Unordered Pair Encoding for Simon Functions]] — encode $\{a,b\}$ as $(\min(a,b),\max(a,b))$ so that the oracle value is invariant under the hidden shift.
3. [[Known-Plaintext Unicity as a Simon Promise]] — use a small tuple of plaintext-ciphertext pairs to enforce injectivity modulo the intended hidden period.

## References within this paper

- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — the hidden-period algorithm used for key recovery.
- Brassard--Høyer (1997) — exact polynomial-time Simon algorithm.
- Mosca--Ekert (1998) — hidden subgroup/eigenvalue-estimation framing for Fourier sampling.
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994/1997)]] — cited as the canonical quantum threat to asymmetric cryptography.
- Bernstein (2009), *Introduction to post-quantum cryptography* — motivates the conventional view that symmetric cryptography mainly pays a Grover square-root penalty.
- Winternitz--Hellman (1987) — original chosen-key / related-key bit-flip model.
- Kelsey--Schneier--Wagner (1996), Bellare--Kohno (2003), Albrecht--Farshim--Paterson--Watson (2011) — classical and formal related-key context.
- Cuccaro--Draper--Kutin--Moulton (2004), Draper--Kutin--Rains--Svore (2006), Takahashi--Tani--Kunihiro (2010), Bennett (1973) — reversible arithmetic, comparison, and uncomputation tools used to implement the canonical pair encoding.

## Cross-links

### Paper notes

- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]]
- [[Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017) — Paper Notes]]
- [[Using Simon's Algorithm to Attack Symmetric-Key Cryptographic Primitives (Santoli-Schaffner 2017) — Paper Notes]]
- [[Quantum Attacks Against Iterated Block Ciphers (Kaplan 2015) — Paper Notes]]
- [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes]]

### Trick cards

- [[Simon Reduction for XOR Related-Key Oracles]]
- [[Canonical Unordered Pair Encoding for Simon Functions]]
- [[Known-Plaintext Unicity as a Simon Promise]]
- [[Coset Sampling via Fourier Transform]]
- [[Q1-Q2 Adversary Split for Quantum Cryptanalysis]]
