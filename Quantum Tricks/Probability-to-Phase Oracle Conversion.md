
> **Source:** Gilyén, Arunachalam, Wiebe, arXiv:1711.00465 (2019)
> **Tags:** #trick #oracle #amplitude-estimation #VQE #QAOA

## What it does
Converts a probability oracle (where $f(x)$ is accessed as a measurement probability) into a phase oracle (where $f(x)$ appears as a phase $e^{if(x)}$), with only logarithmic overhead.

## The trick

Quantum optimisation procedures naturally produce probability oracles: given parameters $\theta$, prepare $|\psi(\theta)\rangle$ and measure — the objective function $f(\theta) = \langle \psi(\theta) | O | \psi(\theta) \rangle$ is an expectation value estimated from measurement statistics.

Gradient algorithms ([[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes|Jordan]], Bernstein-Vazirani) need phase oracles: $|\theta\rangle \to e^{if(\theta)} |\theta\rangle$.

**Conversion:** Use amplitude estimation to extract $f(\theta)$ as a phase. Amplitude estimation on $|\psi(\theta)\rangle$ with precision $\varepsilon$ requires $O(1/\varepsilon)$ calls to the state-preparation unitary, but the result is stored as a phase in the estimation register — exactly what the gradient algorithm needs.

The total overhead is $O(\log(1/\varepsilon))$ multiplicative factor on the number of oracle calls.

## When to reach for it
- Any quantum gradient computation where the objective is a measurement probability or expectation value (VQE, QAOA, quantum autoencoders, quantum ML)
- Bridging the gap between "run the circuit and measure" (natural for near-term quantum computing) and "encode as phase" (natural for quantum algorithms)

## Complexity
$O(\log(1/\varepsilon))$ overhead per gradient oracle call, where $\varepsilon$ is the required precision of $f$.

## Caveat
The conversion uses amplitude estimation, which requires coherent access to the state-preparation circuit (not just measurement samples). This means the optimisation circuit must be run coherently as a subroutine, not just sampled — a real constraint for near-term devices with limited coherence.

## Related notes
- [[Optimizing Quantum Optimization Algorithms via Faster Quantum Gradient Computation (Gilyén-Arunachalam-Wiebe 2019) — Paper Notes]]
- [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes]]
- [[Amplitude Estimation via Phase Estimation on Q]]
- [[Fourier-Eigenstate Kickback for Arithmetic Oracles]]
