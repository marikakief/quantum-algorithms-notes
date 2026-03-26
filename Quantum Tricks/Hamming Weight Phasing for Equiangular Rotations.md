# Hamming Weight Phasing for Equiangular Rotations

> **Source:** Gidney, Quantum **2**, 74 (2018); extended by Kivlichan, Gidney, Babbush et al., arXiv:1902.10673 (2020)
> **Tags:** #trick #fault-tolerant #T-gate-reduction #rotation-synthesis #Trotter #circuit-optimization

## What it does

Reduces $n$ identical parallel $R_z(\theta)$ rotations to $\lfloor \log_2 n + 1 \rfloor$ rotations on an ancilla register, at the cost of $n - 1$ Toffoli gates (or $4n - 4$ T gates) and $n - 1$ ancilla qubits.

## The trick

When $n$ qubits each need the same rotation $R_z(\theta)$, the total phase accumulated is $\theta \cdot k$ where $k$ is the number of qubits in state $|1\rangle$ (the Hamming weight). Compute the Hamming weight on an ancilla register using a Toffoli adder network, apply $\lfloor \log_2 n + 1 \rfloor$ rotations to the binary-encoded Hamming weight bits (angles $\theta, 2\theta, 4\theta, \ldots$), then uncompute.

The rotation synthesis cost drops from $n \cdot c \log(1/\varepsilon)$ to $(\log n) \cdot c \log(1/\varepsilon)$ T gates, at the price of $O(n)$ Toffoli gates. When $n$ is large and $\varepsilon$ is small, the synthesis savings dominate.

**Limited-ancilla variant:** With only $r < n-1$ ancilla qubits, batch $r+1$ rotations at a time in $\lceil n/(r+1) \rceil$ rounds. Alternatively, a $\sqrt{n}$-grouping method uses $\lfloor\sqrt{n}\rfloor + \lfloor\log_2 n\rfloor$ ancilla at roughly $2\times$ the Toffoli cost.

## When to reach for it

- Any [[Trotterized Time Evolution for Chemistry|Trotter step]] where the same rotation angle appears on many qubits — especially translation-invariant Hamiltonians (Hubbard, jellium, periodic materials)
- Split-operator Trotter steps where the [[Fermionic Fast Fourier Transform (FFFT)]] produces many identical rotations
- Any fault-tolerant circuit where rotation synthesis cost dominates and parallel rotations share angles

## Complexity

- **Toffoli gates:** $n - 1$ (full ancilla), or $\sim 2n$ ($\sqrt{n}$-grouping)
- **Synthesized rotations:** $\lfloor \log_2 n + 1 \rfloor$ instead of $n$
- **Ancilla:** $n - 1$ (full), or $O(\sqrt{n} + \log n)$ (limited)
- **Net T savings:** positive when $n > c \log(1/\varepsilon) / 4$ (typically $n > 10$–$20$)

## Caveat

Only works when the $n$ rotations have exactly the same angle. If angles differ, you need to decompose into equiangular groups first — see [[Translation-Invariant Hamming Weight Grouping]] for how translation symmetry enables this. Randomized Trotter methods (e.g., Campbell 2019) typically destroy the equiangular structure, making Hamming weight phasing inapplicable.

## Related notes
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — primary application paper
- [[Translation-Invariant Hamming Weight Grouping]] — ordering trick that maximizes equiangular groups
- [[Rotation Catalysis via Hamming Weight Phasing]] — further optimization for specific angles
- [[Fermionic Fast Fourier Transform (FFFT)]] — produces equiangular rotations in split-operator steps
- [[Trotterized Time Evolution for Chemistry]] — general Trotter framework where this applies
