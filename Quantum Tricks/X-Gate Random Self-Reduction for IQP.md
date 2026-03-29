# X-Gate Random Self-Reduction for IQP

> **Source:** Bremner-Montanaro-Shepherd, arXiv:1504.07999
> **Tags:** #trick #IQP #self-reduction #quantum-supremacy #sampling

## What it does
Converts the task of estimating a single IQP output probability into a sampling problem over all output probabilities, without leaving the IQP circuit class.

## The trick
For an IQP circuit $C$ on $n$ qubits, define $C_x$ by appending an $X$ gate to qubit $i$ for each $i$ with $x_i = 1$. Then:

$$\Pr[C_0 \text{ outputs } y] = |\langle y | C_0 | 0\rangle|^2 = |\langle 0 | C_y | 0\rangle|^2$$

because appending $X$ gates to an IQP circuit just permutes the output probabilities. The resulting circuit $C_x$ is still IQP — $X$ gates commute with the Hadamard layers, so they just change the diagonal phase pattern.

**Consequence:** A classical sampler that approximates the output distribution of *any* IQP circuit to $\ell_1$ error $\epsilon$ can be used (via Stockmeyer's theorem) to multiplicatively estimate $|\langle 0|C_x|0\rangle|^2$ for a random $x$. Combined with anticoncentration, this gives average-case hardness from worst-case hardness.

## When to reach for it
- Any quantum supremacy proof for a circuit class that's closed under local $X$ (or local $Z$) gates
- When you need to connect sampling hardness to amplitude estimation
- Proving random self-reducibility for a quantum sampling problem

## Complexity
No cost — it's a structural observation about IQP circuits, not a computation.

## Caveat
This works because IQP circuits are closed under appending $X$ gates. For BosonSampling, the analogous move (averaging over random unitaries) is more complicated because the resulting circuit may not be a simple linear-optical network. The simplicity of the IQP self-reduction is one of the main advantages of the IQP setting over BosonSampling.

## Related notes
- [[Average-Case Complexity Versus Approximate Simulation of Commuting Quantum Computations (Bremner-Montanaro-Shepherd 2016) — Paper Notes]]
- [[Paley-Zygmund Anticoncentration for Quantum Amplitudes]]
- [[Post-Selection Boosting for Hardness of Sampling]]
- [[Hadamard Gadget for IQP Reduction]]
- [[IQP Amplitudes as Ising Partition Functions]]
