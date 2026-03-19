# Quantum Shannon Counting Argument

> **Source:** The circuit-counting observation is folklore (see e.g. Knill 1995, Nielsen & Chuang 2000). The extension to arbitrary time-dependent Hamiltonian evolution — which requires proving a derivative-free simulation result first — is due to Poulin, Qarry, Somma & Verstraete, arXiv:1102.1360 (2011).
> **Tags:** #trick #complexity-theory #counting-argument #hilbert-space

## What it does

Shows that the overwhelming majority of quantum states in $(\mathbb{C}^d)^{\otimes N}$ are unreachable by polynomial-time local Hamiltonian evolution — the quantum analogue of Shannon's proof that most Boolean functions have no small circuits.

## The trick

**Classical Shannon argument (for reference).** There are $2^{2^N}$ Boolean functions on $N$ bits. Circuits of size $\mathrm{poly}(N)$ from a finite gate set can compute at most $|\mathcal{G}|^{\mathrm{poly}(N)} = 2^{\mathrm{poly}(N)}$ distinct functions. Since $2^{\mathrm{poly}(N)} \ll 2^{2^N}$, most functions are hard.

**Quantum version.** The Hilbert space of $K$ qubits has dimension $2^K$, and the unit sphere lives in $\sim 2 \cdot 2^K$ real dimensions. The argument has two steps:

1. **Simulation:** Any local time-dependent Hamiltonian evolution of duration $T = \mathrm{poly}(K)$ can be $\varepsilon$-approximated by a circuit of $G = \mathrm{poly}(K)$ gates from a finite universal set of size $M$ (via [[Generalised Trotter for Time-Dependent Hamiltonians|generalised Trotter]] + [[Solovay-Kitaev Recursive Gate Compilation|Solovay-Kitaev]]). This is the non-trivial step — it requires handling arbitrary time-dependence without smoothness assumptions.

2. **Counting:** The number of distinct circuits of polynomial size on $K$ qubits is at most $(MK^2)^{K^\alpha}$ for some constant $\alpha$. Place an $\varepsilon$-ball around each circuit output. The total covered volume fraction is $O(K^K \varepsilon^{2K})$ of the full Hilbert sphere — exponentially small in $K$.

So "physical" states — those reachable in polynomial time — form an exponentially small submanifold of Hilbert space.

## When to reach for it

- Arguing that a particular quantum state or family of states is "non-physical" / exponentially hard to prepare.
- Establishing that tensor network or variational ansätze, which also parametrise exponentially small submanifolds, are not obviously missing most states that matter.
- Setting up complexity-theoretic separations between Hamiltonian-based and circuit-based models (spoiler: there is no separation — they're equivalent).
- Contrasting with the classical case, where every $N$-bit string is trivially preparable by a depth-1 circuit. The difference comes from entanglement.

## Complexity

The argument is non-constructive — it doesn't tell you *which* states are unreachable, only that almost all of them are. The key quantitative statement is:

$$\frac{\text{covered volume}}{\text{total Hilbert sphere volume}} = O(K^K \varepsilon^{2K}) \to 0 \text{ exponentially fast in } K.$$

## Caveat

- The notion of "physical" here is defined by polynomial-time preparation from a fiducial state. States that arise as ground states of local Hamiltonians, or thermal equilibrium states, might be physically relevant even if they're hard to *prepare* — the argument is about dynamics, not statics.
- The counting argument doesn't give any useful structural characterisation of the reachable manifold beyond its measure. Compare with tensor network theory, which gives more explicit descriptions of structured subsets.
- Relies on the [[Generalised Trotter for Time-Dependent Hamiltonians|generalised Trotter]] simulation result. If you weaken the Hamiltonian model (e.g., allow unbounded terms), you'd need a different simulation theorem to close the argument.

## Related notes
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes]]
- [[Generalised Trotter for Time-Dependent Hamiltonians]]
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]]
