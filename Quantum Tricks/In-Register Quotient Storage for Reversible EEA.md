# In-Register Quotient Storage for Reversible EEA

> **Source:** Khattar, Shutty, Gidney, Zalcman, Yosri, Maslov, Babbush, Jordan, arXiv:2510.10967
> **Tags:** #trick #reversible-computing #Euclidean-algorithm #finite-fields #arithmetic

## What it does

Makes the low-space synchronized reversible Extended Euclidean Algorithm deterministic by storing each quotient inside padding already present in the shared Euclidean registers.

## The trick

Synchronized reversible EEA implementations share registers between remainders and Bezout coefficients. Earlier low-space variants used a small auxiliary quotient register and tolerated small failure probability when a quotient was too large.

This paper observes that the shared representation has structured unused space. During an EEA step, invariants such as

$$
\deg(u_i)+\deg(r_i)\le n
$$

leave enough padding to hold the current quotient $q_{i+1}=\lfloor r_{i-1}/r_i\rfloor$ in-register. The quotient is computed term-by-term into that padding, consumed by the Bezout update, and removed before the next shared-register swap.

The circuit also merges arithmetic cycles: division and Bezout-update work that previously happened in separate blocks can share a single arithmetic loop because they touch different regions of the shared registers.

## When to reach for it

- Reversible EEA for polynomial GCD or Reed-Solomon decoding.
- Fault-tolerant arithmetic where qubit count and Toffoli count both matter.
- Any setting where quotient data is temporary and can be parked in degree-padding inside a shared polynomial representation.

## Complexity

For degree-$n$ polynomial EEA over $\mathbb{F}_q$, the explicit-access construction uses $2nb+O(\log^2 n)$ qubits and $6n^2$ dominant quantum-quantum field multiplications. The paper proves a worst-case cycle bound of $6n-1$ for full PEEA and $6\lfloor n/2\rfloor+5$ for the half-PEEA used in Reed-Solomon decoding.

## Caveat

The bookkeeping is tight. The trick depends on degree invariants and synchronized register layout; it is not a drop-in replacement for arbitrary reversible polynomial arithmetic.

## Related notes

- [[Verifiable Quantum Advantage via Optimized DQI Circuits (Khattar, Shutty, Gidney et al 2025) — Paper Notes]]
- [[Dialog Representation for Implicit Bezout Access]]
- [[Uncomputation via Syndrome Decoding]]
