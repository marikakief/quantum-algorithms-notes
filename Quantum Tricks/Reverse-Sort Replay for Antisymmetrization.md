
> **Tags:** #trick #antisymmetrization #fermions #sorting
> **Source:** npj Quantum Information 2018 (s41534-018-0071-5)

## What it does
Generates the antisymmetric fermionic superposition by replaying a recorded sorting transcript in reverse on a sorted target register.

## The trick
1. Sort a seed register with a reversible [[Sorting Networks as Quantum Control-Flow Compilers|sorting network]] and store all [[Reversible Comparator with Swap Record|comparator]] outcomes in `record`.
2. Keep only collision-free seeds.
3. Apply the same [[Reversible Comparator with Swap Record|comparator]] sequence **in reverse order** to the sorted target, controlled by `record`.
4. Add phase $(-1)$ per swap (or equivalent parity tracking).

Result: uniform superposition over all permutations with parity sign:
$$\sum_{\sigma}(-1)^{\pi(\sigma)}|\sigma(r_1,\dots,r_\eta)\rangle$$

## Why it works
Sorting transcript encodes a permutation factorization into adjacent/non-adjacent swaps. Reversing it applies precisely that permutation family to the target. Swap count parity gives the fermionic sign.

## When to use
- Fermionic [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes|first-quantization]] state preparation
- Any problem requiring coherent permutation superpositions with sign structure
- Undo/replay paradigms where a reversible preprocessing step generates a control script

## Caveat
Requires sorted, repetition-free input for unitarity and correctness of antisymmetrization map.

## Related Paper Notes

- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes]]
