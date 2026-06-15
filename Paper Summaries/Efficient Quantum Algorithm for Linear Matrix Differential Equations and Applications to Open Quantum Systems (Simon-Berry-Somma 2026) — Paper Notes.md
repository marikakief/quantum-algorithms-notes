> **Source:** Sophia Simon, Dominic W. Berry, and Rolando D. Somma, *Efficient quantum algorithm for linear matrix differential equations and applications to open quantum systems*, arXiv:2605.16195 (2026)
> **Links:** [arXiv](https://arxiv.org/abs/2605.16195) · [PDF](https://arxiv.org/pdf/2605.16195)
> **Tags:** #quantum-algorithms #differential-equations #matrix-equations #block-encoding #open-quantum-systems #fermions

Maintenance note: this is based on a May 2026 arXiv v1, so bibliographic data, theorem numbering, and some constants may change in later versions.

---

## The computational problem

The paper studies the Sylvester-type linear matrix differential equation

$$
\frac{d}{dt}X(t) = A^\dagger X(t) + X(t)B + C,
\qquad X(0)=D,
$$

where $A,B,C,D,X(t) \in \mathbb{C}^{N\times N}$. For time-independent $A,B$, the solution is

$$
X(t)=\int_0^t ds\, e^{(t-s)A^\dagger} C e^{(t-s)B} + e^{tA^\dagger}D e^{tB}.
$$

For time-dependent matrices, the exponentials become time-ordered propagators.

The oracle model is [[Block-Encoding Composition Algebra|block-encoding]] access to $A/a$, $B/b$, $C/c$, and $D/d$, plus state-preparation unitaries $U_\phi|0\rangle=|\phi\rangle$ and $U_\psi|0\rangle=|\psi\rangle$. The task is not to output a normalized vector proportional to $X(t)$, but to estimate the matrix entry

$$
\langle \phi|X(t)|\psi\rangle
$$

to additive error $\epsilon$.

This is the same “don’t vectorize the matrix if the object is a matrix” philosophy as [[Quantum Linear Matrix Equations — Paper Notes]], but now for differential equations rather than static Sylvester equations.

## What the paper does

They give a nearly optimal quantum algorithm for estimating entries of $X(t)$ with query complexity roughly

$$
\widetilde{O}\!\left(\frac{cL}{\epsilon}\,t\mu\right),
\qquad \mu=\max\{a,b\},
$$

where $L$ measures the time-integrated growth/decay of the propagators. For dissipative dynamics, $L$ can be $O(1/|\xi|)+d/c$ rather than $O(t)$, so the dependence can be close to linear in the physical relaxation time. For unitary dynamics, $L=t+d/c$, giving quadratic-in-$t$ query scaling.

The main technical move is an exact reduction of the entry of the matrix solution to an overlap between two two-sided history states. That avoids the usual normalization/readout problem of vectorized quantum ODE solvers such as [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]], [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]], and [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]].

My assessment: this is a natural and useful continuation of the operator-output viewpoint in [[Quantum Linear Matrix Equations — Paper Notes]]. The lower bound is the part that makes it more than a construction: within this block-encoding/access/output model, the $Lt/\epsilon$ scaling is essentially tight. More structure, a different access model, or a narrower output task could still change the story.

## The algorithm / construction

### Step 1: keep the two-sided structure

Set $M\sim t\mu$ time steps and choose a padding length $R\sim \mu d_D/c$, where $d_D$ denotes the block-encoding normalization for $D$ rather than a spatial dimension. Define the two unnormalized history states

$$
|\phi_{\mathrm{hist}}\rangle
:=
\sum_{m=0}^{M-1}|m\rangle e^{tAm/M}|\phi\rangle
+
\sum_{m=M}^{M+R-1}|m\rangle e^{tA}|\phi\rangle,
$$

$$
|\psi_{\mathrm{hist}}\rangle
:=
\sum_{m=0}^{M-1}|m\rangle e^{tBm/M}|\psi\rangle
+
\sum_{m=M}^{M+R-1}|m\rangle e^{tB}|\psi\rangle.
$$

The extra $R$ clock values are not just output-padding in the usual [[History-State Padding for Final-Time Readout]] sense. Here they make the initial-condition term $e^{tA^\dagger}De^{tB}$ fit into the same overlap identity as the integral term.

### Step 2: turn the short-time integral into a clock-diagonal operator

Let

$$
I_C := \int_0^{t/M} d\tau\, e^{\tau A^\dagger} C e^{\tau B}.
$$

Then the integral in $X(t)$ decomposes exactly as

$$
\int_0^t ds\, e^{(t-s)A^\dagger}Ce^{(t-s)B}
=
\sum_{m=0}^{M-1} e^{tA^\dagger m/M} I_C e^{tBm/M}.
$$

Define the clock-diagonal operator

$$
I :=
\sum_{m=0}^{M-1}|m\rangle\!\langle m|\otimes I_C
+
\sum_{m=M}^{M+R-1}|m\rangle\!\langle m|\otimes \frac{D}{R}.
$$

Then

$$
\langle \phi|X(t)|\psi\rangle
=
\langle \phi_{\mathrm{hist}}|I|\psi_{\mathrm{hist}}\rangle.
$$

That identity is the spine of the algorithm. It is the differential-equation version of [[Sandwich LCU for Matrix Equations]], with the clock register deciding which short-time sandwich is being evaluated.

### Step 3: prepare the history states

They give two routes.

1. **LCHS route.** Shift $A$ and $B$ by log-norm upper bounds $\xi_A,\xi_B$ so that the shifted generators have non-positive log-norm, then use [[LCHS Kernel for Non-Unitary Dynamics]] to block-encode the controlled propagators

   $$
   \sum_m |m\rangle\!\langle m|\otimes e^{tm(Y-\xi_Y I)/M}
   $$

   and prepare the clock amplitudes with compensating weights $e^{tm\xi_Y/M}$. This gives the clean $\widetilde{O}((cL/\epsilon)t\mu)$-type bound.

2. **Linear-systems route.** Encode the history state as the solution of a discretized linear system, as in older quantum ODE algorithms. This handles cases where $\|e^{sA}\|$ or $\|e^{sB}\|$ are bounded even when the log-norm analysis is pessimistic, but it carries condition-number overheads.

### Step 4: block-encode $I$

The short-time integral $I_C$ is approximated by a two-sided Taylor expansion

$$
\widetilde{I}_C
=
\sum_{p,q=0}^{K}
\frac{(A^\dagger)^p C B^q}{p!q!}
\frac{h^{p+q+1}}{p+q+1},
\qquad h=t/M\leq 1/\mu.
$$

This is then implemented as a block-encoding using standard [[Block-Encoding Composition Algebra|block-encoding composition]], with one query to $U_C$ and $O(K)$ queries to $U_A^\dagger$ and $U_B$. The $D/R$ blocks are inserted on the padded clock sector.

### Step 5: estimate the overlap

Overlap/amplitude estimation of

$$
\langle \phi_{\mathrm{hist}}|I|\psi_{\mathrm{hist}}\rangle
$$

gives the desired entry after restoring the history-state normalizations and the block-encoding constant for $I$. The normalization factor is $O(cL)$ in the LCHS analysis, so achieving additive error $\epsilon$ in the final matrix entry needs overlap precision $O(\epsilon/(cL))$.

## Key results

### Theorem 1: simplified time-independent upper bound

Let $\mu=\max\{a,b\}$ and

$$
L := \max_{Y\in\{A,B\}}
\left(
\int_0^t ds\, e^{2s\xi_Y} + \frac{d}{c}e^{2t\xi_Y}
\right),
$$

where $\xi_Y$ is a known upper bound on the log-norm of $Y$. Then Problem 1 can be solved with high probability using

$$
\widetilde{O}\!\left(\frac{cL}{\epsilon}\,t\mu\right)
$$

queries to the block-encodings and their inverses. The gate complexity is larger by polylogarithmic factors.

Two regimes matter:

- If $\xi_Y<0$ (dissipative), then $L\leq 1/(2|\xi_Y|)+d/c$ after ignoring exponentially small tails.
- If $\xi_Y=0$ (unitary), then $L=t+d/c$, so the query complexity is quadratic in $t$.

### More general bounded-propagator result

If $\widetilde{L}$ upper-bounds

$$
\max_{Y\in\{A,B\}}
\left(
\int_0^t ds\,\|e^{sY}\| + \frac{d}{c}\max_{s\in[0,t]}\|e^{sY}\|
\right),
$$

then the linear-systems route gives query complexity

$$
\widetilde{O}\!\left(
\frac{c}{\epsilon}\,\mu\widetilde{L}^{2}
\max_{Y\in\{A,B\},\,s\in[0,t]}\|e^{sY}\|\log(t\mu)
\right).
$$

This can be better when the log-norm is positive but actual transient growth is controlled.

### Theorem 2: query lower bound

For diagonal $A\preceq 0$, $C=I$, $D=B=0$, and block-encoding access to $A$ with constant $a=1$, if $t\geq 6$ and $\epsilon\leq t/100$, then estimating $\langle 0|X(t)|0\rangle$ within additive error $\epsilon$ and success probability $p\in(3/4,1)$ requires

$$
\Omega\!\left(\frac{Lt}{\epsilon}|\log(1-p)|\right)
$$

uses of the block-encoding and its inverse, where

$$
L=\int_0^t ds\, e^{2s\xi_A}.
$$

The reduction is from a decision version of [[Gapped Phase Estimation|phase estimation]] / [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev-style eigenphase discrimination]]. The constructed instance uses

$$
A=-\sin\theta\,|0\rangle\!\langle 0| - \sum_{n>0}|n\rangle\!\langle n|,
$$

so

$$
\langle 0|X(t)|0\rangle
=\frac{1-e^{-2t\sin\theta}}{2\sin\theta},
$$

which carries enough information to distinguish $\theta=\delta$ from $\theta\geq 2\delta$.

## Application: dissipative free fermions

For a non-interacting fermion system coupled to a fermionic bath, the covariance matrix

$$
X_{jk}(t)=\langle c_j^\dagger c_k(t)\rangle
$$

obeys, under a Markovian approximation,

$$
\frac{d}{dt}X(t)=B^\dagger X(t)+X(t)B+C,
\qquad B=-iA-\Gamma,
$$

where $A$ is the system Hamiltonian matrix, $\Gamma\succeq 0$ models dissipation, and $C$ models bath noise. If the fixed point is the thermal covariance

$$
X_\beta=(I+e^{\beta A})^{-1},
$$

then the consistency condition is

$$
C=X_\beta\Gamma+\Gamma X_\beta.
$$

Assuming block-encodings of $A/a$ and $\Gamma/\gamma$, and using [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] to block-encode $X_\beta$, the paper obtains query cost roughly

$$
\widetilde{O}\!\left(\frac{a}{\gamma\epsilon}\log\frac{a}{\gamma}\right)
$$

for the $A$-dependent calls, plus

$$
\widetilde{O}\!\left(\frac{\beta a}{\epsilon}\log(\beta a)\right)
$$

for constructing the noise term $C$.

For local lattice systems in spatial dimension $D$, a light-cone estimate gives relevant size

$$
N_e\sim (at_e)^D,
\qquad t_e\sim \frac{1}{\gamma}+\beta.
$$

For this application, the authors argue that Krylov/kernel-polynomial classical methods cost at least linear in the relevant space-time volume,

$$
\Omega(at_eN_e)=\Omega((at_e)^{1+D}),
$$

so the quantum advantage is polynomial for geometrically local systems, e.g. quartic in $D=3$, and potentially exponential for non-geometrically-local interactions. The memory separation is cleaner: $O(\log N)$ quantum memory versus $\Omega(N)$ classical storage.

## Comparison with prior work

| Approach | Output model | Main obstruction | What this paper changes |
|---|---|---|---|
| Vectorized quantum ODE solvers ([[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]], [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]) | normalized state $|X(t)\rangle\rangle$ | entries can be exponentially small after normalization | estimates entries directly through two-sided overlaps |
| [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]] | state/output vector for linear ODE | still a state-normalization story | matrix structure is kept as left/right propagation |
| [[Quantum Linear Matrix Equations — Paper Notes]] | block-encoded solution of static Sylvester equation | static equation, not dynamics | extends the operator-native idea to matrix ODEs |
| Classical Krylov / kernel-polynomial methods | explicit vectors or matrix-vector products | at least linear in relevant space-time volume for lattice systems | quantum method can be polylogarithmic in $N$ under block-encoding access |

## Limits / caveats

- The oracle model is doing real work. Efficient block-encodings of $A$, $B$, $C$, $D$ and the input states are assumed.
- The result estimates entries or generalized entries. It does not output the whole matrix, and it does not magically remove tomography costs if many entries are needed.
- For geometrically local systems, the speedup argument is polynomial, not exponential, once locality is used on the classical side.
- The dissipative free-fermion application depends on Markovian reduction to a covariance-matrix Lyapunov equation. Strongly interacting open systems will not reduce this cleanly.
- The lower bound says the general $Lt/\epsilon$ dependence is tight in the block-encoding access model. Better algorithms need more structure, a different access model, or a narrower output task.

## Reusable ideas

1. [[Two-Sided History-State Overlap for Matrix ODE Entries]] — encode left and right propagators in separate history states and recover a matrix entry as an overlap.
2. [[Clock-Diagonal Short-Integral Block Encoding]] — replace the full Duhamel integral by repeated clock-diagonal short-window sandwiches.
3. [[Log-Norm Shifted LCHS History States]] — shift by a log-norm bound and compensate in the clock amplitudes to prepare non-unitary history states.
4. [[Terminal Padding as an Initial-Condition Term]] — use padded final-time clock sectors to insert $D/R$ into the same overlap identity as the inhomogeneous integral.
5. [[QPE Lower Bound via Scalar Dissipative Matrix ODE]] — reduce phase-estimation discrimination to estimating one scalar entry of a dissipative matrix ODE.

## References within this paper

- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Berry, Childs, Cleve, Kothari & Somma (2015)]] — block-encoding and Hamiltonian simulation background.
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low & Chuang (2017)]] — [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSP/QSVT]] lineage.
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry, Childs, Ostrander & Wang (2017)]] — history-state quantum ODE algorithm.
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes|Berry & Costa (2022)]] — time-dependent ODE solver via Dyson series.
- [[Linear Combination of Hamiltonian Simulation for Non-Unitary Dynamics (An-Liu-Lin 2023) — Paper Notes|An, Liu & Lin (2023)]] — [[LCHS Kernel for Non-Unitary Dynamics|LCHS]] machinery for non-unitary propagators.
- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes|An, Childs & Lin (2023)]] — near-optimal LCHS-style non-unitary dynamics.
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]] — block-encoding composition and matrix transformations.
- [[Quantum Linear Matrix Equations — Paper Notes|Somma, Low, Berry & Babbush (2025)]] — static matrix-equation predecessor.
- Saad (1992/2003) and kernel-polynomial-method references — classical Krylov/KPM comparison for matrix functions and dynamics.

## Cross-links

### Paper notes

- [[Quantum Linear Matrix Equations — Paper Notes]]
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]]
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]]
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes]]
- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes]]
- [[Linear Combination of Hamiltonian Simulation for Non-Unitary Dynamics (An-Liu-Lin 2023) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]

### Trick cards

- [[Two-Sided History-State Overlap for Matrix ODE Entries]]
- [[Clock-Diagonal Short-Integral Block Encoding]]
- [[Log-Norm Shifted LCHS History States]]
- [[Terminal Padding as an Initial-Condition Term]]
- [[QPE Lower Bound via Scalar Dissipative Matrix ODE]]
- [[Sandwich LCU for Matrix Equations]]
- [[LCHS Kernel for Non-Unitary Dynamics]]
- [[History-State Padding for Final-Time Readout]]
- [[Block-Encoding Composition Algebra]]
