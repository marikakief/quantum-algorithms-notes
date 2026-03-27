
> **Source:** Sanders, Low, Scherer, Berry, arXiv:1807.03206, PRL 122, 020502 (2019)
> **Tags:** #trick #state-preparation #comparator #amplitude-loading #Toffoli-count

## What it does
Converts a classical coefficient value stored in a quantum register into a proportional amplitude, without computing any transcendental functions. Replaces the $O(n^2)$-Toffoli arcsine computation in Grover's state preparation with an $n$-Toffoli inequality test.

## The trick

Given oracle output $\alpha_\ell^{(n)} \in \{0, \ldots, 2^n - 1\}$ in a `data` register:

1. **Prepare uniform superposition** on a fresh $n$-qubit `ref` register:
$$H^{\otimes n}|0^n\rangle_{\text{ref}} = \frac{1}{\sqrt{2^n}} \sum_{x=0}^{2^n-1} |x\rangle_{\text{ref}}$$

2. **Inequality test** (comparator): set a `flag` qubit to $|0\rangle$ if $x < \alpha_\ell^{(n)}$, else $|1\rangle$.

The amplitude on the $|0\rangle_{\text{flag}}$ subspace for index $\ell$ is:

$$\frac{\alpha_\ell^{(n)}}{\sqrt{2^n}} \approx \sqrt{2^n}\, \alpha_\ell$$

— proportional to the coefficient $\alpha_\ell$. For **root coefficients** ($\sqrt{\alpha_\ell}$), skip the Hadamard un-preparation of `ref`: the amplitude is then $\sqrt{\alpha_\ell^{(n)}/2^n}$.

**Cost:** The comparator is a ripple-carry comparison requiring exactly $n$ Toffoli gates. Compare to computing $\arcsin(\alpha/2^n)$: $>4800$ Toffolis at $n=17$, $>11000$ at $n=30$.

## When to reach for it
- Any black-box state preparation (PREPARE oracle) where coefficients are given as $n$-bit fixed-point values
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Qubitization]] and [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|LCU]] PREPARE oracles
- Quantum walk coin operators
- Any setting where you'd otherwise use Grover's arcsine-based rotation

## Complexity
$n$ Toffoli gates per round of [[Standard Amplitude Amplification|amplitude amplification]]. Total state preparation cost: $O(n/\delta)$ Toffolis where $\delta = |\vec{\alpha}|_2 / \sqrt{d}$ is the overlap with the uniform state.

## Caveat
- Introduces $n$ ancilla qubits for the `ref` register (reusable after un-preparation)
- For root coefficients, the final `unif`$^{-1}$ cleanup costs $O(n^2)$ Toffolis, paid once
- Cartesian root coefficients ($\sqrt{a + ib}$) still need some arithmetic for the quartic test — not fully arithmetic-free
- The coefficients must be non-negative for the basic version; sign/phase handling adds a flag qubit and $Z$ gate

## Related notes
- [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes]]
- [[Comparator-Based Direct Amplitude Sampling from Fixed-Point Coefficients]] — later variant using SELECT-SWAP grouped lookup
- [[Standard Amplitude Amplification]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
