> **Source:** Guang Hao Low and Nathan Wiebe, *Hamiltonian Simulation in the Interaction Picture*, arXiv:1805.00675  
> **Links:** [arXiv](https://arxiv.org/abs/1805.00675)  
> **Tags:** #dyson-series #interaction-picture #LCU #hamiltonian-simulation

---

## What the paper does

Applies the truncated-Dyson-series machinery of Kieferová–Scherer–Berry (1805.00582) to time-*independent* Hamiltonians by moving to the interaction picture.

If $H = A + B$, and the free evolution $e^{-iAt}$ is cheap, then simulate the interaction-picture Hamiltonian
$$
H_I(t) = e^{iAt} B e^{-iAt}
$$
using the time-dependent Dyson algorithm. The complexity ends up tracking $\|B\|$ (and its derivatives in the interaction frame) rather than $\|A + B\|$.

## When this helps

Two conditions must both hold:

1. **Norm separation**: $\|A\| \gg \|B\|$ (the large part is in $A$).
2. **Cheap frame evolution**: $e^{\pm iAt}$ can be implemented efficiently (e.g., it is fast-forwardable). If simulating $A$ for time $\sim \|B\|^{-1}$ costs as much as simulating $A+B$ directly, the advantage collapses.

The second condition rules out generic implementations of $A$. Favorable cases include diagonally dominant Hamiltonians (where $e^{iAt}$ is diagonal and trivially fast-forwardable) and models with a kinetic term computable via FFT.

## How the oracle is built

The Dyson algorithm needs a block-encoding of $H_I(t) = e^{iAt}Be^{-iAt}$ at each sampled time. This is constructed by conjugating the static oracle $O_B$ for $B$ with controlled evolutions under $A$:

$$
O_{H_I}(t) = e^{iAt} \cdot O_B \cdot e^{-iAt}.
$$

The cost per query to $O_{H_I}$ therefore includes one application of $e^{\pm iAt}$, which must be cheap enough not to dominate.

## Complexity

Let $\alpha_A \ge \|A\|$, $\alpha_B \ge \|B\|$, $C_B$ = cost of one query to $O_B$, $C_{e^{iA/\alpha_B}}$ = cost of implementing $e^{iAt}$ for $t \sim \alpha_B^{-1}$. Then the gate complexity of interaction-picture simulation is roughly

$$
\widetilde{O}\!\left(\alpha_B t \left(C_B + C_{e^{iA/\alpha_B}}\log\frac{t\,\alpha_A}{\varepsilon}\right)\frac{\log(\alpha_B t/\varepsilon)}{\log\log(\alpha_B t/\varepsilon)}\right).
$$

Compare with Schrödinger-picture simulation cost $\widetilde{O}((\alpha_A + \alpha_B)t \cdot C \cdot \log(\cdots))$. The factor $\alpha_A + \alpha_B$ is replaced by $\alpha_B$, which is the gain when $\alpha_A \gg \alpha_B$ and $C_{e^{iA/\alpha_B}}$ is small.

For diagonally dominant Hamiltonians (A = diagonal, e^{iAt} is trivial), this gives **exponential** improvement in gate complexity — the large diagonal part is free.

## Concrete applications

| Problem | Schrödinger cost | Interaction-picture cost |
|---|---|---|
| Hubbard (arbitrary long-range, $N$ sites) | $\widetilde{O}(N^2(α_T + N^2)t)$ | $\widetilde{O}(N^2 α_B t)$ (often $\alpha_B \ll \alpha_A$) |
| Plane-wave quantum chemistry ($N$ orbitals) | $\widetilde{O}(N^{11/3} t)$ | $\widetilde{O}(N^2 t)$ |
| Diagonally dominant | $\widetilde{O}(α \cdot t)$ | $\widetilde{O}(\alpha_B t)$ (exponential gain when $\alpha_B \ll \alpha_A$) |

## Also in this paper

There is a separate result on **sparse time-dependent Hamiltonians**: the paper shows a quadratic improvement in query complexity over the direct Dyson-series approach (1805.00582) when the Hamiltonian is sparse and time-dependent. This is independent of the interaction-picture construction.

## Caveats

- The algorithm requires a low-space-overhead implementation of the Dyson series (the paper claims this explicitly in the abstract). The space overhead is $O(K)$ ancilla registers rather than proportional to segment count.
- "Exponential improvement" for diagonally dominant Hamiltonians is real, but only when $e^{iAt}$ can genuinely be fast-forwarded.
- The interaction picture does not help if the large term $A$ is not fast-forwardable — this is a decomposition-dependent, not generic, result.

## References within this paper

- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová, Scherer & Berry (2019)]] — Dyson series simulation without interaction picture; this paper adds the $H = A + B$ splitting
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs & Wiebe (2012)]] — [[Linear Combination of Unitaries (LCU)|LCU]] framework used to implement the Dyson expansion
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low & Chuang (2017)]] — QSP (optimal black-box scaling comparison)
- [[Oblivious Amplitude Amplification (Robust)]] — used to boost success probability of the LCU implementation
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard, Høyer, Mosca & Tapp (2002)]] — [[Standard Amplitude Amplification|amplitude amplification]] and [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]]

---

## Cross-links

- [[Interaction-Picture Norm Reduction]]
- [[Oracle Conjugation for Time-Dependent Encodings]]
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
- [[Linear Combination of Unitaries (LCU)]]
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Oblivious Amplitude Amplification (Robust)]]
- [[Hamiltonian Simulation — Comparison Tables]]
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes]] — applies similar $H = A + B$ splitting philosophy to linear systems and matrix functions rather than simulation
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]] — uses this paper's Dyson block-encoding for time-dependent ODEs; combines with history-state linear system encoding + optimal QLSA
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes]] — applies time reparameterization to convert $L^\infty$ scaling of this paper's Dyson algorithm to $L^1$ (integrated norm); the rescaled-Dyson approach directly builds on the KSB/Low-Wiebe Dyson framework
