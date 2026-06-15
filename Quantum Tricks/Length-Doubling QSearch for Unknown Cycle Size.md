# Length-Doubling QSearch for Unknown Cycle Size

> **Source:** Cade--Montanaro--Belovs, arXiv:1610.00581; Boyer--Brassard--Høyer--Tapp quantum search
> **Tags:** #trick #Grover #QSearch #graph-algorithms #unknown-solution-count

## What it does

Combines a local test whose cost depends on an unknown witness size with QSearch over possible witness locations.

## The trick

Suppose a predicate detects whether vertex $k$ lies on a cycle of length at most $d$, with cost
$$
\widetilde O(n\sqrt d).
$$
Search over $k\in V$ with QSearch, but run separate rounds for length guesses
$$
d=2^i.
$$

If the shortest cycle has length $\ell$, then at the first scale with $d\ge\ell$, there are $\ell$ good vertices. QSearch needs
$$
O(\sqrt{n/\ell})
$$
iterations, so the scale costs
$$
\widetilde O(\sqrt{n/\ell}\cdot n\sqrt \ell)=\widetilde O(n^{3/2}).
$$
The length-doubling overhead is only polylogarithmic.

## When to use it

- Local-witness tests where the predicate cost scales with witness size.
- Graph algorithms where witness length is unknown.
- Wrapping bounded-error predicates into Grover search; use error reduction enough for superposition calls.

## Related notes

- [[Time and Space Efficient Quantum Algorithms for Detecting Cycles and Testing Bipartiteness (Cade-Montanaro-Belovs 2016) — Paper Notes]]
- [[Standard Amplitude Amplification]]
- [[Coarse Offset Search plus Hidden Shift Refinement]]
