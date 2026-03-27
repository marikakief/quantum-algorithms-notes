
> **Tags:** #trick #heisenberg-picture #operator-growth #scrambling
> **Source:** Shadow Hamiltonian Simulation, Nature Comms 16, 3486 (2025)

## What it does
Encodes a time-evolved operator $Z(t)$ as a quantum state $|Z(t)\rangle$, enabling measurement of operator properties (weight, spread, correlations) via standard quantum operations.

## The trick
Given $Z(t) = \sum_m z_m(t) O_m$ (expanding in the operator basis $S$), map:
$$Z(t) \;\mapsto\; |Z(t)\rangle \propto \sum_m z_m(t) |m\rangle$$

If the IP holds, $|Z(t)\rangle$ evolves under $\overline{H_S}$ (complex conjugate of the shadow Hamiltonian):
$$\frac{d}{dt}|Z(t)\rangle = -i\overline{H_S}|Z(t)\rangle$$

[[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] $|Z(0)\rangle$ (often a simple computational basis state for a local operator), evolve with $\overline{H_S}$, then measure properties of $|Z(t)\rangle$.

## When to reach for it
- Studying operator growth / scrambling / light cones
- Learning unitary operations (Clifford characterisation in $2n$ experiments)
- Computing out-of-time-order correlators (OTOCs) indirectly
- Any problem where operator dynamics is more natural than state dynamics

## Complexity
Same as simulating $H_S$: $O(t \|H_S\| + \log(1/\varepsilon))$ via [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]]. See [[Shadow State Compression via Invariance Property]] for the general framework.

## Caveat
Requires IP closure, same as [[Shadow State Compression via Invariance Property|shadow states]]. Measuring $|Z(t)\rangle$ gives information about operator coefficients $z_m(t)$, not directly about physical observables on a state. The operator weight $\sum_m z_m(t)^2 \cdot \text{wt}(O_m)$ is a natural diagnostic but needs interpretation.

## Related Paper Notes

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
- [[Shadow Hamiltonian Simulation — Paper Notes]]
