# Known-Plaintext Unicity as a Simon Promise

> **Source:** Rötteler--Steinwandt, arXiv:1306.2301
> **Tags:** #trick #Simon #quantum-cryptanalysis #block-ciphers

## What it does

Uses enough plaintext-ciphertext pairs to turn a cryptanalytic hidden-shift construction into a valid Simon instance by killing accidental key collisions.

## The trick

Simon’s algorithm needs the promise that the oracle is exactly two-to-one:

$$
f(x)=f(x')\iff x'=x\oplus s.
$$

In block-cipher reductions, this can fail if different keys agree on the data being tested. Rötteler--Steinwandt fix the promise by choosing a tuple of known plaintexts

$$
\vec m=(m_1,\ldots,m_r)
$$

such that the map

$$
K\mapsto E_K(\vec m)=(E_K(m_1),\ldots,E_K(m_r))
$$

is injective on the relevant key space. Then a collision of ciphertext tuples forces equality of keys:

$$
E_x(\vec m)=E_{x'}(\vec m)\implies x=x'.
$$

For the related-key Simon oracle

$$
f_s(x)=\{E_x(\vec m),E_{s\oplus x}(\vec m)\},
$$

this injectivity proves that the only collisions are $x$ with $x\oplus s$.

The paper appeals to the known-plaintext unicity-distance heuristic: for an $n$-bit block cipher with $k$-bit keys, roughly

$$
r>\lceil k/n\rceil
$$

plaintext-ciphertext pairs should identify the key for typical ciphers.

## When to reach for it

- A period-finding or hidden-shift reduction has unintended collisions from non-injective keyed evaluations.
- A small tuple of examples can make the key identifiable.
- You need a promise proof, not only an implementation of the quantum oracle.

## Complexity

The oracle output length grows from $n$ to $rn$ bits. If $r=O(\operatorname{poly}(k))$ and the cipher is efficient, evaluating the tuple remains polynomial. In standard block-cipher parameter ranges, the heuristic $r>\lceil k/n\rceil$ is a constant.

## Caveat

This is a cryptographic assumption about the cipher family, not a theorem for every block cipher. Redundant key bits, equivalent keys, or deliberately bad key schedules break it.

## Related notes

- [[A Note on Quantum Related-Key Attacks (Roetteler-Steinwandt 2013) — Paper Notes]]
- [[Simon Reduction for XOR Related-Key Oracles]]
- [[Canonical Unordered Pair Encoding for Simon Functions]]
- [[Q1-Q2 Adversary Split for Quantum Cryptanalysis]]
