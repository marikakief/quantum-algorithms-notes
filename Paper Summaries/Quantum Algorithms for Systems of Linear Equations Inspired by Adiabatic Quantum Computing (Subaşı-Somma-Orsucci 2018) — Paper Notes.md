> **Source:** Yiğit Subaşı, Rolando D. Somma, Davide Orsucci, *Quantum algorithms for systems of linear equations inspired by adiabatic quantum computing*, arXiv:1805.10549, Phys. Rev. Lett. **122**, 060504 (2019)
> **Links:** [arXiv](https://arxiv.org/abs/1805.10549) · [PRL](https://doi.org/10.1103/PhysRevLett.122.060504)
> **Tags:** #QLSA #adiabatic #randomization-method #linear-systems #spectral-gap-amplification

---

## The computational problem

The paper studies the **quantum linear systems problem (QLSP)**: given a Hermitian matrix $A \in \mathbb{C}^{N\times N}$ with $\|A\|=1$ and condition number $\kappa$, and a procedure that prepares $|b\rangle$, prepare the normalized solution state

$$
|x\rangle = \frac{A^{-1}|b\rangle}{\|A^{-1}|b\rangle\|}
$$

to error at most $\varepsilon$.

The point is not another HHL-style phase-estimation algorithm. The point is to show that an **adiabatic / Hamiltonian-based route** can solve QLSP with near-linear dependence on $\kappa$, without phase estimation, variable-time amplitude amplification, or a large ancilla workspace.

## What the paper does

This is the first clean adiabatic-style QLSA that is actually competitive asymptotically. The authors give two algorithms based on the [[Randomization Method for Eigenpath Traversal]]: one with complexity $O(\kappa^2 \log \kappa / \varepsilon)$, and a better one using [[Spectral Gap Amplification]] with complexity $O(\kappa \log \kappa / \varepsilon)$.

My read: the paper matters mostly because it opened the adiabatic QLSP line that later runs through [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes|An-Lin (2019)]] and [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|Costa et al. (2021)]]. The final scaling here is not optimal, but the construction is conceptually much cleaner than HHL.

## The algorithm / construction

### 1. Linear-system state as a null-space vector

Let

$$
A(s) = (1-s) Z \otimes I + s X \otimes A,
$$

with an ancilla qubit, and let

$$
|\bar b\rangle = |+\rangle \otimes |b\rangle,
\qquad
P_{\bar b}^{\perp} = I - |\bar b\rangle\langle \bar b|.
$$

The first family of Hamiltonians is

$$
H(s) = A(s) P_{\bar b}^{\perp} A(s).
$$

For every $s \in [0,1]$, the state

$$
|x(s)\rangle \propto A(s)^{-1}|\bar b\rangle
$$

is a zero-eigenvalue state of $H(s)$. At $s=0$, this state is easy to prepare. At $s=1$, it encodes the desired solution $|x\rangle$ (up to a known ancilla factor). So the problem becomes one of following a zero-energy eigenpath.

### 2. Randomized eigenpath traversal

Instead of continuous adiabatic evolution, the paper uses the [[Randomization Method for Eigenpath Traversal]]: discretize the path $s_0,\dots,s_q$, and at each step evolve for a random time under the current Hamiltonian. Randomization damps components orthogonal to the target eigenspace, mimicking a projective measurement onto the instantaneous ground space.

The cost is governed by the inverse minimum spectral gap along the path and by the path length

$$
L = \int_0^1 \|\partial_s |x(s)\rangle\| ds.
$$

For the first Hamiltonian family, the gap scales as $\Delta(s)=\Omega((1-s)^2 + (s/\kappa)^2)$, which is the source of the $\kappa^2$ dependence.

### 3. Gap-amplified Hamiltonian family

To improve the scaling, the authors replace $H(s)$ by a linear Hamiltonian whose nonzero spectrum is the square root of the old one. Define

$$
H'(s) = \sigma^+ \otimes A(s) P_{\bar b}^{\perp} + \sigma^- \otimes P_{\bar b}^{\perp} A(s),
$$

on one extra ancilla qubit. The same target path sits in the null space, but now the gap is amplified from $\Delta$ to roughly $\sqrt{\Delta}$.

This changes the minimum gap from order $1/\kappa^2$ to order $1/\kappa$, giving the better runtime.

### 4. Gate-model implementation

The algorithm is described in Hamiltonian language, but the paper is explicit about implementing it in the circuit model via [[Hamiltonian simulation]]. Each randomized step uses simulation of either $H(s)$ or $H'(s)$ for a randomly drawn time. The resulting query/gate complexity inherits the same $\kappa$ scaling up to polylog factors.

## Key results

For the better, gap-amplified algorithm, the main complexity statement is

$$
T = O\!\left(\frac{\kappa \log \kappa}{\varepsilon}\right),
$$

where $T$ is the total evolution time / Hamiltonian-simulation cost needed to prepare a state within trace-distance error $\varepsilon$ of the target solution state.

For the unamplified version,

$$
T = O\!\left(\frac{\kappa^2 \log \kappa}{\varepsilon}\right).
$$

More structurally:

- the eigenpath length is $L = O(\log \kappa)$,
- the minimum gap is $\Delta = \Omega(1/\kappa^2)$ for $H(s)$,
- the minimum gap becomes $\Delta' = \Omega(1/\kappa)$ for $H'(s)$.

So the runtime is basically “path length divided by gap”, times the usual $1/\varepsilon$ cost of randomized projection.

## Comparison with prior work

| Paper | Method | QLSP scaling | What is different |
|---|---|---:|---|
| [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes\|HHL (2009)]] | Phase estimation + eigenvalue inversion | $\tilde O(\kappa^2/\varepsilon)$ | Original circuit-model QLSA, but heavier machinery |
| [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes\|Childs-Kothari-Somma (2015)]] | LCU / polynomial approximation | $\tilde O(\kappa\,\mathrm{polylog}(1/\varepsilon))$ | Better asymptotics, less Hamiltonian intuition |
| **Subaşı-Somma-Orsucci (2018/2019)** | Randomized eigenpath traversal | $O(\kappa \log\kappa/\varepsilon)$ | First competitive adiabatic-style QLSA |
| [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes\|An-Lin (2019)]] | Time-optimal adiabatic schedule | $O(\kappa/\varepsilon)$ or $O(\kappa\,\mathrm{polylog}(\kappa/\varepsilon))$ | Refines this line by choosing a better schedule |
| [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes\|Costa et al. (2021)]] | Discrete adiabatic walk + filtering | $O(\kappa \log(1/\varepsilon))$ | Removes the leftover $\log\kappa$ and $1/\varepsilon$ penalties |

The honest assessment: this paper is mostly a bridge. It is not the end of the QLSA story, but it changes the story by showing that the adiabatic route is real rather than decorative.

## Limits / caveats

1. **Precision dependence is still linear in $1/\varepsilon$.** Later adiabatic and filtering papers do much better.
2. **The $\log\kappa$ factor survives.** That gets removed only in later work.
3. **The speedup still depends on the usual QLSA assumptions.** You need efficient access to $A$ and efficient preparation of $|b\rangle$, and the output is a quantum state, not a classical vector.
4. **The construction is mostly for Hermitian $A$.** Extensions are possible, but the clean formulation is Hermitian.

## Reusable ideas

1. [[Randomization Method for Eigenpath Traversal]]
2. [[Spectral Gap Amplification]]

## References within this paper

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow, Hassidim, Lloyd (2009)]] — the starting point for QLSA.
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs, Kothari, Somma (2015)]] — polynomial-method QLSA with better asymptotics.
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes|Roland-Cerf (2002)]] — conceptual ancestor for gap-sensitive adiabatic scheduling.
- Boixo, Knill, Somma (2009) — introduces the randomization method for eigenpath traversal.
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes|Somma-Boixo (2013)]] — gap amplification framework used directly here.
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes|Aharonov et al. (2004)]] — broader adiabatic-computation context.
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes|An-Lin (2019)]] — direct follow-up that tightens the schedule.
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|Costa et al. (2021)]] — later optimal version of this line.

## Cross-links

### Paper notes
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]]
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes]]
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes]]
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes]]
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]]
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]

### Trick cards
- [[Randomization Method for Eigenpath Traversal]]
- [[Spectral Gap Amplification]]
