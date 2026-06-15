# Block-Cipher Key Search as a Reversible Grover Predicate

> **Source:** Grassl, Langenberg, Roetteler, Steinwandt, arXiv:1512.04965
> **Tags:** #trick #Grover #quantum-cryptanalysis #block-ciphers #reversible-oracles

## What it does

Turns known-plaintext block-cipher key recovery into a clean Boolean predicate for [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]].

## The trick

Given plaintext-ciphertext pairs
$$(m_i,c_i)_{i=1}^r, \qquad c_i = E_K(m_i),$$
define
$$
f(K') = \bigwedge_{i=1}^r \left(E_{K'}(m_i)=c_i\right).
$$
Then implement the reversible oracle
$$
U_f|K'\rangle|y\rangle = |K'\rangle|y\oplus f(K')\rangle.
$$

The useful circuit layout is:

```text
candidate key K'
      │
      ├── reversible E_{K'}(m_1) ── compare with c_1 ─┐
      ├── reversible E_{K'}(m_2) ── compare with c_2 ─┼── multi-control flip
      │                         ...                   │
      └── reversible E_{K'}(m_r) ── compare with c_r ─┘

then uncompute all E_{K'}(m_i) boxes
```

The encryption boxes can run in parallel for depth, but their gates are counted $2r$ times because the oracle must compute and uncompute them. The final comparison is a multi-controlled NOT over all output bits, with controls adjusted to the known ciphertext values.

For a $k$-bit key and $n$-bit block, the paper uses the heuristic collision estimate
$$
r > \left\lceil \frac{2k}{n}\right\rceil
$$
to make the marked key unique. For AES, $n=128$, so this gives $r=3,4,5$ for AES-128/192/256.

## When to reach for it

- Estimating resource costs for Grover attacks on block ciphers or MACs.
- Comparing symmetric-key security levels under quantum exhaustive search.
- Building a reversible oracle where the predicate is equality against known classical outputs.
- Separating the cost of the primitive implementation from the $\Theta(2^{k/2})$ Grover iteration count.

## Complexity

If one reversible encryption box costs $(T_E,C_E,D_E,s_E)$ in T gates, Clifford gates, depth, and qubits, then the predicate oracle with $r$ known pairs costs roughly:

$$
T(U_f) \approx 2rT_E + T_{\mathrm{cmp}}(rn),
$$
$$
D(U_f) \approx 2D_E + D_{\mathrm{cmp}}(rn)
$$
if the $r$ encryption boxes run in parallel, and
$$
s(U_f) \approx r s_E + O(1).
$$

The full Grover attack then multiplies the oracle cost by
$$
\left\lfloor \frac{\pi}{4}2^{k/2}\right\rfloor
$$
for the unique-key case.

## Caveat

The uniqueness guarantee is usually heuristic for real block ciphers. If multiple keys match the available pairs, the iteration count must use the marked-item count $M$, or one must switch to unknown-$M$ search such as [[Randomised Iteration Count for Grover Search]] or counting via [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]].

## Related notes

- [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes]]
- [[Standard Amplitude Amplification]]
- [[Randomised Iteration Count for Grover Search]]
- [[Depth-Optimised Grover Oracle via Parallel Fan-Out]]
