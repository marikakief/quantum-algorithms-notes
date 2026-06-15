# Simon Reduction for XOR Related-Key Oracles

> **Source:** Rötteler--Steinwandt, arXiv:1306.2301
> **Tags:** #trick #quantum-cryptanalysis #Simon #related-key #symmetric-key

## What it does

Turns coherent XOR-related-key access to a block cipher into a Simon oracle whose hidden period is the secret key.

## The trick

Suppose a block cipher $E_K$ has secret key $s\in\{0,1\}^k$, and the attacker can query the related-key oracle

$$
(L,m)\mapsto E_{s\oplus L}(m)
$$

coherently in the mask $L$. Choose a tuple of plaintexts $\vec m=(m_1,\ldots,m_r)$ such that the ciphertext tuple $E_s(\vec m)$ uniquely determines $s$.

Define

$$
f_s(x)=\{E_x(\vec m),E_{s\oplus x}(\vec m)\}.
$$

Then

$$
f_s(x)=f_s(x\oplus s),
$$

and uniqueness of the plaintext-ciphertext tuple rules out any other collision. So $f_s$ satisfies Simon’s promise with hidden XOR period $s$. Running [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon’s algorithm]] recovers $s$ from $O(k)$ samples $y$ with

$$
y\cdot s=0\pmod 2.
$$

This is best viewed as an oracle-model transformation: related-key structure supplies the hidden shift; the cipher’s ordinary keyed evaluation supplies the second branch.

## When to reach for it

- A cryptanalytic oracle gives coherent access to $K\oplus x$ for an unknown fixed key $K$.
- You can build a function invariant under $x\mapsto x\oplus K$.
- You can prove that all collisions come only from that XOR shift.
- You want to distinguish a genuine Q2 attack from a Groverised classical attack; see [[Q1-Q2 Adversary Split for Quantum Cryptanalysis]].

## Complexity

If one evaluation of the hiding function costs $t_f(k)$ and solving a $k\times k$ linear system over $\mathbb F_2$ costs $g(k)$, Simon sampling gives expected time

$$
O(k\,t_f(k)+g(k)).
$$

For an efficiently reversible block cipher and polynomially many plaintext blocks, $t_f(k)=\operatorname{poly}(k)$.

## Caveat

The coherent related-key oracle is the whole issue. Without superposition access over key masks, this reduction is not an attack on a normal encryption API. The plaintext tuple also has to enforce injectivity modulo the intended period.

## Related notes

- [[A Note on Quantum Related-Key Attacks (Roetteler-Steinwandt 2013) — Paper Notes]]
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]]
- [[Canonical Unordered Pair Encoding for Simon Functions]]
- [[Known-Plaintext Unicity as a Simon Promise]]
- [[Q1-Q2 Adversary Split for Quantum Cryptanalysis]]
