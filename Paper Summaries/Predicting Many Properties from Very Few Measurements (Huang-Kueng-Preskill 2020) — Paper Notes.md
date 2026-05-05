> **Source:** Hsin-Yuan Huang, Richard Kueng, John Preskill, *Predicting many properties of a quantum system from very few measurements*, arXiv:2002.08953, Nature Physics **16**, 1050–1057 (2020)
> **Links:** [arXiv](https://arxiv.org/abs/2002.08953) · [Nature Physics](https://www.nature.com/articles/s41567-020-0932-7)
> **Tags:** #shadow-tomography #classical-shadows #measurement #quantum-learning #tomography

---

## The computational problem

Given many identical copies of an unknown $n$-qubit state $\rho$ and a list of $M$ target observables

$$O_1,\ldots,O_M,$$

estimate all expectation values $\operatorname{tr}(O_i\rho)$ to additive error $\varepsilon$ with failure probability at most $\delta$, using as few state preparations and measurements as possible.

The key restriction is that the measurements are **single-copy** and fixed before the later prediction task. The payoff is that the same measurement data can be reused for many observables chosen after the experiment.

## What the paper does

This is the classical-shadows paper. It turns Aaronson-style [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes|shadow tomography]] into a practical randomized-measurement protocol: apply a random Clifford or local Pauli measurement, store a short classical snapshot, and later estimate many observables by linear inversion plus median-of-means.

The result is clean: the number of measurements scales like $\log M/\varepsilon^2$ times a measurement-dependent **shadow norm**. For local observables with local Pauli measurements the cost is $O(4^k\log M/\varepsilon^2)$, independent of the total system size. For global Clifford measurements the cost is controlled by Hilbert--Schmidt norm, so fidelities and many global witnesses become accessible with surprisingly few copies.

My assessment: this is foundational, and much more central to Robert Huang's later programme than the splashier 2022--2025 claims. It gave the community a reusable measurement primitive; later QML, many-body learning, randomized-measurement, and chemistry papers are mostly specialised ways of choosing the right shadow ensemble or making the post-processing efficient.

## The algorithm / construction

### 1. Randomized measurement primitive

Choose a random unitary $U$ from an ensemble $\mathcal{U}$, apply it to $\rho$, and measure in the computational basis. If the bit string outcome is $\hat b$, the raw snapshot is

$$U^\dagger |\hat b\rangle\!\langle \hat b| U.$$

Averaged over $U$ and the measurement outcome, this defines a quantum channel

$$\mathcal{M}(\rho)=\mathbb{E}_{U,\hat b}\left[U^\dagger |\hat b\rangle\!\langle \hat b|U\right].$$

If $\mathcal{M}$ is invertible on the relevant operator space, form the single-shot unbiased estimator

$$\hat\rho = \mathcal{M}^{-1}\!igl(U^\dagger |\hat b\rangle\!\langle \hat b|U\bigr).$$

This is not a physical density matrix in general. It is a classical estimator satisfying $\mathbb{E}[\hat\rho]=\rho$.

### 2. Explicit inverses

For global Clifford measurements on $n$ qubits,

$$\mathcal{M}^{-1}(X)=(2^n+1)X-\mathbb{I}.$$

For tensor-product single-qubit Clifford / Pauli measurements,

$$\mathcal{M}^{-1}=\bigotimes_{j=1}^{n}\mathcal{M}_1^{-1}, \qquad \mathcal{M}_1^{-1}(X)=3X-\mathbb{I}.$$

This factorisation is why local Pauli shadows are experimentally cheap and naturally suited to $k$-local observables.

### 3. Median-of-means prediction

Take $NK$ independent snapshots and split them into $K$ batches of size $N$. For each batch $k$, average the snapshots to obtain $\hat\rho_{(k)}$. For each observable $O_i$, output

$$\hat o_i=\operatorname{median}_{1\le k\le K}\operatorname{tr}(O_i\hat\rho_{(k)}).$$

The median-of-means wrapper is not cosmetic. Single classical-shadow estimators can have heavy tails; batching gives high-probability simultaneous guarantees for all $M$ observables.

## Key results

### General upper bound

For a measurement primitive $\mathcal{U}$ and Hermitian observables $O_1,\ldots,O_M$, define the shadow norm

$$
\|O\|_{\rm shadow}^2
=
\max_{\sigma\;{\rm state}}
\mathbb{E}_{U\sim\mathcal U}\sum_b
\langle b|U\sigma U^\dagger|b\rangle
\langle b|U\mathcal{M}^{-1}(O)U^\dagger|b\rangle^2.
$$

Theorem 1 states that with

$$K=2\log(2M/\delta),$$

and

$$N=\frac{34}{\varepsilon^2}\max_{1\le i\le M}\left\|O_i-\frac{\operatorname{tr}(O_i)}{2^n}\mathbb{I}\right\|_{\rm shadow}^2,$$

the $NK$ independent shadows estimate all $M$ observables to error $\varepsilon$ with probability at least $1-\delta$.

So the total copy complexity is

$$O\!\left(\frac{\log(M/\delta)}{\varepsilon^2}\max_i\|O_i\|_{\rm shadow}^2\right),$$

up to constants and the harmless trace subtraction.

### Global Clifford shadows

For global Clifford measurements,

$$\|O\|_{\rm shadow}^2 \le 3\operatorname{tr}(O^2).$$

This is good for global observables with small Hilbert--Schmidt norm: fidelities with known pure states, many entanglement witnesses, and stabilizer-type properties. The cost is circuit depth: generic Clifford measurements are not the NISQ-friendly part of the paper.

### Local Pauli shadows

For a $k$-local observable $O$ measured with local Pauli / single-qubit Clifford shadows,

$$\left\|O-\frac{\operatorname{tr}(O)}{2^n}\mathbb{I}\right\|_{\rm shadow}^2 \le 4^k\|O\|_\infty^2.$$

Thus $M$ $k$-local observables can be estimated with

$$O\!\left(\frac{4^k\log(M/\delta)}{\varepsilon^2}\right)$$

copies, independent of $n$ except through $M$ and the observable list.

### Lower bounds

For fixed single-copy measurement procedures, the logarithmic dependence on $M$ and the norm dependence are essentially unavoidable.

- For arbitrary observables under global single-copy measurements, the paper proves a minimax lower bound

$$N\ge \Omega\!\left(\frac{B\log M}{\varepsilon^2}\right)$$

when the target effects satisfy $0\preceq O_i\preceq I$ and $\max_i\|O_i\|_2^2\le B$, for $M\le \exp(d/32)$.

- For local measurements and arbitrary $k$-local observables, it proves

$$N\ge \Omega\!\left(\frac{3^k\log M}{\varepsilon^2}\right).$$

The local upper bound has $4^k$ while the lower bound has $3^k$; for tensor-product Pauli observables the scaling is tighter.

### Nonlinear functions

Quadratic functions can be handled by applying the same logic to independent shadows of $\rho\otimes\rho$. For example, second Rényi entropies of a subsystem $A$ can be estimated by using the swap operator on $A$:

$$\operatorname{tr}(\rho_A^2)=\operatorname{tr}(\mathrm{SWAP}_A\,\rho^{\otimes 2}).$$

The cost is exponential in the subsystem size $|A|$, not in the full system size $n$. This became a bridge to the randomized-measurement literature on entanglement estimation.

## Comparison with prior work

| Method | Measurement model | Copy scaling | Practical issue |
|---|---:|---:|---|
| Full tomography | informationally complete | $\Omega(4^n)$ | learns far more than needed |
| MPS tomography | local measurements + structure | polynomial for low-bond-dimension states | assumes strong tensor-network structure |
| [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes|Aaronson shadow tomography]] | collective / adaptive measurements | polylogarithmic in $M$ and dimension, worse in $\varepsilon$ | not experimentally simple |
| Classical shadows | fixed single-copy randomized measurements | $O(\|O\|_{\rm shadow}^2\log M/\varepsilon^2)$ | choose ensemble well; post-processing may dominate |
| Direct Pauli measurement | measure each term separately | often $O(M/\varepsilon^2)$ | no logarithmic reuse across observables |

## Limits / caveats

- The method is only as good as the measurement ensemble. Local Pauli shadows are poor for highly nonlocal observables; global Clifford shadows are poor for shallow hardware.
- The guarantees are additive. If the target quantity is exponentially small, the nominal $\varepsilon$ may still be scientifically useless.
- The classical post-processing can be hard even when the sample complexity is excellent. This is exactly why later work such as [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes|matchgate shadows]] matters.
- The lower bounds are for fixed single-copy measurements. They do not rule out adaptive, collective, or structure-exploiting procedures for special state families.
- The paper gives a measurement primitive, not a full learning theory. Generalisation, distributional assumptions, and quantum-vs-classical prediction advantage are handled in later work such as [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]].

## Reusable ideas

1. [[Classical Shadow Channel Inversion]] — turn one randomized measurement outcome into an unbiased estimator of the state by inverting the average measurement channel.
2. [[Median-of-Means Shadow Prediction]] — get simultaneous high-probability estimates for many observables despite heavy-tailed single-shot estimators.
3. [[Shadow Norm as Measurement-Observable Match]] — choose the randomized measurement ensemble by minimizing the shadow norm for the observable family, not by trying to reconstruct the whole state.

## References within this paper

- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes|Aaronson (2018)]] — earlier shadow tomography with strong sample guarantees but much less experimental simplicity.
- Cramer, Plenio, Flammia, Somma, Gross, Bartlett, Landon-Cardinal, Poulin, Liu (2010) — MPS tomography; efficient under low-entanglement structure.
- Flammia and Liu (2011); da Silva, Landon-Cardinal, Poulin (2011) — direct fidelity estimation; precursor for estimating specific observables without full tomography.
- Brydges et al. (2019), Elben et al. (2018/2019) — randomized measurements for Rényi entropies and entanglement in many-body experiments.
- Gottesman--Knill stabilizer simulation — makes Clifford shadows classically representable and computable for stabilizer-compatible observables.
- Spencer's method of conditional expectations / pessimistic estimators — foreshadows [[Efficient estimation of Pauli observables by derandomization (Huang-Kueng-Preskill 2021) — Paper Notes|the later derandomized Pauli measurement paper]].

## Cross-links

### Paper notes

- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]] — conceptual predecessor.
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]] — uses shadows as part of projected quantum kernels.
- [[Efficient estimation of Pauli observables by derandomization (Huang-Kueng-Preskill 2021) — Paper Notes]] — replaces random local Pauli measurements by a deterministic schedule tuned to a fixed observable family.
- [[Learning to predict arbitrary quantum processes (Huang-Chen-Preskill 2023) — Paper Notes]] — reuses local shadows to learn low-degree Heisenberg-picture observables of an unknown process rather than observables of a fixed state.
- [[Learning shallow quantum circuits (Huang-Liu-Broughton-Kim-Anshu-Landau-McClean 2024) — Paper Notes]] — uses randomized single-qubit measurements to reconstruct local Heisenberg data and then sew together a shallow circuit description.
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes]] — coherent alternative for estimating many observables; useful comparison point.
- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]] — later attempt to improve shadow tomography on sample, time, and measurement efficiency.
- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes]] — replaces Clifford shadows with fermionic Gaussian measurements to fix post-processing for fermionic observables.
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes]] — uses classical shadows as an overlap-estimation primitive.

### Trick cards

- [[Classical Shadow Channel Inversion]]
- [[Median-of-Means Shadow Prediction]]
- [[Shadow Norm as Measurement-Observable Match]]
- [[Shadow Tomography for QMC Overlap Estimation]]
- [[Matchgate 3-Design for Classical Shadows]]
