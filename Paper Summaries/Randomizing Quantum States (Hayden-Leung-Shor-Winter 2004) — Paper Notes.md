> **Source:** Patrick Hayden, Debbie Leung, Peter W. Shor, Andreas Winter, *Randomizing quantum states: constructions and applications*, Communications in Mathematical Physics 250:371–391, 2004
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0307104) · [Journal](https://doi.org/10.1007/s00220-004-1195-6)
> **Tags:** #randomization #private-quantum-channel #data-hiding #locked-correlations #approximate-encryption #quantum-shannon-theory

---

## The problem

A perfectly secure private quantum channel for $d$-dimensional states requires $2\log d$ shared random key bits (matching the superdense coding factor of 2). The question: how many random bits are needed if only *approximate* security is required?

## What the paper does

Shows that $O(\log d)$ random bits suffice — the factor of 2 disappears entirely for approximate encryption. Specifically, $n = O(d \log d / \epsilon^2)$ randomly chosen unitaries define an $\epsilon$-randomizing map. Since $\log n = \log d + O(\log\log d)$, the key length is essentially $\log d$ instead of $2\log d$.

This observation has consequences far beyond cryptography. The paper develops three applications:

1. **LOCC data hiding:** $\sim \log d$ qubits can be hidden in $2\log d$ qubits, far more efficient than any prior construction.
2. **Locked classical correlations:** a negligible classical key can unlock an arbitrarily large amount of classical correlation. Both the amplification ratio and the communication-to-correlation ratio go to zero simultaneously.
3. **Building block for remote state preparation** (developed in a companion paper).

The approximate randomization construction also prefigures the [[Decoupling by Random Measurement|decoupling]] approach that later reshaped quantum Shannon theory.

## The construction

**Definition.** A CPTP map $R : B(\mathbb{C}^d) \to B(\mathbb{C}^d)$ is $\epsilon$-randomizing if for all states $\varphi$:
$$\left\| R(\varphi) - \frac{I}{d} \right\|_\infty \leq \frac{\epsilon}{d}$$

**Theorem II.2 (Main construction).** For all $\epsilon > 0$ and sufficiently large $d$, there exist $n = O(d \log d / \epsilon^2)$ unitaries $\{U_j\}$ in $U(d)$ such that
$$R(\varphi) = \frac{1}{n}\sum_{j=1}^n U_j \varphi U_j^\dagger$$
is $\epsilon$-randomizing.

**Key length:** $\log n = \log d + \log\log d + O(\log(1/\epsilon)) + O(1)$.

For comparison:
- Perfect randomization (Pauli twirl): $d^2$ unitaries, key length $2\log d$
- Approximate randomization: $O(d \log d)$ unitaries, key length $\sim \log d$

### Proof technique

The proof combines two ingredients:

1. **Large deviation bound (Lemma II.3).** For a fixed pure state $\varphi$, rank-$p$ projector $P$, and i.i.d. Haar-random unitaries $\{U_j\}$:
$$\Pr\left[\left|\frac{1}{n}\sum_j \operatorname{Tr}(U_j \varphi U_j^\dagger P) - \frac{p}{d}\right| \geq \frac{\epsilon p}{d}\right] \leq 2\exp(-Cnp\epsilon^2)$$
where $C \geq (6\ln 2)^{-1}$. This uses Cramér's theorem on the moment generating function, bootstrapping from Gaussian random vectors.

2. **$\epsilon$-net (Lemma II.4).** The set of pure states in $\mathbb{C}^d$ has an $\epsilon$-net of size at most $(5/\epsilon)^{2d}$ in trace distance. Take a union bound over all pairs of net points.

The result: the probability that a random choice of $n$ unitaries *fails* is at most $(10d/\epsilon)^{4d} \exp(-Cn\epsilon^2/4)$, which vanishes for $n > 16d \log(10d/\epsilon) / (C\epsilon^2)$.

## The applications

### Private quantum channel

An eavesdropper seeing $R(\varphi)$ has accessible information bounded by the Holevo quantity:
$$\chi \leq \log(1 + \epsilon) \leq \epsilon / \ln 2$$

Choosing $\epsilon = (\log d)^{-\alpha}$ gives $\chi \to 0$ with key length $\log d + (1+2\alpha)\log\log d + O(1)$.

### Data hiding (Theorem IV.2)

Approximate randomization destroys all correlations accessible to LOCC, but *not* all quantum correlations. This asymmetry enables data hiding: encode $\sim \log d$ qubits into $2\log d$ qubits via the randomizing map. LOCC measurements cannot distinguish encoded states (security), but a collective measurement can recover them (correctness, via the transpose channel/pretty good measurement).

The hidden-to-physical qubit ratio approaches $1:2$ — orders of magnitude better than prior constructions.

### Locked correlations (Section V)

There exist bipartite quantum states where:
- Local measurements extract only $O(1)$ bits of mutual information
- Adding $O(\log\log d)$ bits of classical communication unlocks $\Omega(\log d)$ bits of correlation

Both the amplification ratio and the key-to-correlation ratio go to zero simultaneously. This is impossible classically.

## Key results

**Theorem II.2.** $O(d\log d / \epsilon^2)$ random unitaries suffice for $\epsilon$-randomization.

**Lemma III.1 (Classical correlations destroyed).** For separable $\rho_{AB}$, $\|(\mathcal{R} \otimes I)(\rho_{AB}) - I/d \otimes \rho_B\|_1 \leq \epsilon$.

**Lemma III.2 (Quantum correlations hidden from LOCC).** For any LOCC POVM $\{M_i\}$ and maximally entangled $\Phi$: $\|p - q\|_1 \leq \epsilon$ where $p_i = \operatorname{Tr}(M_i (\mathcal{R} \otimes I)(\Phi))$ and $q_i = \operatorname{Tr}(M_i \cdot I/d^2)$.

**Theorem III.3 (General).** For any state $\rho_{AB}$ and LOCC measurement: the distribution of outcomes under $(\mathcal{R} \otimes I)(\rho_{AB})$ is $\epsilon$-close (in $\ell_1$) to the distribution under $I/d \otimes \rho_B$.

**Theorem IV.2 (Data hiding).** For sufficiently large $d$, there exist $(δ, \epsilon, p, d^2)$-qubit hiding schemes with $p = \Theta(\delta^2 \epsilon^2 d / \log d)$ and limiting ratio $\log p / \log d^2 \to 1/2$.

## Comparison with prior work

| Setting | Perfect encryption | Approximate encryption (this paper) |
|---|---|---|
| Key length | $2\log d$ | $\log d + O(\log\log d)$ |
| Number of unitaries | $d^2$ | $O(d\log d)$ |
| Security | Exact: $R(\varphi) = I/d$ | $\|R(\varphi) - I/d\|_\infty \leq \epsilon/d$ |
| Entanglement with environment | Fully destroyed | Quantum correlations survive but hidden from LOCC |

| Data hiding | Werner state (DiVincenzo et al.) | This paper |
|---|---|---|
| Hidden-to-physical ratio | $\sim 1:24$ (for $\delta = \epsilon = 1/16$) | $\to 1:2$ |
| Hidden content | Bits | Qubits |

## Limits / caveats

- **Security criterion.** The $\epsilon$-randomizing definition uses the operator norm, which is stronger than trace distance. The paper shows this stronger notion is needed for the LOCC hiding application (Lemma III.2), but it's not known whether the trace-norm version suffices.
- **Puriﬁcation-accessible adversary.** Security holds only when the adversary cannot access the purification of the input state. With purification access, approximate security can fail even when the trace-norm criterion is satisfied.
- **Non-constructive.** The unitaries are chosen randomly; finding an explicit set of $O(d\log d)$ unitaries achieving $\epsilon$-randomization remained open (later partially addressed by approximate $t$-design constructions).
- **Not optimal.** The constant 134 in the unitary count is not tight. The exact distribution of $\operatorname{Tr}(U\varphi U^\dagger P)$ was computed by Życzkowski and Sommers, which could improve the bounds.

## Reusable ideas

1. [[Approximate Randomization with Few Unitaries]] — The core result: $O(d\log d)$ unitaries suffice for approximate randomization. The factor-of-2 saving over perfect randomization appears because approximate security only needs to erase local information, not global (quantum) correlations.

2. [[LOCC Indistinguishability from Approximate Randomization]] — Approximate randomization of one subsystem makes any bipartite state LOCC-indistinguishable from a product state. The proof exploits the separable structure of LOCC measurements. This is the seed of the data hiding and locked correlation applications.

3. [[Large Deviation Bound for Random Unitaries]] — The Cramér/moment-generating-function approach to concentration of $\operatorname{Tr}(U\varphi U^\dagger P)$ around its mean, bootstrapped from Gaussian random vectors via Jensen's inequality. Reusable for any Haar-random unitary concentration argument.

## References within this paper

- Ambainis, Mosca, Tapp & de Wolf (2000) — perfect private quantum channel requires $2\log d$ key bits
- Boykin & Roychowdhury (2003) — optimal perfect encryption via Pauli group
- Bennett & Brassard (1984) — quantum key distribution
- DiVincenzo, Hayden & Terhal (2002) — original data hiding construction
- Hayden, Horodecki & Terhal (2001) — locking classical correlations, first demonstration
- [[The Mother of All Protocols (Abeyesinghe-Devetak-Hayden-Winter 2006) — Paper Notes|Abeyesinghe, Devetak, Hayden & Winter (2006)]] — later work using approximate randomization for the decoupling approach

## Cross-links

### Paper notes
- [[The Mother of All Protocols (Abeyesinghe-Devetak-Hayden-Winter 2006) — Paper Notes]] — uses the randomization/decoupling ideas from this paper
- [[Quantum State Merging and Negative Information (Horodecki-Oppenheim-Winter 2005) — Paper Notes]] — related decoupling technique, cites this work

### Trick cards
- [[Approximate Randomization with Few Unitaries]]
- [[LOCC Indistinguishability from Approximate Randomization]]
- [[Large Deviation Bound for Random Unitaries]]
- [[Decoupling by Random Measurement]]
