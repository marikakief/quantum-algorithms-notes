# Dirty Ancilla Constant Addition

> **Source:** Häner, Roetteler, Svore, arXiv:1611.07995
> **Tags:** #trick #quantum-arithmetic #adder #dirty-qubits #Toffoli #circuit-primitive

## What it does

Adds a classically known $n$-bit constant $c$ to an $n$-qubit register $|a\rangle$ in place, using only dirty ancilla qubits (qubits in unknown states borrowed from other parts of the computation).

## The trick

The construction uses divide-and-conquer on the bit-register, with a [[Carry Computation via Dirty Qubit Toggling|carry gate]] as the base primitive.

**Step 1 — Split.** Divide $a$ into high bits $x_H$ and low bits $x_L$ of roughly $n/2$ bits each.

**Step 2 — Carry.** Compute the carry from $x_L + c_L$ into a single qubit, borrowing $x_H$ as dirty ancillae. This carry gate uses $4(n/2 - 2) + 2$ Toffoli gates and returns all borrowed qubits to their original state.

**Step 3 — Increment.** Conditionally increment $x_H$ based on the carry. The incrementer uses $n/2$ borrowed dirty qubits and the identity $|x\rangle|g\rangle \to |x - g\rangle|g\rangle \to |x - g\rangle|\bar{g}\rangle \to |x - g - \bar{g}\rangle|\bar{g}\rangle = |x + 1\rangle|g\rangle$, where $\bar{g}$ is the bitwise complement (so $g + \bar{g} + 1 = 2^n$, giving $-g - \bar{g} = +1 \bmod 2^n$). This uses the ancilla-free adder of Takahashi (2009) and its reverse.

**Step 4 — Recurse.** Recursively add $c_L$ to $x_L$ and $c_H$ to $x_H$.

When the ancilla qubit is dirty rather than clean, the incrementer runs twice with a conditional inversion in between.

At the lowest recursion level, 1-bit additions are just conditional NOT gates.

**Toffoli count:**

$$T_{\text{add}}(n) = 8n(\log_2 n - 2) + O(1)$$

**Depth:** $O(n)$ (serial version; the parallel version computes low-half and high-half carries simultaneously using $n/2$ additional dirty qubits).

## When to reach for it

- Implementing [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]] at the $2n + 2$ qubit limit, where no clean ancillae are available for the adder.
- Any circuit where classical constants must be added to quantum registers and the only available workspace is dirty qubits (entangled with other parts of the computation).
- Situations where avoiding rotation synthesis (no QFT) is desirable for fault-tolerant implementations.

## Complexity

- **Size:** $O(n \log n)$ Toffoli gates
- **Depth:** $O(n)$
- **Ancillae:** 1 dirty qubit minimum (serial), up to $n$ dirty qubits for full parallelism
- **Controlled version:** only the two final CNOT gates of the CARRY circuit need to be made controlled

## Caveat

The $O(n \log n)$ gate count is a $\log n$ factor worse than the $O(n)$-gate adders of [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro (2004)]] or Takahashi (2009), which require $n$ clean ancillae. Whether a linear-size constant adder exists using only dirty ancillae is an open question (as of the paper's publication).

The adder handles classical-to-quantum addition only ($c$ is known at circuit-generation time). For quantum-to-quantum addition ($|a\rangle + |b\rangle$), use [[MAJ-UMA Decomposition for Reversible Adders|Cuccaro's construction]] instead.

**Superseded:** [[A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) — Paper Notes|Gidney (2025)]] achieves $O(n)$ Toffolis with $O(1)$ clean ancillae for the same CQ addition problem, eliminating the $\log n$ factor. This construction remains relevant when only dirty ancillae are available and no clean ancillae at all can be provided.

## Related notes
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]] — source paper
- [[Carry Computation via Dirty Qubit Toggling]] — the carry gate used as the base primitive
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes]] — the clean-ancilla alternative
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes]] — the QFT-based alternative (also zero-ancilla but uses rotations)
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — later T-count optimisation applicable to paired Toffolis
- [[Dirty Qubit Recycling for T-Gate Reduction]] — the general principle of borrowing idle qubits
- [[Reversible Modular Exponentiation with Garbage Cleanup]] — the context in which this adder is used
- [[A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) — Paper Notes]] — supersedes this construction with $O(n)$ Toffolis and $O(1)$ clean ancillae
- [[Carry Venting via X-Basis Measurement]] — the technique that enables the $O(n)$ CQ adder
