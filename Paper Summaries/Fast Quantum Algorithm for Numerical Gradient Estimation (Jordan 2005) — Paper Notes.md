> **Source:** Stephen P. Jordan, *Fast Quantum Algorithm for Numerical Gradient Estimation*, Phys. Rev. Lett. **95**, 050501 (2005); arXiv:quant-ph/0405146
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0405146) · [PRL](https://doi.org/10.1103/PhysRevLett.95.050501)
> **Tags:** #oracle #gradient #phase-kickback #numerical-methods #Bernstein-Vazirani

---

## The problem

Given a black box evaluating a smooth function $f: \mathbb{R}^d \to \mathbb{R}$ at points on a grid with $n$ bits of precision, estimate the gradient $\nabla f$ at a given point $\mathbf{x}$. Classically, you need $d + 1$ queries (evaluate $f$ at $\mathbf{x}$ and at $d$ displaced points). Can a quantum computer do better?

## What the paper does

**One query.** Regardless of dimension $d$, a single quantum query to the black box suffices to estimate all $d$ partial derivatives to $n$ bits of precision. The precision in evaluating $f$ matches the classical requirement in the large-$n$ limit (up to an additive constant in bits).

The algorithm is a generalisation of [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes|Bernstein-Vazirani]] to continuous functions — the first practical application of that algorithmic template.

---

## The algorithm

### The oracle

The black box takes $d$ input registers of $n$ qubits each (encoding a grid point $\boldsymbol{\delta}$) plus an output register of $n_o$ qubits. It computes $f$ in fixed-point arithmetic and adds the result modulo $2^{n_o}$ into the output register:

$$|{\boldsymbol{\delta}}\rangle |a\rangle \to |{\boldsymbol{\delta}}\rangle |a \oplus \lceil f(\boldsymbol{\delta}) \rceil \rangle$$

### The trick: phase kickback from modular addition

This is where it gets clever. Prepare the output register in a Fourier basis state — specifically, write 1 into it and inverse-Fourier-transform:

$$\frac{1}{\sqrt{N_o}} \sum_{a=0}^{N_o - 1} e^{i2\pi a / N_o} |a\rangle$$

This state is an **eigenstate of addition modulo $N_o$** with eigenvalue $e^{i2\pi x/N_o}$ for addition of $x$. So when the oracle adds $f(\boldsymbol{\delta})$ to this register, the result is pure [[Phase Kickback from Oracle Queries|phase kickback]]:

$$|{\boldsymbol{\delta}}\rangle \to e^{i2\pi f(\boldsymbol{\delta})/N_o'} |{\boldsymbol{\delta}}\rangle$$

where $N_o'$ absorbs the scaling. The output register is unchanged — it factors out completely.

### Full circuit

1. Prepare $d$ input registers in $|0\rangle^{\otimes n}$, output register with value 1 then inverse-QFT
2. Hadamard all input qubits → uniform superposition over grid points $\boldsymbol{\delta}$
3. Query the oracle → phase kickback encodes $f(\boldsymbol{\delta})$ as phases
4. For $f$ approximately linear near $\mathbf{x}$, the state factors as:

$$\bigotimes_{i=1}^{d} \left( \frac{1}{\sqrt{N}} \sum_{\delta_i} e^{i2\pi \delta_i \cdot \partial f/\partial x_i / m} |\delta_i\rangle \right)$$

5. QFT each input register → the gradient components appear in the computational basis
6. Measure → read off $\nabla f$ with $n$ bits of precision

The linearity approximation is what makes this work: when $f$ is approximately linear, the phases encode the gradient as frequencies in each register independently. The registers are in a **product state** — no entanglement between dimensions.

---

## Error analysis

The error comes from the quadratic (Hessian) terms in the Taylor expansion. Using stationary phase analysis:

$$\sigma_i \approx \frac{l}{2\sqrt{3}} \sqrt{\sum_j \left(\frac{\partial^2 f}{\partial x_i \partial x_j}\right)^2}$$

where $l$ is the size of the sampling region. Compare to classical central differences: $\sigma \sim l^2 D_3 / 24$ where $D_3$ is the typical magnitude of third derivatives. The quantum error scales with the Hessian (second derivatives) rather than third derivatives — worse per unit of $l$, but since $l$ appears linearly instead of quadratically, the tradeoff is manageable.

The optimal $l$ differs between quantum and classical cases, but in both cases the required precision $n_o$ for evaluating $f$ scales the same way (proportional to $n$), differing only by an additive constant.

---

## Key results

| Setting | Queries for $d$-dimensional gradient |
|---|---|
| Classical | $d + 1$ (one-sided) or $2d$ (centered) |
| Quantum | $1$ (any $d$) |
| Precision match | Quantum matches classical $n_o$ up to $O(1)$ additive bits |

---

## Why this is neat

The oracle construction is the interesting part, as you noted. Standard [[Phase Kickback from Oracle Queries|phase kickback]] works on a $|{-}\rangle$ ancilla for Boolean oracles. Here the oracle computes a real-valued function via modular addition, and the kickback comes from preparing the output register in a **Fourier basis eigenstate of the addition operator**. This is the same mechanism used in [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]] (where controlled multiplication replaces controlled addition), but applied to a completely different problem.

The product-state structure after kickback — each dimension decouples — is also elegant. It's why one query suffices regardless of $d$: the gradient has $d$ components, but they're encoded in $d$ independent registers, not entangled.

---

## Limits / caveats

- **Higher derivatives don't get the same speedup.** Recursing the algorithm as a subroutine introduces a global phase that requires an extra query to remove, giving $2^n$ queries for $n$-th derivatives — same as classical.
- **Smoothness required.** The function must be smooth enough that it's well-approximated by its linear Taylor expansion over the sampling region. Non-smooth functions break the algorithm.
- **The Hessian error** means the quantum algorithm is somewhat more sensitive to curvature than the classical one (linear in $l$ vs. quadratic in $l$). For highly curved functions, $l$ must be smaller, requiring more bits in $f$.
- **Practical impact unclear.** The one-query advantage is dramatic in query complexity but the actual speedup depends on whether function evaluation is the bottleneck.

---

## Reusable ideas

1. [[Fourier-Eigenstate Kickback for Arithmetic Oracles]] — prepare the output register of an arithmetic oracle (addition mod $N$) in a Fourier basis eigenstate so that the function value kicks back as a phase. Generalises Boolean phase kickback to real-valued functions.

---

## References within this paper

- Bernstein & Vazirani (1993) — the discrete version of the same algorithm (inner product mod 2 → gradient)
- Mosca (1999, PhD thesis) — context for the Bernstein-Vazirani generalisation
- Cleve, Ekert, Macchiavello, Mosca (1998) — [[Gapped Phase Estimation|phase estimation]]
- Nielsen & Chuang (2000) — QFT properties used in the precision analysis

---

## Cross-links

### Paper notes
- [[Optimizing Quantum Optimization Algorithms via Faster Quantum Gradient Computation (Gilyén-Arunachalam-Wiebe 2019) — Paper Notes]] — direct improvement: quadratically better precision via higher-order stencils, plus oracle conversions
- [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes]] — the Hadamard-sandwich structure this generalises
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — same kickback mechanism (Fourier eigenstates of modular arithmetic)
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes]] — the discrete algorithm this generalises (hidden inner product → gradient)
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — related oracle algorithm lineage

### Paper notes (continued)
- [[Polynomial-Time Quantum Algorithm for the Simulation of Chemical Dynamics (Kassal-Jordan-Love-Mohseni-Aspuru-Guzik 2008) — Paper Notes]] — co-author Jordan; applies the same Fourier-eigenstate kickback to evaluate Coulomb potentials in chemical dynamics

### Trick cards
- [[Fourier-Eigenstate Kickback for Arithmetic Oracles]]
- [[Phase Kickback from Oracle Queries]] — the Boolean special case
- [[Hadamard Sandwich for Global Function Properties]] — the structural template
