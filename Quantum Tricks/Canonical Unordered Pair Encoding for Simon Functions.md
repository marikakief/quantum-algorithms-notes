# Canonical Unordered Pair Encoding for Simon Functions

> **Source:** Rötteler--Steinwandt, arXiv:1306.2301
> **Tags:** #trick #Simon #oracle-construction #reversible-computation

## What it does

Makes a two-branch oracle value invariant under swapping the branches, while keeping a fixed bit-string output for Simon sampling.

## The trick

Many hidden-shift reductions naturally produce an unordered pair

$$
\{a(x),b(x)\}
$$

rather than an ordered tuple. For Simon’s algorithm, the oracle must output a definite computational-basis string. Choose a total order on the range, usually lexicographic or integer order, and encode

$$
\{a,b\}\longmapsto (\min(a,b),\max(a,b)).
$$

In the related-key setting of [[A Note on Quantum Related-Key Attacks (Roetteler-Steinwandt 2013) — Paper Notes]], this turns

$$
\{E_x(\vec m),E_{s\oplus x}(\vec m)\}
$$

into a $2rn$-bit string. The value is identical for $x$ and $x\oplus s$, but does not depend on which branch was computed first.

A reversible implementation is straightforward:

1. Compute $a$ and $b$ into work registers.
2. Compare $a$ and $b$ reversibly, e.g. by computing a subtraction sign bit.
3. Conditionally copy or swap registers so the output is $(\min(a,b),\max(a,b))$.
4. Uncompute the comparison garbage.

The final uncomputation matters: leftover comparison garbage can carry which-branch information and spoil the interference in Simon sampling.

## When to reach for it

- A hidden-period oracle is symmetric under exchanging two computed values.
- The mathematical oracle is set-valued, but the quantum implementation needs a basis-state encoding.
- You need to hide which branch corresponds to $x$ and which corresponds to $x\oplus s$.

## Complexity

For $N$-bit values, reversible comparison and conditional swap are polynomial-size circuits. With standard ripple-carry arithmetic this is $O(N)$ to $O(N\log N)$ elementary reversible gates depending on the adder/comparator family; the paper only needs polynomial size.

## Caveat

The encoding preserves the Simon promise only if $a\ne b$ and all unintended collisions are ruled out separately. The canonical ordering removes branch labels; it does not prove injectivity by itself.

## Related notes

- [[A Note on Quantum Related-Key Attacks (Roetteler-Steinwandt 2013) — Paper Notes]]
- [[Simon Reduction for XOR Related-Key Oracles]]
- [[Known-Plaintext Unicity as a Simon Promise]]
- [[Coset Sampling via Fourier Transform]]
