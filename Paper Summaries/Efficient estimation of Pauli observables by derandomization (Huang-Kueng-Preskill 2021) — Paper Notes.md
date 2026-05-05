> **Source:** Hsin-Yuan Huang, Richard Kueng, John Preskill, *Efficient estimation of Pauli observables by derandomization*, arXiv:2103.07510, Physical Review Letters **127**, 030503 (2021)
> **Links:** [arXiv](https://arxiv.org/abs/2103.07510) · [PRL](https://doi.org/10.1103/PhysRevLett.127.030503)
> **Tags:** #classical-shadows #measurement #pauli-measurements #vqe #derandomization

---

## The computational problem

Given repeated access to an unknown $n$-qubit state $\rho$ and a fixed list of $L$ Pauli observables

$$O_{o_1},\ldots,O_{o_L},$$

estimate all expectation values

$$\omega_\ell(\rho)=\operatorname{tr}(O_{o_\ell}\rho)$$

to additive error $\varepsilon$ with success probability at least $1-\delta$, using as few copies of $\rho$ as possible.

Each measurement round chooses a full-weight Pauli basis string

$$p_m\in\{X,Y,Z\}^n,$$

measures all qubits once, and can be reused for any target Pauli string $o_\ell\in\{I,X,Y,Z\}^n$ that it **hits**, meaning $o_\ell$ is obtained from $p_m$ by replacing some local Paulis with identities. If $h(o_\ell;P)$ counts how many measurement settings in $P=[p_1,\ldots,p_M]$ hit $o_\ell$, then the natural estimator is the empirical average over those hits.

The whole design problem is therefore: choose the measurement settings $P$ so that every target observable gets hit often enough, without wasting shots on incompatible strings.

## What the paper does

It takes the local-Pauli version of [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes|classical shadows]] and removes the dumbest part: the basis choices do not have to stay random.

The paper defines a global confidence bound for simultaneous estimation of many Pauli observables, then derandomizes the usual random-basis protocol one single-qubit choice at a time using the method of conditional expectations. The result is a deterministic measurement schedule that is guaranteed to do at least as well as the average randomized protocol, and often much better when the observable list mixes many low-weight terms with a few awkward high-weight ones.

My assessment: this is not a new asymptotic theorem in the deepest sense — the randomized baseline was already known — but it is a very good piece of measurement design. For VQE-style Hamiltonians it fixes a real practical weakness of plain local shadows.

## The algorithm / construction

### Estimator and confidence bound

For each target Pauli string $o_\ell$, define

$$
\hat\omega_\ell=
\frac{1}{h(o_\ell;P)}
\sum_{m:o_\ell\triangleright p_m}
\prod_{j:o_\ell[j]\neq I} q_m[j],
$$

where $q_m[j]\in\{\pm1\}$ is the measurement outcome on qubit $j$ in shot $m$.

The paper packages the simultaneous failure probability into

$$
\mathrm{Conf}_\varepsilon(O;P)=\sum_{\ell=1}^{L}\exp\!\left(-\frac{\varepsilon^2}{2}h(o_\ell;P)\right).
$$

If

$$
\mathrm{Conf}_\varepsilon(O;P)\le \frac{\delta}{2},
$$

then all $L$ estimates are $\varepsilon$-accurate with probability at least $1-\delta$.

### Randomized baseline

If each qubit basis in each shot is chosen uniformly from $\{X,Y,Z\}$, then a weight-$w(o_\ell)$ target is hit with probability $3^{-w(o_\ell)}$, so

$$
\mathbb E_P\big[h(o_\ell;P)\big]=\frac{M}{3^{w(o_\ell)}}.
$$

This yields

$$
\mathbb E_P\big[\mathrm{Conf}_\varepsilon(O;P)\big]
=
\sum_{\ell=1}^L\left(1-\nu 3^{-w(o_\ell)}\right)^M,
\qquad \nu=1-e^{-\varepsilon^2/2}.
$$

So low-weight Pauli families are easy, but a single global string is effectively invisible to purely random local measurements.

### Derandomization by conditional expectation

The key move is to treat the $n\times M$ table of single-qubit basis choices as variables that can be fixed greedily. Suppose some entries have already been set, giving a partially deterministic schedule $P^\sharp$. Then the paper computes the conditional expectation of the confidence bound over all remaining random entries:

$$
\mathbb E_P\big[\mathrm{Conf}_\varepsilon(O;P)\mid P^\sharp\big].
$$

More explicitly, when the first $m-1$ measurement strings are fixed and the first $k$ qubits of the $m$-th shot are fixed,

$$
\mathbb E_P[\mathrm{Conf}_\varepsilon(O;P)\mid P^\sharp]
=
\sum_{\ell=1}^L
\exp\!\left(-\frac{\varepsilon^2}{2}\sum_{m'=1}^{m-1}\prod_{k'=1}^{n}\mathbf 1\{o_\ell[k']\triangleright P^\sharp[k',m']\}\right)
$$

$$
\times
\left(1-\nu\prod_{k'=1}^{k}\mathbf 1\{o_\ell[k']\triangleright P^\sharp[k',m]\}\,3^{-w_{\neg k}(o_\ell)}\right)
\left(1-\nu 3^{-w(o_\ell)}\right)^{M-m}.
$$

At each position $(k,m)$, the algorithm evaluates this score for the three candidate assignments $W\in\{X,Y,Z\}$ and chooses

$$
P^\sharp[k,m]=\arg\min_{W\in\{X,Y,Z\}}
\mathbb E_P\big[\mathrm{Conf}_\varepsilon(O;P)\mid P^\sharp, P[k,m]=W\big].
$$

This is just the method of conditional expectations in a measurement-design costume.

### What the greedy rule is really doing

- For many low-weight terms, it behaves like a structured version of randomized local shadows.
- For a few incompatible global strings, it rediscovers the obvious thing: measure those strings directly and alternate between them.
- For chemistry Hamiltonians, it allocates basis choices to overlap with many Pauli terms at once instead of letting rare high-weight terms dominate the shot count.

## Key results

### Lemma 1: confidence bound implies simultaneous accuracy

If

$$
\mathrm{Conf}_\varepsilon(O;P)\le \frac{\delta}{2},
$$

then the empirical estimators satisfy

$$
|\hat\omega_\ell-\omega_\ell(\rho)|\le \varepsilon
\qquad\text{for all }1\le \ell\le L
$$

with probability at least $1-\delta$.

The proof is just Hoeffding plus a union bound, but it gives the paper the right objective function to optimize.

### Theorem 1: randomized local Pauli measurements

Randomized Pauli measurements estimate all $L$ target Pauli expectations up to additive error $\varepsilon$ provided

$$
M\propto \frac{\log L}{\varepsilon^2}\max_\ell 3^{w(o_\ell)}.
$$

A more explicit sufficient budget appearing in Appendix A is

$$
M = \frac{4\log(2L/\delta)}{\varepsilon^2}\max_\ell 3^{w(o_\ell)}.
$$

So for bounded-weight observables the copy complexity is logarithmic in the number of observables.

### Theorem 2: derandomization promise

Algorithm 1 outputs deterministic measurement settings $P^\sharp$ satisfying

$$
\mathrm{Conf}_\varepsilon(O;P^\sharp)
\le
\mathbb E_P\big[\mathrm{Conf}_\varepsilon(O;P)\big].
$$

So the deterministic schedule is never worse than the average fully random schedule.

### Practical improvement over prior heuristics

For molecular Hamiltonians (H$_2$, LiH, BeH$_2$, H$_2$O, NH$_3$ under Jordan--Wigner, parity, and Bravyi--Kitaev encodings), the derandomized schedule beats:

- original local [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes|classical shadows]],
- locally biased shadows,
- largest-degree-first commuting-group heuristics,

at fixed measurement budget in the reported numerics.

The asymptotics are not dramatically new; the constants are.

## Comparison with prior work

| Method | Strength | Weakness |
|---|---|---|
| Direct term-by-term Pauli measurement | Optimal for a few incompatible global strings | Terrible for large observable families |
| [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes|Randomized local shadows]] | Reuse across many low-weight observables; $O(\log L)$ scaling | High-weight terms are hit exponentially rarely |
| Locally biased shadows | Better if term coefficients are informative | Still randomized; still leaves structure on the table |
| LDF / commuting grouping | Good when many terms commute | Does not exploit partial overlap among noncommuting terms |
| This paper | Deterministic schedule tuned to the whole target family; provably no worse than average random | Greedy, offline, and not globally optimal |

## Limits / caveats

- The guarantee is relative to the **average randomized schedule**, not the globally optimal deterministic schedule.
- The greedy update can get stuck. Appendix A gives a stylized non-example where one minority observable is effectively forgotten because the first qubit choice is dominated by a large but internally incompatible majority.
- The protocol assumes the full target observable list is known in advance.
- It stays within shallow local Pauli measurements. That is a feature for NISQ hardware, but it also means it cannot exploit deeper entangling measurement schemes.
- The main benefit is strongest when the observable family has exploitable overlap structure. If everything is mutually incompatible and global, no magic happens.

## Reusable ideas

1. [[Derandomized Pauli Measurement Design via Conditional Expectations]] — turn an average-case randomized measurement guarantee into a deterministic measurement schedule by greedily minimizing conditional expected failure.
2. [[Confidence-Bound Greedy Allocation for Pauli Measurements]] — compress a many-observable estimation problem into one scalar objective that rewards basis choices hitting several target strings at once.

## References within this paper

- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes|Huang, Kueng, Preskill (2020)]] — local randomized measurement baseline; source of the shadow-style sample scaling.
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes|Aaronson (2018)]] — earlier shadow-tomography backdrop.
- Hadfield et al.; Verteletskyi, Yen, Izmaylov; Crawford et al. — commuting-grouping and measurement-reduction heuristics for VQE chemistry Hamiltonians.
- Spencer / pessimistic-estimator derandomization line — the combinatorial method behind the conditional-expectation update rule.

## Cross-links

### Paper notes

- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes]] — randomized local-shadow precursor.
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes]] — coherent many-observable estimation rather than shallow local measurements.
- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes]] — another attempt to adapt shadow-style measurements to a more structured observable family.
- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]] — later shadow-tomography efficiency improvements in a different measurement model.
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]] — same broader Huang programme, but on the learning side rather than measurement design.

### Trick cards

- [[Derandomized Pauli Measurement Design via Conditional Expectations]]
- [[Confidence-Bound Greedy Allocation for Pauli Measurements]]
- [[Classical Shadow Channel Inversion]]
- [[Shadow Norm as Measurement-Observable Match]]
