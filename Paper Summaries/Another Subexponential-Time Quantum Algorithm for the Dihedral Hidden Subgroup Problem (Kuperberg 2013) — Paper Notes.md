# Another Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2013) — Paper Notes

> **Source:** Greg Kuperberg, *Another subexponential-time quantum algorithm for the dihedral hidden subgroup problem*, arXiv:1112.3333
> **Links:** [arXiv](https://arxiv.org/abs/1112.3333)
> **Tags:** #hidden-subgroup-problem #dihedral-hsp #hidden-shift #sieve

---

## Metadata

- **Author:** Greg Kuperberg
- **Year:** 2011 preprint; published version 2013
- **arXiv:** [1112.3333](https://arxiv.org/abs/1112.3333)
- **Zoo entry:** Quantum Algorithm Zoo #218
- **Topic:** Dihedral HSP; cyclic hidden shift; collimation sieve; quantum-space reduction.
- **Status in vault:** Added from Algorithm Zoo HSP batch.

## Computational problem

Solve the dihedral hidden subgroup problem for $D_N$, equivalently the cyclic hidden shift problem: given injective functions $f,g:\mathbb Z_N\to X$ with

$$g(x)=f(x+s),$$

recover the hidden shift $s$.

The algorithm focuses on states of the form

$$|\psi_b\rangle \propto |0\rangle + e^{2\pi i b s/N}|1\rangle,$$

where $b$ is known and random. The task is to combine many such states until a final measurement reveals bits of $s$.

## What the paper does

This paper replaces Kuperberg's original sieve with a **collimation sieve**. It keeps the subexponential time scale

$$\exp(O(\sqrt{\log N})),$$

but cuts quantum memory to $O(\log N)$ by traversing the sieve tree depth-first. The cost is classical memory: the algorithm keeps classical descriptions of phase vectors, and the main version uses $\exp(O(\sqrt{\log N}))$ classical space.

It also makes the relation to Regev's algorithm explicit. One end of the parameter range recovers Regev-style polynomial space with higher time; the other uses more classical memory, or even quantumly addressable classical memory, to reduce quantum time.

## Algorithmic structure

1. **Convert HSP to hidden-shift phase vectors.**
   - Query the hidden-shift oracle, discard the function value, and measure the Fourier mode.
   - This produces a known mode $b$ and a phase vector whose phases are linear in $bs$.

2. **Use phase vectors instead of only qubits.**
   - A phase vector is a state over $\ell$ labels:
     $$|\psi\rangle={1\over\sqrt{\ell}}\sum_{j=0}^{\ell-1} e^{2\pi i b_j s/N}|j\rangle.$$
   - The classical list $(b_j)$ is tracked outside the quantum register.

3. **Collimate.**
   - Combine two phase vectors by taking tensor products.
   - Partially measure the coefficient sums modulo a chosen power of 2.
   - Keep the branch where the new coefficients agree on more low-order bits. This narrows the phase spread.
   - In symbols, tensor
     $$
     \sum_i e^{2\pi i b_i s/N}|i\rangle
     \otimes
     \sum_j e^{2\pi i c_j s/N}|j\rangle
     =
     \sum_{i,j} e^{2\pi i (b_i+c_j)s/N}|i,j\rangle,
     $$
     then measure $b_i+c_j$ modulo a chosen power of two and retain the induced narrower congruence class of coefficients.

4. **Traverse the sieve depth-first.**
   - The original sieve keeps many quantum states simultaneously.
   - The new algorithm recursively creates the needed children, combines them, and discards them, keeping only logarithmic quantum space.

5. **Read bits of the hidden shift.**
   - Once the sieve produces a qubit whose phase is $(-1)^s$, measuring in the Hadamard basis reveals the parity of $s$.
   - Recurse on smaller moduli to recover all bits.

## Key results

- The algorithm uses $O(\log N)$ quantum space.
- Time remains subexponential: $\exp(O(\sqrt{\log N}))$ up to model-dependent constants.
- Classical space is $\exp(O(\sqrt{\log N}))$ in the main tradeoff described in the abstract.
- The framework exposes a family of trade-offs among classical space, quantum time, and QRACM-style memory access. With fully classical access one pays for classical bookkeeping; with quantumly addressable classical space, some tradeoffs reduce quantum time without increasing quantum storage.
- Multiple hidden shifts can be used, though the speed-up from that feature is limited.

| Algorithm | Quantum space | Classical space | Time/query scale | QRACM role |
|---|---:|---:|---:|---|
| [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg 2005]] | $\exp(O(\sqrt{\log N}))$ | small bookkeeping | $\exp(O(\sqrt{\log N}))$ | not the main model |
| [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev 2004]] | $\operatorname{poly}(\log N)$ | polynomial storage; subexponential classical enumeration time | $2^{O(\sqrt{\log N\log\log N})}$ | not needed |
| Kuperberg 2013 collimation | $O(\log N)$ | $\exp(O(\sqrt{\log N}))$ in the main model | $\exp(O(\sqrt{\log N}))$ | optional tradeoff lever |

## Assessment

This is the more useful Kuperberg paper for separating quantum memory from classical bookkeeping. The first sieve proves subexponential time; this one explains how to keep quantum memory small and makes explicit which part of the cost is classical bookkeeping. For fault-tolerant-era thinking, that separation matters: quantum memory and quantumly coherent storage are the expensive resources.

The catch is that the algorithm is still subexponential, not polynomial, and it relies on fairly elaborate phase-vector bookkeeping. It narrows the dihedral-HSP barrier rather than removing it.

## Reusable ideas

- **Phase-vector bookkeeping:** store large coefficient lists classically while keeping the quantum register small.
- **Collimation:** combine states and measure coarse congruence classes to force phase coefficients into a narrower residue class.
- **Depth-first quantum sieve:** reduce quantum memory by generating sieve branches recursively.
- **Resource split:** count classical space, quantum space, and quantum access to classical memory separately.

## Cross-links

- [[Hidden Subgroup Problem]]
- [[Collimation Sieve for Dihedral Hidden Shift]]
- [[Depth-First Quantum Sieve to Save Quantum Space]]
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg (2005)]]
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev (2004)]]
- [[On Quantum Algorithms for Noncommutative Hidden Subgroups (Ettinger-Hoyer 1998) — Paper Notes|Ettinger--Høyer (1998)]]

## References

- Greg Kuperberg, *Another subexponential-time quantum algorithm for the dihedral hidden subgroup problem*, arXiv:1112.3333.
