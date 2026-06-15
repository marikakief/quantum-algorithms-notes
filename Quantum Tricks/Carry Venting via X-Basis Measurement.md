# Carry Venting via X-Basis Measurement

> **Source:** Gidney, arXiv:2507.23079
> **Tags:** #trick #quantum-arithmetic #measurement #uncomputation #circuit-primitive #fault-tolerant

## What it does

Eliminates carry garbage from a quantum adder by measuring carry qubits in the X basis, converting unwanted qubit state into classical phasing tasks that can be resolved later.

## The trick

During a ripple-carry addition $x \to x + d + c_0$, each carry bit $c_{k+1} = \text{maj}(x_k, d_k, c_k)$ is computed, used, and then becomes garbage. Normally you'd uncompute it (running the carry logic in reverse), which costs additional gates and requires workspace to hold intermediate values.

The key identity, under the paper's modulo/carry-in convention for the internal carry chain: the carries of $x + d + c_0$ are exactly reproduced by adding the same offset $d$ into the bitwise complement of the result:

$$\text{carry}(\sim x', d, c_0) = \text{carry}(x, d, c_0)$$

This makes each carry qubit **Z-redundant** — its value is a deterministic function of qubits that will still exist after the addition completes. A Z-redundant qubit can be erased by measuring it in the X basis ([[Measurement-Asymmetric Uncomputation]]):

1. Apply $H$ to the carry qubit, then measure.
2. Outcome $|+\rangle$: carry erased cleanly, no correction needed.
3. Outcome $|-\rangle$: carry erased, but an unwanted phase flip $Z_{c_k}$ was introduced.

The phase flip is a "phasing task" — a classical record that says "a $Z$ correction conditioned on $\text{carry}(\sim x', d, c_0)_k$ must eventually be applied." This task is resolved later using a carry-xor circuit from [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes|Häner et al. (2017)]].

The result: carry qubits are recycled immediately (only 2 clean ancillae needed for the entire ripple chain), at the cost of deferred phase corrections.

## When to reach for it

- Building classical-quantum adders where the offset register doesn't exist as qubits (so [[MAJ-UMA Decomposition for Reversible Adders|Cuccaro's trick]] of storing carries in the offset register is impossible).
- Any addition circuit where ancilla qubits are scarce but mid-circuit measurement is cheap.
- More generally, any reversible computation that produces Z-redundant garbage — qubits whose values are deterministic functions of other qubits that survive the computation.

## Complexity

- **Per carry vented:** 1 X-basis measurement + 1 classical bit of bookkeeping.
- **Resolving all internal-carry phasing tasks:** Requires $n \pm O(1)$ additional Toffolis via the carry-xor construction, plus $n - 2$ dirty qubits as a scratch register. Boundary carries depend on the exact adder variant.
- **Net effect on adder:** Reduces clean ancillae from $O(n)$ to $O(1)$ while keeping Toffoli count at $O(n)$.

## Caveat

Venting converts qubit garbage into phasing tasks, which must still be resolved. The resolution requires either dirty workspace (a register of $n - 2$ qubits to target with carry-xor) or the [[Half-Register Dirty Workspace Borrowing]] trick. You don't get something for nothing — you trade qubit garbage for classical bookkeeping and deferred gate cost.

Requires mid-circuit measurement and classical feedforward of measurement outcomes. In the surface code, this is the native mode of operation. In other architectures, measurement latency may be a concern.

Compared with compute-copy-uncompute, venting is most attractive when the garbage is classically recoverable from surviving quantum data but expensive to uncompute coherently. If the ordinary reverse computation is cheap and workspace is available, measurement venting may not be the simplest choice.

## Related notes
- [[A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) — Paper Notes]] — source paper
- [[Measurement-Asymmetric Uncomputation]] — the general principle: measurement-based erasure of Z-redundant qubits
- [[Temporary Logical-AND]] — an earlier application of the same measurement-based erasure, for AND ancillae rather than carries
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — Gidney's earlier adder; this trick extends the measurement-based approach from AND gates to carry propagation
- [[Dirty Ancilla Constant Addition]] — the prior CQ adder from Häner et al., which venting supersedes
- [[Half-Register Dirty Workspace Borrowing]] — companion trick for eliminating external dirty ancillae from the carry-xor resolution
- [[In-Place Majority for Carry Propagation]] — the MAJ gate that computes each carry before it is vented
