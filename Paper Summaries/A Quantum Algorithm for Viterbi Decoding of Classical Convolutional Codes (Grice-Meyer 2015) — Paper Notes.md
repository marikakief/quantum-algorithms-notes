# A Quantum Algorithm for Viterbi Decoding of Classical Convolutional Codes (Grice-Meyer 2015) — Paper Notes

> **Source:** Jon R. Grice and David A. Meyer, *A Quantum Algorithm for Viterbi Decoding of Classical Convolutional Codes*, arXiv:1405.7479, 2015
> **Links:** [arXiv](https://arxiv.org/abs/1405.7479) · [PDF](https://arxiv.org/pdf/1405.7479)
> **Tags:** #quantum-algorithms #coding-theory #viterbi #amplitude-amplification #hidden-markov-models

---

## The computational problem

The paper targets maximum-likelihood decoding for a hidden Markov model (HMM), with convolutional-code decoding as the worked example.

Input:

- a finite hidden-state alphabet $Q$;
- a visible emission sequence $y_1,\ldots,y_N$;
- transition/emission probabilities
  $$
  P(i,j\mid y)=\Pr(x_{n+1}=j\mid x_n=i,y_n=y),\qquad
  P(y\mid i,j)=\Pr(y_n=y\mid x_n=i,x_{n+1}=j),
  $$
  and $P_{i,j}(y)=P(i,j\mid y)P(y\mid i,j)$;
- fanout $F$, the maximum number of allowed transitions out of any hidden state.

Output: a most likely hidden-state path
$$
\pi^*\in Q^N,
$$
given the emissions. For a convolutional code this path is the encoder-state history, from which the decoded message is read.

The comparison point is the classical Viterbi algorithm, which keeps the best predecessor for each state at every time step. Grice and Meyer instead build a coherent superposition over all legal paths through the trellis and try to bias measurement toward the most likely path.

## What the paper does

Grice and Meyer propose a quantum Viterbi algorithm (QVA): prepare all legal trellis paths in superposition, mark each path by a phase depending on its likelihood, then use a Grover-like amplification step on the legal-path subspace. One iteration has gate complexity
$$
O(N|Q|F(\log F)^2),
$$
and, assuming the $|Q|$ controlled transition blocks are run in parallel, time complexity $O(N\log F)$.

My assessment: this is a useful algorithmic idea but not a clean black-box speedup theorem. The advantage is regime-dependent: it needs low fanout $F\ll |Q|$, enough parallel hardware to hide the $|Q|$ factor, and a small number of amplification iterations or short decoding frames.

## The algorithm / construction

### 1. Encode trellis paths as computational-basis states

A path
$$
\pi=\pi_1\pi_2\cdots \pi_N
$$
is represented by
$$
|\pi_1\pi_2\cdots\pi_N\rangle,
$$
with each factor living in $\mathbb C^Q$. Let $F_N$ denote the set of admissible paths of length $N$, with $|F_N|\approx F^N$ in the uniform-fanout case.

For example, a two-step trellis can be represented by an unnormalised state of the form
$$
|\psi_2\rangle
= e^{i f(p_{0,0})} f(p_{0,0}) |000\rangle
+ e^{i f(p_{0,0})} f(p_{0,2}) |002\rangle
+ e^{i f(p_{0,2})} f(p_{2,1}) |021\rangle
+ e^{i f(p_{0,2})} f(p_{2,2}) |022\rangle,
$$
where $f$ is chosen monotone in the path probability. The formula in the paper is notationally loose here — the role of $f$ is to set relative phase, not to define a normalised amplitude distribution.

### 2. Build one transition layer with controlled state preparation

For a hidden state $k$ and emission $y$, define the transition state
$$
|\psi_{k,y}\rangle=\sum_j e^{i f(P_{k,j}(y))}|j\rangle,
$$
where the sum is over the at-most-$F$ allowed next states. Choose a unitary $U_{|\psi_{k,y}\rangle}$ such that
$$
U_{|\psi_{k,y}\rangle}|0\rangle=|\psi_{k,y}\rangle.
$$

The block $V_y$ is a block-diagonal controlled operation on $\mathbb C^Q\otimes\mathbb C^Q$:
$$
V_y=\sum_{k\in Q} |k\rangle\langle k|\otimes U_{|\psi_{k,y}\rangle}.
$$
It appends a next-state register conditioned on the current state and the observed emission $y$.

The circuit applies
$$
V_{y_1},V_{y_2},\ldots,V_{y_N}
$$
sequentially, each time appending a fresh $|0\rangle$ register.

### 3. Phase-mark complete paths by their probabilities

After the $N$ transition layers, Proposition 1 states that the system is in
$$
|\psi_f\rangle
=\sum_{\pi\in F_N} e^{i\prod_j f(\pi_j)} |\pi\rangle,
$$
where $\prod_j \pi_j$ denotes the path probability in the authors' notation. The construction is proved by induction over layers: the $N$th $V_{y_N}$ appends every legal next state $j$ from the current state $i$ and multiplies by the phase determined by $P_{i,j}(y_N)$.

I would rewrite the phase more transparently as
$$
\exp\!\left(i\Phi(\pi)\right),\qquad
\Phi(\pi)=\sum_{n=1}^N \phi(P_{\pi_{n-1},\pi_n}(y_n)),
$$
for some monotone scoring function $\phi$. That is the version one would actually implement.

### 4. Implement the transition preparation unitaries

For a $q=\log |Q|$ register and a vector $|\psi\rangle$ with $K\le F$ nonzero entries, the paper gives a canonical construction using two-level $R_y$ rotations. A product
$$
V=R_y(\theta_1)_{\{|0\rangle,|1\rangle\}}
R_y(\theta_2)_{\{|0\rangle,|2\rangle\}}
\cdots
R_y(\theta_K)_{\{|0\rangle,|K\rangle\}}
$$
sets the first column to the desired vector in spherical coordinates. Combined with Gray-code logic and generalized Toffoli gates, this costs
$$
O(F(\log F)^2)
$$
gates per $U_{|\psi\rangle}$. Since each $V_y$ has $|Q|$ controlled blocks and there are $N$ layers, one QVA iteration costs
$$
O(N|Q|F(\log F)^2)
$$
gates.

### 5. Amplify the most likely path

The paper then applies a legal-path reflection/amplification step. It defines the legal-path inversion as
$$
G_{F_N}=I-\frac{1}{\sqrt{F^N}}\sum_{\pi\in F_N}|\pi\rangle\langle\pi|,
$$
although dimensionally this should be read as the reflection-like operation about the legal-path superposition. Operationally it is the [[Standard Amplitude Amplification]] move: turn the phase profile into a larger amplitude on the best path.

If a global phase is chosen so that the most likely path has phase near $-1$ and lower-likelihood paths have phases nearer $1$, the step behaves like a noisy [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover]] search. The number of amplification iterations can be as high as
$$
O(\sqrt{L}),\qquad L=F^N,
$$
but the authors argue that in short-frame decoding examples fewer iterations may suffice after tuning the phase parameter.

### 6. Specialisation to binary rate-$1/2$ convolutional codes

For a binary $(n,k)$ convolutional code with memory $m$, the encoder state is the contents of the shift register. Increasing $m$ grows $|Q|$ while the fanout stays
$$
F=2^k.
$$
That is the regime where the proposed method is most favourable: large state space, fixed local branching.

For the paper's $(2,1)$ example with $m=2$, $F=2$, and the state diagram has four shift-register states. The received block fixes a classically controlled transition block $V_{00},V_{01},V_{10},V_{11}$. For a received $00$ block, the mapping includes
$$
|00\rangle|00\rangle\mapsto \frac{1}{\sqrt2}|00\rangle(e^{i0}|00\rangle+e^{2i\omega}|01\rangle),
$$
with analogous cases for the other starting states. The phase $\omega$ encodes Hamming-error cost: fewer-error paths are intended to lie closer to the marked phase.

For $N=4$ and no channel errors, the authors report that choosing $\omega=0.68$ and applying three amplification rounds gives measurement probability $0.673$ for the correct path $|0000\rangle$. They tabulate tuned values for $N=3,\ldots,10$; the number of iterations ranges from $2$ to $25$, and reported success probabilities range from $0.67$ to $0.82$ for the no-error case.

### 7. Probabilistic QVA variant

If the HMM satisfies
$$
\sum_j P_{i,j}(y)=1\quad\text{for all }i,y,
$$
or if $f$ is chosen so the transition probabilities can be loaded directly into amplitudes, the algorithm can skip amplification and sample the path distribution. Then selecting the mode reduces to a multinomial-selection problem.

Using a lemma from Geroch, if the largest and second-largest probabilities are $b$ and $b'$, the error probability $\kappa(r)$ after $r$ trials satisfies
$$
\lim_{r\to\infty}\frac{-\log \kappa(r)}{r}
=\lambda
=\frac{(b-b')^2}{2\left[b(1-(b-b'))^2+b'(1+(b-b'))^2\right]}.
$$
For their no-error decoding example with $E_0=0.8$, achieving error probability around $e^{-2}$ needs roughly
$$
r\sim 24(1.25)^N-4
$$
trials. The authors conclude that for these convolutional codes, the amplified QVA is quadratically better than repeated probabilistic sampling.

## Key results

**Per-iteration cost.** A single QVA iteration has gate complexity
$$
O(N|Q|F(\log F)^2),
$$
and time complexity
$$
O(N\log F)
$$
if the $|Q|$ controlled transition operations can be executed in parallel.

**Worst-case amplification count.** The number of amplification iterations may be as large as
$$
O(\sqrt{L}),\qquad L=F^N.
$$
For trellis decoding, the paper writes the final time with maximal parallelism as
$$
O(\sqrt{F^N}\,N\log F),
$$
modulo repetitions/trials needed to accumulate a reliable classical mode.

**Fanout-based separation from the classical trellis size.** For binary convolutional codes with block size $k$, the fanout is
$$
F=2^k,
$$
while $|Q|$ grows with the constraint length. The QVA's advertised advantage is therefore in large-constraint-length, short-frame, small-$k$ settings.

**Probabilistic trial bound.** If probabilities are loaded into amplitudes and no amplification is used, the probability that the empirical mode is wrong decays as
$$
\kappa(r)\approx e^{-\lambda r},
$$
with $\lambda$ as above. For the example $E_0=0.8$, the required trials scale like
$$
r\sim 24(1.25)^N-4.
$$

## Comparison with prior work

| Method | Target | Cost / behaviour | Comment |
|---|---|---:|---|
| Classical Viterbi | Best HMM/trellis path | dynamic program over $N$ layers and $|Q|$ states | Strong classical baseline; deterministic and stable. |
| Plain Grover over paths | Search over $F^N$ paths | $O(\sqrt{F^N})$ oracle calls | Ignores trellis tensor structure unless the oracle is built carefully. |
| Grice--Meyer QVA | HMM/convolutional-code trellis | one iteration $O(N|Q|F(\log F)^2)$ gates; $O(N\log F)$ parallel time | Useful only when fanout is small, parallelism over $|Q|$ is realistic, and iterations stay low. |
| Probabilistic QVA | Sample path-likelihood distribution | trial count governed by multinomial gap $b-b'$ | Natural for small $N$, but close path probabilities kill it. |

This is less polished than the later quantum-walk/backtracking literature: the paper gives an explicit coherent trellis construction, but the success analysis is mostly example-driven and phase-tuning dependent.

## Limits / caveats

- The speedup is conditional on hardware-level parallelism over the $|Q|$ controlled transition blocks. Without it, the $|Q|$ factor remains in time.
- The amplification step is not analysed as a clean theorem for arbitrary likelihood landscapes. The phase choice $\omega$ is tuned empirically in the worked examples.
- If the best and second-best paths have close probabilities, both the probabilistic QVA and the phase-amplified QVA need extra trials or better phase design.
- The legal-path reflection $G_{F_N}$ is sketched rather than fully compiled for arbitrary HMMs. The stated formula is not quite the standard reflection normalisation, so one should treat it as an algorithmic template rather than a finished circuit family.
- For long decoding frames $N$, the $\sqrt{F^N}$ amplification ceiling can erase the advantage.
- Classical Viterbi is hard to beat in ordinary convolutional-code regimes where $F$ and $|Q|$ are not separated enough or the frame length is large.

## Reusable ideas

1. [[Trellis Path Superposition for Hidden Markov Models]] — build the HMM path lattice coherently by appending transition registers with controlled state-preparation blocks.
2. [[Phase-Weighted Viterbi Trellis Amplification]] — encode path likelihoods as phases and use a legal-path reflection to bias measurement toward the maximum-likelihood path.
3. [[Fanout-Local State Preparation for Sparse HMM Transitions]] — exploit small fanout $F$ even when the hidden state space $|Q|$ is large, by preparing only locally supported transition states.

## References within this paper

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — amplitude amplification/search analogy and the $O(\sqrt L)$ iteration scale.
- [[Quantum Fourier Transform Circuit]] / Coppersmith (1994), Deutsch (1985) — cited for the tensor-product lesson from the quantum Fourier transform.
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes|Dürr--Høyer (1996)]] — function minimisation via Grover search, conceptually close to selecting the most likely path.
- Meyer and Pommersheim (2010) — multiphase kickback scheme for learning from Hamming-distance oracles; cited as related to using non-binary phases.
- Barg and Zhou (1998) — quantum decoding of the simplex code; listed by the Zoo as a neighbouring coding paper but not yet in this vault batch because no arXiv PDF is recorded.
- Montanaro (2012) — Reed--Muller learning/decoding connection.
- Forney (1973) and Viterbi (1967) — classical trellis decoding foundations.
- Rabiner (1989) — HMM tutorial background.

## Cross-links

### Paper notes

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]

### Trick cards

- [[Trellis Path Superposition for Hidden Markov Models]]
- [[Phase-Weighted Viterbi Trellis Amplification]]
- [[Fanout-Local State Preparation for Sparse HMM Transitions]]
- [[Standard Amplitude Amplification]]
- [[Grover Search with Evolving Threshold for Minimum Finding]]
- [[Quantum Fourier Transform Circuit]]
