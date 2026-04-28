# In-Place Majority for Carry Propagation

> **Source:** Cuccaro, Draper, Kutin, Moulton, arXiv:quant-ph/0410184
> **Tags:** #trick #quantum-arithmetic #ripple-carry #Toffoli #reversible-computation

## What it does

Computes the majority (carry) of three bits using only the three input registers — no extra ancilla qubit needed per carry.

## The trick

Given three bits $c_i$, $b_i$, $a_i$ (current carry, second addend bit, first addend bit), the MAJ gate computes $c_{i+1} = \text{MAJ}(a_i, b_i, c_i) = a_i b_i \oplus a_i c_i \oplus b_i c_i$ and stores it in register $A_i$, while the other two registers hold $c_i \oplus a_i$ and $b_i \oplus a_i$.

The circuit is two CNOTs followed by one Toffoli:

```
c_i ──•──────•── c_i ⊕ a_i
b_i ──⊕──•──── b_i ⊕ a_i
a_i ─────⊕──T── MAJ(a_i, b_i, c_i)
```

The first CNOT XORs $a_i$ into $c_i$. The second XORs $a_i$ into $b_i$. The Toffoli then computes $(c_i \oplus a_i)(b_i \oplus a_i) \oplus a_i$. Expanding: $c_i b_i \oplus c_i a_i \oplus a_i b_i \oplus a_i^2 \oplus a_i = c_i b_i \oplus c_i a_i \oplus a_i b_i$ (since $a_i^2 \oplus a_i = 0$ for bits). This is exactly the majority function.

The carry is now stored in $A_i$, overwriting $a_i$. The value $a_i$ is recoverable from the companion UMA gate (see [[MAJ-UMA Decomposition for Reversible Adders]]).

## When to reach for it

- Building ripple-carry adders with minimal ancilla overhead. Classical reversible adders (VBE) use a fresh ancilla for each carry bit; this gate reuses the input register instead.
- Any reversible circuit that needs to propagate a Boolean majority through a chain of bit triples.
- As the compute half of a compute/uncompute pair in quantum arithmetic, where the uncompute half is the UMA gate.

## Complexity

2 CNOTs + 1 Toffoli per carry bit. The Toffoli costs 4 T gates in a fault-tolerant setting (using the matched-phase-error trick for paired Toffolis, or via [[Temporary Logical-AND]]).

## Caveat

The gate overwrites $a_i$ with the carry. If $a_i$ is needed later, it must be restored — which is what the UMA gate does. The MAJ gate is not useful in isolation; it is always paired with UMA in a working adder.

## Related notes
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes]] — source paper
- [[MAJ-UMA Decomposition for Reversible Adders]] — the paired gate structure
- [[Gate Commutation for Ripple-Carry Depth Reduction]] — depth optimisation of MAJ chains
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — replaces paired MAJ/UMA Toffolis with [[Temporary Logical-AND]] for lower T-count
- [[Ancilla Opportunity Cost Analysis]] — tradeoff analysis for 1-ancilla vs. multi-ancilla adders
