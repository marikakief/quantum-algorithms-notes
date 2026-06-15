# Quantum Meet-in-the-Middle as Claw Finding

> **Source:** Kaplan, arXiv:1410.1434
> **Tags:** #trick #quantum-cryptanalysis #claw-finding #element-distinctness #block-ciphers

## What it does

Turns double-composition key recovery into claw finding between a forward middle-value map and a backward middle-value map.

## The trick

Suppose a keyed permutation family $F=\{F_1,\ldots,F_N\}$ is used twice and the attacker knows $P,C$ with

$$
F_{k_2}(F_{k_1}(P))=C.
$$

Define

$$
G_1(x)=F_x(P),\qquad G_2(y)=F_y^{-1}(C).
$$

Then

$$
G_1(k_1)=G_2(k_2)
$$

if and only if $(k_1,k_2)$ is the encryption key pair. Key recovery is therefore a claw-finding instance. Use [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis element distinctness]] / claw finding rather than Grover search over all $N^2$ key pairs.

The same pattern applies whenever a target equation splits around an intermediate value:

$$
L(a)=R(b),
$$

where $L$ can be computed forward from the left secret and $R$ backward from the right secret.

## When to reach for it

- Double encryption or any two-layer composition with invertible right layer.
- Cryptanalytic attacks where a middle state can be computed from either side.
- Search problems where the classical meet-in-the-middle table would store two lists and look for a collision.

## Complexity

For two lists of size $N$ with a unique claw, the quantum query/time complexity is

$$
\Theta(N^{2/3})
$$

using $O(N^{2/3})$ memory in the time-efficient quantum-walk implementation.

For double encryption this improves over Grover search on $(k_1,k_2)$, which costs $O(N)$ time, but uses much more memory.

## Caveat

This is not automatically the best time-space tradeoff. In Kaplan's double-encryption setting, the $O(N^{2/3})$-time attack has time-space product $O(N^{4/3})$, while a slower Grover search over key pairs uses only logarithmic memory.

The reduction also needs coherent access to the relevant forward and inverse permutation oracles.

## Related notes

- [[Quantum Attacks Against Iterated Block Ciphers (Kaplan 2015) — Paper Notes]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
- [[Projected Adversary Matrix Lower Bound for Structured Oracles]]
- [[Middle-Value Quantum Walk for Iterated-Cipher Dissection]]
