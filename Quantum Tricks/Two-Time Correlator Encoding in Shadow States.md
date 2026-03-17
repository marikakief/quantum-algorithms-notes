
> **Tags:** #trick #correlators #greens-function #shadow-simulation
> **Source:** Shadow Hamiltonian Simulation, Nature Comms 16, 3486 (2025)

## What it does
Encodes two-time (or $q$-time) correlators $\langle O_m(t) O_{m'}(t') \rangle$ in the amplitudes of a quantum state, without preparing the full system state at either time.

## The trick
Define the two-time shadow state:
$$|\rho; S(t, t')\rangle \propto \sum_{m, m'} \langle O_m(t) O_{m'}(t') \rangle |m, m'\rangle$$

**Theorem 2:** This state evolves as:
$$\frac{\partial}{\partial t}|\rho; S(t,t')\rangle = -i(H_S \otimes \mathbb{1})|\rho; S(t,t')\rangle$$
$$\frac{\partial}{\partial t'}|\rho; S(t,t')\rangle = -i(\mathbb{1} \otimes H_S)|\rho; S(t,t')\rangle$$

So: [[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] $|\rho; S(0,0)\rangle$, evolve first register with $H_S$ for time $t$, second register with $H_S$ for time $t'$.

Generalises to $q$-time correlators with $q$ registers and to products of operators evolving under **different** Hamiltonians.

## When to reach for it
- Green's functions $G(t, t') = \langle c_j(t) c_k^\dagger(t') \rangle$
- Dynamical structure factors, response functions
- Any two-point function that would otherwise require full time evolution + Hadamard test

## Complexity
Twice the cost of single-time shadow simulation (two independent evolutions of $H_S$).

## Caveat
Initial state $|\rho; S(0,0)\rangle$ encodes equal-time correlators $\langle O_m O_{m'}\rangle$ — must be efficiently preparable. For Gaussian/product states this is often tractable.

## Related Paper Notes

- [[Shadow Hamiltonian Simulation — Paper Notes]]
