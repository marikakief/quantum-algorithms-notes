# Quantum Sieve for Labelled Qubits

> **Source:** Greg Kuperberg, arXiv:quant-ph/0302112 (2005)
> **Tags:** #trick #quantum-sieve #dihedral-HSP #subexponential

## What it does
Transforms a collection of randomly-labelled qubit states into qubit states with *target* labels, by iteratively combining pairs. Reduces exponential problems to subexponential cost.

## The trick
Start with qubits $|\psi_k\rangle \propto |0\rangle + e^{2\pi i ks/N}|1\rangle$ where $k$ is a known random label and $s$ is the hidden value to find.

**Summand extraction:** Given $|\psi_k\rangle$ and $|\psi_\ell\rangle$:
1. Tensor them together (2-qubit state)
2. Apply CNOT
3. Measure the target qubit

The result is $|\psi_{k+\ell}\rangle$ or $|\psi_{k-\ell}\rangle$, each with probability $1/2$. The label of the output qubit is $k \pm \ell$ — known from the measurement outcome.

**The sieve:** Organise qubits into stages. At each stage, pair qubits whose labels agree in certain low-order bits. When $k$ and $\ell$ agree in $m$ bits, $k - \ell$ gains $\geq m$ trailing zeros with probability $1/2$. After $O(\sqrt{n})$ stages of $O(\sqrt{n})$ bit-clearing each, the label reaches the target (e.g., $2^{n-1}$). Starting from $2^{O(\sqrt{n})}$ qubits, the sieve produces the target state.

This is the quantum analogue of the Blum-Kalai-Wasserman classical sieve for learning parity with noise, and of the Ajtai-Kumar-Sivakumar sieve for shortest lattice vector. The quantum version is faster because the qubit states carry phase information that classical coin flips cannot.

## When to reach for it
- Hidden subgroup / hidden shift problems where [[Coset Sampling via Fourier Transform|Fourier sampling]] yields labelled qubit states with random labels
- Any problem where you have quantum states indexed by group elements and need to "steer" toward a target element via algebraic combinations
- The dihedral HSP, generalised dihedral HSP, and hidden shift for abelian groups
- Could potentially apply to other nonabelian HSPs where the representations are low-dimensional

## Complexity
$2^{O(\sqrt{\log N})}$ time, queries, and space for the basic sieve. For smooth $N = r^n$, the greedy sieve achieves $e^{O(\sqrt[3]{2\log^3 N})}$.

## Caveat
- Requires $2^{O(\sqrt{\log N})}$ qubits stored simultaneously — the main bottleneck. [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev (2004)]] trades this for a slightly higher time complexity with polynomial space.
- Only works when the representations of the group are low-dimensional (qubits or small). For the symmetric group, irreps are typically very large and the sieve loses control.
- Each combination step destroys two qubits to produce (at best) one. The sieve is inherently lossy — about half the attempts fail, and the useful output shrinks by $\sim 4\times$ per stage.

## Related notes
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]]
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes]]
- [[Coset State to Labelled Qubit Reduction]]
- [[Coset Sampling via Fourier Transform]]
- [[Entangled Multi-Copy Measurements for Nonabelian HSP]]
