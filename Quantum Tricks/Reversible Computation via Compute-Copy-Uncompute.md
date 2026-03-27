
> **Source:** Bennett (1973); formalized for quantum use in Kitaev, arXiv:quant-ph/9511026 (1995), Lemmas 1–2
> **Tags:** #trick #reversible #garbage #fundamental

## What it does

Computes a function $F$ reversibly (without garbage) by computing, copying the result, then uncomputing. This is required for any classical operation used inside a quantum algorithm — garbage destroys quantum coherence.

## The trick

### Lemma 1: Compute and copy

If $F: N \to B^m$ is computable by a circuit of size $L$, define $F^\tau: (u, v) \mapsto (u, v \oplus F(u))$. This bijection can be computed reversibly in $2L + m$ operations:

$$
(x, 0, 0) \xrightarrow{G} (x, F(x), \text{garbage}) \xrightarrow{\tau_m} (x, F(x), \text{garbage} \oplus \text{output}) \xrightarrow{G^{-1}} (x, F(x), 0)
$$

Wait — that's not quite right. The correct sequence is: compute $G$ (which writes $F(x)$ plus garbage), copy $F(x)$ to a clean register, then run $G^{-1}$ to erase the garbage.

### Lemma 2: Full bijection, no garbage

If both $G: N \to M$ and $G^{-1}$ are computable by circuits of size $L$ and $L'$, then $G$ can be computed reversibly (input register ends with $G(x)$, all ancillae return to $|0\rangle$) with $2L + 2L' + 4n$ operations:

$$
(x, 0) \xrightarrow{G^\tau} (x, G(x)) \xrightarrow{\tau_n} (0, G(x)) \xrightarrow{(G^{-1})^\tau} (G(x), G(x)) \xrightarrow{\tau_n} (G(x), 0)
$$

## Why garbage kills quantum computation

If a classical operation $G$ produces garbage $g(x)$ that varies with input:

$$
U|x, 0\rangle = |G(x), g(x)\rangle
$$

Then tracing out the garbage register gives a **diagonal** density matrix — all interference between different inputs is destroyed. The state becomes classical. This is why every classical subroutine inside a quantum algorithm must be garbage-free.

## When to reach for it

- Any classical function used inside a quantum algorithm (arithmetic, comparisons, table lookups)
- [[Gapped Phase Estimation|Phase estimation]]: the conditional $U^j$ applications involve classical comparisons that must be garbage-free
- [[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE oracles]]: state preparation circuits that compute amplitudes need garbage-free arithmetic
- [[Sorting Networks as Quantum Control-Flow Compilers|Sorting networks]]: the [[Reversible Comparator with Swap Record|comparator]] records must be properly uncomputed

## Complexity

$2L + m$ operations for $F^\tau$ (Lemma 1); $2L + 2L' + 4n$ for a full bijection (Lemma 2). Roughly doubles the circuit size.

## Caveat

Requires running the computation forward AND backward. If $G^{-1}$ is much harder to compute than $G$ (e.g., one-way functions), the cost can be asymmetric.

## Related notes

- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
- [[Sorting Networks as Quantum Control-Flow Compilers]]
- [[Reversible Comparator with Swap Record]]
- [[Hamiltonian-to-Projection via Phase Estimation]]
