> **Source:** Sadegh Raeisi, Mária Kieferová, Michele Mosca, *Novel Technique for Robust Optimal Algorithmic Cooling*, arXiv:1902.04439, Phys. Rev. Lett. **122**, 220501 (2019)
> **Links:** [arXiv](https://arxiv.org/abs/1902.04439) · [PRL](https://doi.org/10.1103/PhysRevLett.122.220501)
> **Tags:** #algorithmic-cooling #HBAC #quantum-thermodynamics #entropy-compression

---

## The computational problem

The paper asks whether **heat-bath algorithmic cooling (HBAC)** can reach the optimal cooling limit without the fragile state-dependent sorting step used in the partner-pairing algorithm (PPA).

Setup: there are $n$ computation qubits and one reset qubit that can be refreshed to a bath state with polarization $\epsilon$. Standard HBAC alternates between refreshing the reset qubit and applying a compression unitary that pushes entropy out of the computation register. The best-known asymptotic protocol, PPA, needs the compression step to depend on the current diagonal of the density matrix. That is ugly in practice because it means tomography, adaptive control, and a different circuit at each round.

The problem here is to get the same asymptotic cooling limit with a **fixed operation**.

## What the paper does

The paper introduces **two-sort algorithmic cooling (TSAC)**, a state-independent HBAC protocol that repeatedly applies the same unitary after each reset. It reaches the same optimal asymptotic state as PPA, while avoiding the adaptive sorting step that makes PPA brittle.

This is a nice paper. The theorem is clean, the fixed-point analysis is clean, and the practical message is better than the title suggests: sometimes full sorting is overkill, and a carefully chosen local permutation has the same limit.

## The algorithm / construction

### 1. Standard HBAC setting

Take $n$ computation qubits and one reset qubit. After each round, the reset qubit is refreshed to the bath state

$$
\rho_R = \frac{1}{z}\begin{pmatrix} e^{\epsilon} & 0 \\ 0 & e^{-\epsilon} \end{pmatrix},
\qquad z = e^{\epsilon}+e^{-\epsilon}.
$$

If the diagonal of the computation register is

$$
\mathbf p = (p_0,\dots,p_{2^n-1}),
$$

a refresh turns the joint diagonal into

$$
(p_0 e^{\epsilon}, p_0 e^{-\epsilon}, p_1 e^{\epsilon}, p_1 e^{-\epsilon}, \dots)/z.
$$

PPA would now sort these populations in decreasing order. TSAC does something much simpler.

### 2. The two-sort unitary

Define the fixed unitary $U_{\mathrm{TS}}$ that swaps adjacent basis states pairwise, except for the first and last basis states, which stay put:

$$
U_{\mathrm{TS}} = |0\rangle\langle 0| + |2^{n+1}-1\rangle\langle 2^{n+1}-1| + \sum_{i\in\mathrm{odd}} \left(|i\rangle\langle i+1| + |i+1\rangle\langle i|\right).
$$

So the compression step is not “sort everything”. It is “swap neighbors in a fixed brickwork pattern, with boundaries pinned”.

After each refresh, apply this same $U_{\mathrm{TS}}$ again.

### 3. Markov-chain viewpoint

Because the operation is fixed, the diagonal populations evolve under a time-homogeneous Markov chain. The induced transition matrix has a unique fixed point, and that fixed point is exactly the optimal asymptotic HBAC distribution:

$$
p_i^{\infty} \propto e^{-2 i \epsilon}.
$$

This is the same cooling limit reached by PPA.

That is the real idea in the paper: replace adaptive optimal control by a fixed linear map whose stationary distribution is already the one you want.

### 4. Monotonic cooling and implementation

The authors prove that if the target qubit is hotter than the HBAC limit, TSAC increases its polarization monotonically. They also give a circuit for $U_{\mathrm{TS}}$ using a cyclic [[SHIFT Operator via QFT Phases]] plus a multi-controlled Toffoli, with gate count $O(n^2)$ per round.

## Key results

The main claims are:

1. **TSAC converges to the optimal asymptotic state (OAS).** The fixed point is the same one obtained by PPA, namely a geometric population profile along the computational basis.

2. **Per-round circuit cost is polynomial:**

$$
\text{gates per iteration} = O(n^2).
$$

3. **Mixing time is exponential in the number of cooled qubits:**

$$
t_{\mathrm{mix}} = O(2^n).
$$

So the protocol is operationally simple per step, but exact saturation of the cooling limit still takes exponentially many rounds.

4. **Monotonicity theorem.** If the first computation qubit has polarization below the HBAC limit, repeated TSAC steps increase it toward that limit.

## Comparison with prior work

| Paper / method | Compression step | Needs state estimation? | Asymptotic limit | Per-step complexity |
|---|---|---|---|---|
| Boykin et al. style algorithmic cooling | Fixed, non-optimal compression | No | Below HBAC optimum | Simple |
| Correlation-enhanced / HBAC variants | Usually structured but not optimal | Sometimes | Better than naive cooling | Varies |
| Partner-pairing algorithm (PPA) | Full sort of diagonal populations | **Yes** | Optimal | Conceptually expensive |
| **TSAC (this paper)** | Fixed adjacent-pair swaps | **No** | Optimal | $O(n^2)$ gates |

The tradeoff is obvious: PPA is greedy and adaptive, TSAC is blind but converges to the same place. In practice that blindness is a feature.

## Limits / caveats

1. **Mixing time is exponential.** The protocol is simple, but saturation is not fast in $n$.
2. **The result is asymptotic.** If you care about the best finite-round cooling schedule, TSAC is not obviously the winner.
3. **The paper studies diagonal-state evolution.** Coherences are not the point here; the protocol is about entropy redistribution under reset.
4. **Hardware noise still matters.** The protocol avoids tomography noise and adaptive-control errors, but gate noise in repeated applications of $U_{\mathrm{TS}}$ is still there.

## Reusable ideas

1. [[State-Independent Markovian Cooling]]
2. [[Lexicographic Two-Sort Compression]]
3. [[SHIFT Operator via QFT Phases]]

## References within this paper

- Boykin et al. (2002) — early algorithmic cooling protocol.
- Schulman, Mor, Weinstein, Vazirani (2005) — heat-bath algorithmic cooling framework.
- Raeisi and Mosca (2015/2017) — previous algorithmic-cooling work by overlapping authors.
- Rodríguez-Briones et al. (2017) — correlation-enhanced algorithmic cooling.
- Partner-pairing algorithm literature — state-dependent optimal HBAC benchmark that TSAC matches asymptotically.
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] — not a cooling paper, but relevant background for Mosca’s broader algorithmic lineage.

## Cross-links

### Paper notes
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Tomography and Generative Training with Quantum Boltzmann Machines (Kieferová-Wiebe 2017) — Paper Notes]]

### Trick cards
- [[State-Independent Markovian Cooling]]
- [[Lexicographic Two-Sort Compression]]
- [[SHIFT Operator via QFT Phases]]
