
> **Tags:** #trick #fermions #jordan-wigner #swap-network
> **Source:** arXiv:2510.08644 (Liu, Zhu, Lin, Low, Yang), Section IV.3

## What it does

Implements fermionic parity strings (the $Z^{\otimes p}$ chains in Jordan-Wigner encoding) efficiently by routing the target orbital to a canonical register position using SWAP gates, applying a fixed-size parity circuit there, then routing back. This replaces variable-length parity strings with fixed-cost operations.

## The problem

In Jordan-Wigner encoding, the creation operator $a_p^\dagger$ on orbital $p$ of $n$ orbitals involves a parity string:
$$a_p^\dagger = Z_0 \otimes Z_1 \otimes \cdots \otimes Z_{p-1} \otimes X_p^+$$

For large $p$, this string requires $O(p)$ two-qubit gates. In the [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]], different terms involve orbitals at different positions, so naive implementation incurs $O(n)$ gates per term and $O(nL)$ total — costly.

## The fix

The SWAP-UP approach (for the sparsity oracle $O_C$):
1. **Route orbital $p$ to register 0** using a sequence of controlled-SWAPs: move the qubit at position $p$ to the front, taking $O(p)$ SWAP gates but each SWAP is $O(1)$ T gates.
2. **Apply a fixed parity-increment gate** at register 0 — only touches the top register.
3. **Route back** (reverse SWAPs).

The key: SWAP gates have lower T-count than individual parity phase gates when accounting for the full [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] circuit structure. Moreover, the routing allows batching of multiple orbital operations with shared SWAPs.

## When to reach for it

- Jordan-Wigner encoded second-quantized Hamiltonians where parity string length varies across terms
- When T-count is the primary concern and Clifford depth is acceptable
- Constructions where multiple creation/annihilation operators are applied in sequence (pairwise interactions), enabling SWAP network reuse

## Complexity

Clifford gate overhead: $O(n)$ SWAPs for worst-case orbital at position $n-1$. T-gate savings come from replacing $O(n)$ conditional phase operations with $O(n)$ SWAPs plus $O(1)$ T-gates per parity operation. Improvement in constant factors is the main benefit, not asymptotic — the paper reports improved constant factor in Clifford gate count vs prior constructions.

## Caveat

- On hardware with limited qubit connectivity (2D grid, linear chain), the SWAP routing itself requires additional SWAPs for routing, potentially restoring the overhead.
- The T-count improvement vs naive parity-string is primarily a constant-factor gain, not asymptotic. The real savings come when this is combined with the SELECT-SWAP amplitude oracle to get the $\tilde{O}(\sqrt{L})$ T-count overall.
- "SWAP-UP" is a descriptor for the routing strategy; the paper calls this architecture simply the "SWAP architecture" for $O_C$.

## Related Paper Notes
- [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes]]
- [[SELECT-SWAP Grouped Data Lookup for Block-Encoding]]
