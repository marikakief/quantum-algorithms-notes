> **Source:** Gilles Brassard, Peter Høyer, and Alain Tapp, *Quantum Counting*, 25th International Colloquium on Automata, Languages and Programming (ICALP), LNCS **1443**, pp. 820–831 (1998); arXiv:quant-ph/9805082
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/9805082)
> **Tags:** #counting #amplitude-amplification #amplitude-estimation #grover #phase-estimation #foundational

---

## The computational problem

**Approximate counting:** Given a Boolean function $F: \{0, \ldots, N-1\} \to \{0,1\}$ with $t = |F^{-1}(1)|$ solutions, estimate $t$ using as few evaluations of $F$ as possible.

**Amplitude estimation (generalisation):** Given a quantum algorithm $A$ with $A|0\rangle = |\Psi\rangle$ and success probability $a = \langle\Psi_a|\Psi_a\rangle$, estimate $a$.

---

## What the paper does

Three things, each building on the previous:

1. **Amplitude amplification** — formalises [[Standard Amplitude Amplification|the generalisation of Grover's iteration]] from search to arbitrary quantum algorithms. The operator $Q = -AS_0^{\phi}A^{-1}S_\chi^{\varphi}$ rotates in the good/bad 2D subspace. With $\phi = \varphi = -1$, after $m$ iterations the success probability is $\sin^2((2m+1)\theta)$ where $\sin^2\theta = a$.

2. **Quantum speedup of classical heuristics** — if a classical heuristic finds a solution in expected time $T$, a quantum version finds one in $O(\sqrt{T})$. This uses Cauchy-Schwarz on the heuristic's success distribution.

3. **Approximate counting / amplitude estimation** — the main result. Combines the Grover operator with [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|QFT-based phase estimation]] to estimate $t$ (or more generally $a$).

This paper, together with [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes|BBHT (1998)]], forms the bridge between [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] and the definitive treatment in [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]].

---

## Amplitude amplification (Section 2)

The paper defines the generalised operator:

$$
Q(A, \chi, \phi, \varphi) = -AS_0^{\phi} A^{-1} S_\chi^{\varphi}
$$

where $S_\chi^{\varphi}$ applies phase $\varphi$ to good states and $S_0^{\phi}$ applies phase $\phi$ to the $|0\rangle$ state. The standard Grover case has $\phi = \varphi = -1$, $A = W$ (Walsh-Hadamard), and $\chi = F$.

**Theorem 1 (Amplitude amplification):** With $\phi = \varphi = -1$, after $j$ applications of $Q$:

$$
Q^j A|0\rangle = \frac{1}{\sqrt{a}}\sin((2j+1)\theta)|\Psi_a\rangle + \frac{1}{\sqrt{1-a}}\cos((2j+1)\theta)|\Psi_b\rangle
$$

where $\sin^2\theta = a$.

**Theorem 2 (Quadratic speedup):** With $m = \lfloor\pi/(4\theta)\rfloor$ iterations, success probability $\geq \max(1-a, a)$.

**Exact amplification:** The paper gives two methods to achieve success probability exactly 1:
- Pre-rotate the initial state to adjust $\theta$ to exactly $\pi/(4m_0 + 2)$ for some integer $m_0$.
- Use complex phases $\phi, \varphi$ on the final iteration to land exactly at $\theta = \pi/2$.

---

## Quantum speedup of heuristics (Section 3)

**Theorem 4:** If a classical heuristic $G$ finds a solution to $F$ in expected time $T$ (over the problem distribution), a quantum version finds one in expected $O(\sqrt{T})$.

The proof uses Cauchy-Schwarz: $\sum_F \sqrt{|R|/h_F} \cdot P_F \cdot \sqrt{P_F} \leq (\sum_F \frac{|R|}{h_F} P_F)^{1/2}$, where $h_F$ is the heuristic's number of "good seeds" for instance $F$. The classical expected time is $\sum_F \frac{|R|}{h_F} P_F = T$, so the quantum time is $O(\sqrt{T})$.

This is the formal justification for "any classical search can be quadratically sped up." It applies to Las Vegas algorithms, randomised restarts, and local search heuristics — as long as they can be made reversible.

---

## The counting algorithm (Section 4)

### Algorithm: Count($F$, $P$)

1. Prepare $|\Psi_0\rangle = \frac{1}{\sqrt{PN}} \sum_{m=0}^{P-1} \sum_{x=0}^{N-1} |m\rangle|x\rangle$ (Hadamard on both registers).
2. Apply controlled-$G_F^m$: $|m\rangle|\Psi\rangle \mapsto |m\rangle G_F^m|\Psi\rangle$.
3. (Optional) Measure the second register.
4. Apply QFT to the first register.
5. Measure the first register to get $\tilde{f}$. If $\tilde{f} > P/2$, set $\tilde{f} \leftarrow P - \tilde{f}$.
6. Output $\tilde{t} = N\sin^2(\tilde{f}\pi/P)$.

### Why it works

After step 2, the first register encodes the function $m \mapsto \sin((2m+1)\theta)$ (or $\cos$), which has frequency $f = P\theta/\pi$. The QFT extracts this frequency. Since $\sin^2\theta = t/N$, knowing $\theta$ gives $t$.

If $f$ were an integer, the QFT would return it exactly. In general $f$ is not an integer, so there's spectral leakage — the output is close to $f$ but not exact.

### Main result

**Theorem 5:** The estimate $\tilde{t}$ satisfies:

$$
|t - \tilde{t}| < \frac{2\pi}{P}\sqrt{tN} + \frac{\pi^2}{P^2}N
$$

with probability $\geq 8/\pi^2 \approx 0.81$.

### Different precision regimes

| Goal | Choice of $P$ | Cost (evaluations of $F$) | Error bound |
|---|---|---|---|
| Absolute within $O(\sqrt{t})$ | $P = c\sqrt{N}$ | $c\sqrt{N}$ | $|t - \tilde{t}| < \frac{2\pi}{c}\sqrt{t} + \frac{\pi^2}{c^2}$ |
| Relative within $t/c$ | Use CountRel | $(c + \log\log N)\sqrt{N/t}$ | $|t - \tilde{t}| < t/c$ |
| Exact count | Two-phase estimate | $O(\sqrt{tN})$ | $\tilde{t} = t$ w.h.p. |
| Decide $t = 0$ vs. $t = t_0$ | $P = O(\sqrt{N/t_0})$ | $O(\sqrt{N/t_0})$ | Exact decision |

The relative-error algorithm (CountRel) is a nice construction: it starts with $P = 2$ and doubles until the observed frequency $\tilde{f} > 1$, then calls Count with a large enough $P$ to get the desired relative accuracy. The doubling loop costs a geometric series, so the total work is dominated by the final call.

For exact counting, a two-phase approach: first estimate $t$ to within $O(\sqrt{t})$ using Count($F, \sqrt{N}$), then use this to set $P \approx 20\sqrt{\tilde{t}N}$ for a second call that resolves $t$ exactly (error $< 1/2$, round to nearest integer). Total cost: $O(\sqrt{tN})$ — and this is optimal by the polynomial method lower bound of Beals, Buhrman, Cleve, Mosca and de Wolf.

---

## Key results

| Result | Query complexity | Notes |
|---|---|---|
| Amplitude amplification | $O(1/\sqrt{a})$ applications of $A$ | Known $a$ |
| Unknown-$a$ search | $O(1/\sqrt{a})$ expected | Via [[Randomised Iteration Count for Grover Search\|BBHT schedule]] |
| Heuristic speedup | $O(\sqrt{T})$ | $T$ = classical expected time |
| Approximate counting ($\sqrt{t}$ absolute) | $O(\sqrt{N})$ | Fixed cost |
| Relative counting ($\varepsilon t$ error) | $O(\sqrt{N/t}/\varepsilon)$ | Adaptive |
| Exact counting | $O(\sqrt{tN})$ | Optimal |

---

## Comparison with prior and subsequent work

| Paper | Contribution |
|---|---|
| [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes\|Grover (1996)]] | Original search, $t = 1$ |
| [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes\|BBHT (1998)]] | Exact analysis, unknown $t$, counting sketch |
| **This paper (1998)** | Full amplitude amplification framework, counting algorithm, heuristic speedup |
| [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes\|BHMOT (2002)]] | Definitive treatment with improved error analysis |

---

## Limits / caveats

- The $8/\pi^2$ success probability is for a single run. Standard majority-vote amplification gets this to $1 - \delta$ at $O(\log(1/\delta))$ overhead.
- Controlled-$G^m$ requires $m$ sequential applications of the Grover operator, making the total depth $O(P)$. This is the same depth cost as standard [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]].
- The error bound has a $\sqrt{t}$ additive term — so for very small $t$, relative accuracy requires more work.
- The exact counting algorithm uses $O(\sqrt{tN})$ queries, which is optimal. But it requires knowing an initial estimate of $t$ to set $P$ for the second round, introducing a two-phase structure.

---

## Reusable ideas

1. **[[Controlled Grover Iteration for Frequency Extraction]]:** Applying controlled-$G^m$ creates a quantum state whose amplitudes oscillate with frequency $\theta/\pi$ in the control register. A QFT on the control register extracts this frequency, converting search into counting. This is the template that became [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]].

2. **[[Quadratic Speedup for Classical Heuristics via Amplitude Amplification]]:** Any classical probabilistic algorithm with expected runtime $T$ can be sped up to $O(\sqrt{T})$ by treating the heuristic as the "preparation" algorithm $A$ in amplitude amplification. The proof via Cauchy-Schwarz on the success distribution is clean and general.

---

## References within this paper

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1997)]] — the search algorithm being generalised
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes|Boyer, Brassard, Høyer & Tapp (1998)]] — tight analysis and unknown-$t$ algorithm
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1997)]] — QFT and phase estimation techniques
- [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes|Cleve, Ekert, Macchiavello & Mosca (1998)]] — phase estimation framework
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|Bennett, Bernstein, Brassard & Vazirani (1997)]] — $\Omega(\sqrt{N})$ lower bound
- Beals, Buhrman, Cleve, Mosca & de Wolf (1998) — polynomial method lower bound for counting
- Brassard & Høyer (1997) — earlier amplitude amplification (Simon's problem application)

---

## Cross-links

### Paper notes
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]] — the companion paper
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] — the definitive generalisation
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — the original search algorithm
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — phase estimation
- [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes]] — QPE circuit construction used here
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]] — applies counting/estimation to Monte Carlo

### Trick cards
- [[Controlled Grover Iteration for Frequency Extraction]] — the counting technique
- [[Quadratic Speedup for Classical Heuristics via Amplitude Amplification]] — the heuristic result
- [[Standard Amplitude Amplification]] — the core operation
- [[Amplitude Estimation via Phase Estimation on Q]] — the mature version of counting
- [[Randomised Iteration Count for Grover Search]] — the unknown-$a$ search from BBHT
