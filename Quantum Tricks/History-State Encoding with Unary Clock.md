# History-State Encoding with Unary Clock

> **Source:** Kitaev (2002), adapted by Aharonov, van Dam, Kempe, Landau, Lloyd, Regev, arXiv:quant-ph/0405098
> **Tags:** #trick #history-state #clock #encoding #fundamental

## What it does

Encodes the entire trajectory of an $L$-step quantum computation into a single quantum state — the history state — using a unary clock register to index time steps.

## The construction

Given a circuit $U_1, \ldots, U_L$ on $n$ qubits with intermediate states $|\alpha(\ell)\rangle = U_\ell \cdots U_1 |0^n\rangle$, define:

$$
|\eta\rangle = \frac{1}{\sqrt{L+1}} \sum_{\ell=0}^{L} |\alpha(\ell)\rangle \otimes |1^\ell 0^{L-\ell}\rangle_c
$$

The clock register uses **unary encoding**: time step $\ell$ is represented as $\ell$ ones followed by $L - \ell$ zeros.

```
 Computation     Clock (unary)
 register        register

 |α(0)⟩    ⊗    |000...0⟩     ← step 0
 |α(1)⟩    ⊗    |100...0⟩     ← step 1 (U₁ applied)
 |α(2)⟩    ⊗    |110...0⟩     ← step 2 (U₂ applied)
   ⋮              ⋮
 |α(L)⟩    ⊗    |111...1⟩     ← step L (all gates applied)

 History state |η⟩ = equal superposition of all rows
```

## Why unary?

Legality of the clock state can be checked **2-locally**: just forbid the substring "01" anywhere in the clock register. Any state of the form $|1^\ell 0^{L-\ell}\rangle$ passes; anything else fails. This makes the clock Hamiltonian

$$
H_{\mathrm{clock}} = \sum_{\ell=1}^{L-1} |01\rangle\langle 01|_{c_\ell, c_{\ell+1}}
$$

a sum of nearest-neighbor projectors.

## Why the history state can be verified locally

Each propagation check — "did step $\ell$ apply $U_\ell$ correctly?" — involves:
- Two adjacent clock qubits (to detect the $\ell$-th transition)
- The computation qubits that $U_\ell$ acts on (at most 2 for a 2-qubit gate)

Total locality: 5-local in the basic construction (3 clock + 2 computation). Reducible to 3-local via the [[Perturbation Lemma for Locality Reduction|perturbation lemma]], and to 2-local nearest-neighbor via the [[Snake-Path 2D Embedding for Nearest-Neighbor Universality|snake-path embedding]].

The history state is the unique zero-energy ground state of $H_{\mathrm{final}} = \frac{1}{2}\sum_\ell H_\ell + H_{\mathrm{input}} + H_{\mathrm{clock}}$, where each $H_\ell$ penalizes incorrect propagation.

## When to reach for it

- Adiabatic computation constructions (the original use)
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Quantum algorithms for ODEs/PDEs]] via linear system encodings
- QMA-completeness proofs (local Hamiltonian problem)
- Any setting where you need to embed a sequential process into the ground state of a local Hamiltonian

## Cost

- Clock register: $L$ qubits (one per time step)
- Output extraction: measuring the clock gives time step $\ell = L$ with probability $1/(L+1)$ — boost with [[Identity-Gate Padding for Output Amplification]]

## Caveat

The unary clock is space-inefficient ($L$ qubits for $L$ time steps). Binary clocks save space but lose 2-local verifiability. The tradeoff depends on whether locality or qubit count matters more.

## Related notes

- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[Identity-Gate Padding for Output Amplification]]
- [[History-State Padding for Final-Time Readout]]
- [[Snake-Path 2D Embedding for Nearest-Neighbor Universality]]
- [[Perturbation Lemma for Locality Reduction]]
- [[Hamiltonian-to-Markov-Chain Spectral Gap Mapping]]
