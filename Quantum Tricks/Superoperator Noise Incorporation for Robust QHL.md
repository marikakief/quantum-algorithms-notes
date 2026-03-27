
> **Source:** Wiebe, Granade, Ferrie, Cory, arXiv:1311.5269 (PRA 89, 042314, 2014)
> **Tags:** #trick #hamiltonian-learning #noise-robustness #superoperator #process-tomography #IQLE

## What it does
Handles realistic multi-qubit gate errors in Hamiltonian learning by extracting the noisy channel as a superoperator from experiment (or simulation) and plugging it into the likelihood model — leaving the outer inference algorithm completely unchanged.

## The trick

The IQLE protocol requires a swap gate between the untrusted device and the trusted simulator. Real swap gates have fidelity $< 1$. Rather than assuming a parametric noise model, the paper models the full noise as a **superoperator** $\Lambda_\mathrm{noise}$.

**Step 1: Extract the noisy channel.**
Use quantum process tomography (or GRAPE pulse simulation) on the swap gate to obtain the superoperator $\Lambda_\mathrm{noise}$ as a $4^n \times 4^n$ matrix (in Liouville representation). This is a one-time characterisation cost.

**Step 2: Use $\Lambda_\mathrm{noise}$ in the likelihood model.**
For each candidate particle $\mathbf{x}_j$ in the SMC filter, compute the noisy likelihood:

$$\Pr(D|\mathbf{x}_j)_\mathrm{noisy} = \mathrm{Tr}\!\left[|D\rangle\langle D|\, \Lambda_\mathrm{IQLE}(\mathbf{x}_j)\right]$$

where $\Lambda_\mathrm{IQLE}(\mathbf{x}_j)$ applies the full noisy IQLE channel (forward evolution under $H(\mathbf{x}_j)$, noisy swap $\Lambda_\mathrm{noise}$, inversion evolution, measurement). The noisy channel replaces the ideal one; no change to the SMC weight update or PGH.

**Step 3: Run the algorithm unchanged.**
Because the superoperator is incorporated into the likelihood, the Bayesian update automatically accounts for the noise. The algorithm is *self-consistent*: it learns the Hamiltonian as if the noise model were exact.

**Cumulant expansion (equations 14–17):** For small gate errors, the noisy superoperator can be expanded perturbatively in the error generators. The first cumulant gives the leading decoherence effect; higher cumulants capture correlated errors. The paper uses this to justify why moderate swap gate infidelity still leaves the algorithm working.

**Numerical validation:**

| Platform | Swap fidelity | Outcome |
|---|---|---|
| Quantum dots | 0.973 | Rapid convergence, ~6 digits by $N=200$ |
| SC primitive | 0.906 – 0.996 | Still converges; rate reduced |
| SC XY4 decoupled | 0.998 | Near-ideal performance |

## When to reach for it

- Any quantum learning or estimation protocol where a key subroutine gate is noisy: rather than deriving an analytic noise model, characterise the gate experimentally and plug in the superoperator.
- Hamiltonian learning on real hardware where swap or CNOT gates have characterised but non-trivial error channels.
- Generally: when you have a good quantum process tomography estimate of a noisy channel and want to use it in a classical inference loop.

## Complexity

- Superoperator extraction: one-time cost proportional to $4^n$ process tomography experiments. Expensive for large $n$; practical for small ($n = 2$–$3$) interaction blocks.
- Per-experiment overhead: simulating $\Lambda_\mathrm{IQLE}(\mathbf{x}_j)$ classically costs $O(4^n)$ per particle. For $n$ small this is fine.
- Outer algorithm cost: unchanged — $O(N_p)$ per experiment, $O(d \log 1/\delta)$ total experiments.

## Caveat

- Process tomography cost scales as $4^n$: not viable for large system-level characterisation. This trick works well for small interaction-block gates but not for full-system swaps on many qubits.
- If the noise is **time-varying** or **state-dependent**, a static superoperator is inaccurate. Works best for approximately Markovian, time-stationary noise.
- The extracted superoperator is itself subject to tomographic error; if process tomography is inaccurate, the likelihood model is wrong, and the algorithm may converge to an incorrect Hamiltonian.
- This is a **framework**, not a single trick: the actual implementation details depend heavily on hardware and available tomography primitives.

## Related notes
- [[Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]]
- [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]]
- [[Noise-Robust Bayesian Hamiltonian Learning]]
- [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation]]
- [[Sequential Monte Carlo for Quantum Parameter Estimation]]
