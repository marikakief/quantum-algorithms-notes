> **Source:** Mária Kieferová, Nathan Wiebe, *On The Power Of Coherently Controlled Quantum Adiabatic Evolutions*, New Journal of Physics **16**, 123034 (2014)
> **Links:** [arXiv:1403.6545](https://arxiv.org/abs/1403.6545) · [NJP](https://iopscience.iop.org/article/10.1088/1367-2630/16/12/123034)
> **Tags:** #adiabatic #state-preparation #coherent-control #diabatic-error #LCU #hamiltonian-simulation

---

## The computational problem

Given a Hamiltonian path $H(s)$ for $s \in [0,1]$ with ground state $|g(0)\rangle$ easily prepared, prepare $|g(1)\rangle$ to error $\varepsilon$. The obstacle is diabatic error: for finite evolution time $T$, the state leaks into excited states at rate $O(1/T)$.

**Context:** This is not about finding $|g(1)\rangle$ faster in a complexity-theoretic sense — it's about suppressing diabatic error more efficiently than conventional schedules or boundary-cancellation alone.

---

## What the paper does

Uses a small coherently-controlled ancilla to run two adiabatic evolutions on different paths $H(f_A(s))$ and $H(f_B(s))$ in superposition, then measures the ancilla. Conditional on the success outcome, the system has undergone a weighted average $\cos^2\!\theta\, U_A + \sin^2\!\theta\, U_B$. By designing $f_B$ and choosing $\theta$, the two leading-order ($O(1/T)$) diabatic amplitudes cancel, leaving $O(1/T^2)$ residual error — without requiring longer evolution time. The approach is shown to be polynomially equivalent to the circuit model, so it offers no super-polynomial advantage, but it does give real practical gains in the regime where $T$ is finite and diabatic errors are the bottleneck.

---

## The algorithm / construction

### Core gadget: probabilistic LCU for adiabatic evolutions

Prepare ancilla in $\cos\theta|0\rangle + \sin\theta|1\rangle$, apply $\text{ctrl-}U_A$ (ancilla $|0\rangle$) and $\text{ctrl-}U_B$ (ancilla $|1\rangle$), rotate ancilla back, measure. Outcome $|0\rangle$ (succeeds with probability $p_0$) implements:

$$|\psi'\rangle \propto (\cos^2\!\theta\, U_A + \sin^2\!\theta\, U_B)|\psi\rangle$$

Failure probability is bounded: $p_0 \geq 1 - \|(U_A - U_B)|\psi\rangle\|^2$. In the adiabatic regime, $U_A$ and $U_B$ agree on the ground state to leading order, so $p_0 = 1 - O(1/T^2)$. Failures are heralded; repeat-until-success is practical.

This is the [[Linear Combination of Unitaries (LCU)]] gadget of [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs-Wiebe (2012)]], specialised to adiabatic evolutions.

### Why errors cancel: the first-order adiabatic expansion

The key formula (from [[Improved Error Bounds for the Adiabatic Approximation (Cheung-Høyer-Wiebe 2011) — Paper Notes|Cheung-Høyer-Wiebe 2011]], eq. (9)):

$$(\mathbb{I} - |g(1)\rangle\!\langle g(1)|)\, U(1,0)|g(0)\rangle = \sum_{n \neq g} e^{-i\int_0^1 E_n T\,d\xi} \left[\frac{\langle\dot{n}(s)|g(s)\rangle\, e^{i\int_0^s \gamma_{g,n}(\xi)T\,d\xi}}{-i\gamma_{g,n}(s)T}\right]_{s=0}^{s=1} |n(1)\rangle + O(1/T^2)$$

where $\gamma_{g,n}(s) = E_g(s) - E_n(s)$ is the gap. The leading $O(1/T)$ error depends only on **boundary terms** at $s=0$ and $s=1$ (see [[Adiabatic Approximation Boundary-Term Dominance]]). The strategy is to choose $f_A$, $f_B$, $\theta$, $T_A$, $T_B$ so that:

$$\cos^2\!\theta \cdot [\text{boundary terms of } U_A] + \sin^2\!\theta \cdot [\text{boundary terms of } U_B] = 0$$

for the relevant transition(s), leaving $O(1/T^2)$.

### Construction 1: Partially antisymmetric path pair (Section III.A)

Choose $f_A$, $f_B$ with matching derivative at $s=0$ but opposite sign at $s=1$:

$$\dot{f}_B(0) = \dot{f}_A(0), \qquad \dot{f}_B(1) = -\dot{f}_A(1)$$

Then the cancellation condition reduces to choosing $T_B$ and $\theta$:

$$T_B = \frac{\int_0^1 \gamma_{A,g,e}(\xi)\,d\xi}{\int_0^1 \gamma_{B,g,e}(\xi)\,d\xi}\,T_A + \frac{(2n+1)\pi}{\int_0^1 \gamma_{B,g,e}(\xi)\,d\xi}, \qquad \theta = \arctan\!\sqrt{T_B/T_A}$$

$f_B$ must be non-monotone near $s=1$ (overshoots past 1, then returns) to flip the derivative sign. Build $f_B$ via quartic polynomial interpolation near $s = 1-\Delta$:

$$f_B(s) = \begin{cases} f_A(s) & s < 1-\Delta \\ es^4 + ds^3 + cs^2 + bs + a & s \geq 1-\Delta \end{cases}$$

Coefficients match value and two derivatives at the splice point with $\dot{f}_B(1) < 0$.

### Construction 2: Completely antisymmetric path pair (Section III.B)

Flip derivatives at **both** boundaries:

$$\dot{f}_B(0) = -\dot{f}_A(0), \qquad \dot{f}_B(1) = -\dot{f}_A(1)$$

Choose $T_B$ so gap integrals match modulo $2\pi$ (eq. (28)). With $\theta = \pi/4$ and $T_A = T_B$ this requires matching $\int_0^1 \gamma_{A,g,e}\,ds = \int_0^1 \gamma_{B,g,e}\,ds \pmod{2\pi}$. $f_B$ must overshoot near both $s=0$ and $s=1$ — two polynomial splices needed.

### Global cancellation for symmetric-spectrum Hamiltonians (Section V)

When the eigenvalue gaps are symmetric about $s=1/2$, i.e. $\gamma_{g,n}(s) = \gamma_{g,n}(1-s)$, choose $f_B(s) = 1 - f_A(1-s)$ (time-reverse pair). Then:
- $\int_0^1 \gamma_{g,n}(f_A(s))\,ds = \int_0^1 \gamma_{g,n}(f_B(s))\,ds$ for all $n$
- Boundary derivatives satisfy the antisymmetry conditions

With $\theta = \pi/4$, $T_A = T_B$, the weighted average cancels $O(1/T)$ error for **every** excited state simultaneously. This is stronger than prior methods (Wiebe-Babcock) which only cancel errors at discrete special times.

### Combining with boundary cancellation (Section VI)

If the first $m$ derivatives of $H$ vanish at boundaries, leading error becomes $O(1/T^{m+1})$ (standard boundary-cancellation). The coherent averaging approach extends naturally: replace $\dot{f}$ with the $(m+1)$-th derivative boundary term and use higher-order polynomial splices. You get $O(1/T^{m+2})$ residual, combining both improvements.

---

## Key results

**Corollary 1** (query complexity for time-dependent simulation, citing [34]):

Let the Hamiltonians be $\Lambda$–$2k$–smooth with $L$ non-smooth points. The number of queries to simulate within error $\varepsilon$ over interval $\Delta t$ satisfies:

$$N_{\text{queries}} \leq 12\, C\, M\, d^2\, 5^{k-1} \left[(L+1) + 24k\,d^2\,\Lambda\,\Delta t\,\left(\tfrac{5}{3}\right)^k \left(\frac{6d^2\Lambda\Delta t}{\varepsilon/3}\right)^{1/(2k)}\right]$$

**Theorem 2** (simulation of the controlled adiabatic gadget):

Same bound with $\Delta t \to \max_j T_j$. Cost $C$ of simulating a one-sparse Hamiltonian:

$$C \leq 4n(z_n + 2) + 3n_H + 2\left\lceil\log_2\!\left(\frac{6\Gamma\max_j T_j}{\varepsilon}\right)\right\rceil$$

**Corollary 3** (polynomial equivalence): If $H(f_j(s))$ is $d$-sparse and row-computable, and $f_j$ efficiently computable, then coherently controlled adiabatic evolutions are polynomially equivalent to the circuit model — no exponential extra power.

**Failure probability**: $p_0 \geq 1 - \|(U_A - U_B)|g(0)\rangle\|^2 = 1 - O(1/T^2)$ in the adiabatic regime.

---

## Comparison with prior work

| Method | Error scaling | Key mechanism | Limitation |
|---|---|---|---|
| Standard AQC | $O(1/T)$ | — | Baseline |
| [[Local Adiabatic Schedule via Gap-Dependent Evolution Rate\|Local Adiabatic (Roland-Cerf)]] | $O(1/T)$ but optimal $T$ | Adapt schedule to gap | Pushes fast near endpoints, conflict with BC |
| Boundary Cancellation (Lidar et al., Wiebe-Babcock) | $O(1/T^{m+1})$ | Set $m$ derivatives to 0 at boundary | Only works at special times; conflicts with LAE |
| This paper | $O(1/T^2)$ generally, $O(1/T^{m+2})$ with BC | Coherent path averaging | Non-monotone path required; success probability |
| Symmetric case (Section V) | $O(1/T^2)$ for **all** transitions | Time-reverse path pairing | Requires symmetric gap profile |

---

## Limits / caveats

- The cancellation is designed for one specific transition at a time (partially antisymmetric case) or requires spectral symmetry for universal cancellation.
- $f_B$ must be non-monotone. Near an avoided crossing this can be problematic if the Hamiltonian behaves badly off the monotone path.
- The polynomial equivalence result (Corollary 3) means no exponential quantum speedup from the coherent control structure itself — the advantage is purely in the constant / polynomial prefactors of the diabatic error.
- The first-order adiabatic expansion requires $H$ twice-differentiable and the system to already be in the adiabatic regime ($O(1/T) \gg O(1/T^2)$). Near a phase transition where the gap closes, $T \to \infty$ and the whole analysis breaks.
- The cost metric (eq. (35): $\max_j \{\int_0^1\|H_j(s)\|\,ds \cdot T_j\} / P(0)$) accounts for both time and Hamiltonian norm, so the improvement in error scaling must be weighed against the cost of the second path.

---

## Reusable ideas

1. **[[Coherent Averaging of Adiabatic Paths for Diabatic Error Cancellation]]** — run two adiabatic evolutions in superposition via LCU gadget, measure ancilla; weighted average cancels leading $O(1/T)$ diabatic errors. Requires designing the second path.

2. **[[Antisymmetric Path Design for Adiabatic Error Cancellation]]** — construct a second adiabatic path by polynomial-splicing near boundaries to flip derivative sign(s); combined with timing choice, cancels first-order error for a chosen transition.

3. **[[Time-Reverse Path Pairing for Global Diabatic Error Suppression]]** — when gap is symmetric under $s \leftrightarrow 1-s$, pair $f_A$ with its time-reverse $f_B(s) = 1 - f_A(1-s)$; with equal weights and times, cancels $O(1/T)$ error for **every** excited state at once.

---

## References within this paper

- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs-Wiebe (2012)]] — the [[Linear Combination of Unitaries (LCU)]] gadget used as the core combination mechanism
- [[Improved Error Bounds for the Adiabatic Approximation (Cheung-Høyer-Wiebe 2011) — Paper Notes|Cheung-Høyer-Wiebe (2011)]] — the first-order adiabatic expansion (eq. 9) used throughout
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes|Roland-Cerf (2002)]] — local adiabatic evolution; the comparison baseline
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes|Aharonov et al. (2004)]] — used in polynomial-equivalence argument
- Wiebe & Babcock (2012) — boundary-cancellation interference method; the direct predecessor; no vault note yet
- Lidar, Rezakhani, Hamma — boundary-cancellation via smooth schedules; no vault note yet
- Itay Hen — controlled adiabatic circuits; related model; no vault note
- [34] (Wiebe-Berry-Høyer-Sanders) — used for Corollary 1, the Trotter-Suzuki simulation cost bound → [[Simulating Quantum Dynamics on a Quantum Computer (Wiebe-Berry-Høyer-Sanders 2011) — Paper Notes]]

---

## Cross-links

### Paper notes
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Improved Error Bounds for the Adiabatic Approximation (Cheung-Høyer-Wiebe 2011) — Paper Notes]]
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes]]
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[Simulating Quantum Dynamics on a Quantum Computer (Wiebe-Berry-Høyer-Sanders 2011) — Paper Notes]]
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]]

### Trick cards
- [[Coherent Averaging of Adiabatic Paths for Diabatic Error Cancellation]]
- [[Antisymmetric Path Design for Adiabatic Error Cancellation]]
- [[Time-Reverse Path Pairing for Global Diabatic Error Suppression]]
- [[Linear Combination of Unitaries (LCU)]]
- [[Adiabatic Approximation Boundary-Term Dominance]]
- [[Local Adiabatic Schedule via Gap-Dependent Evolution Rate]]
- [[Superadiabatic Smooth Scheduling for Exponential Precision]]
