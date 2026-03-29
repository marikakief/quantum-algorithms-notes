> **Source:** Richard P. Feynman, *Simulating physics with computers*, International Journal of Theoretical Physics **21**(6/7):467–488, 1982
> **Links:** [Journal](https://doi.org/10.1007/BF02650179)
> **Tags:** #foundational #quantum-simulation #vision #Bell-inequality #negative-probability

---

## What the paper does

This is the paper that started quantum computing as a research programme. Feynman asks: can a classical computer efficiently simulate a quantum physical system? His answer is no — and the reason is that quantum mechanics requires negative "probabilities" (the Wigner function can go negative), which a classical probabilistic computer cannot represent. He then proposes that the right way to simulate quantum physics is with a computer that is itself quantum mechanical — a *quantum computer*.

The paper is a keynote address, not a technical contribution. There are no theorems, no algorithms, no complexity bounds. What there is: a clean argument structure that identifies exactly *why* classical simulation fails for quantum systems, and a concrete proposal for what to do about it. Everything that followed — [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd's 1996 proof]], [[Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012) — Paper Notes|Jordan-Lee-Preskill's QFT algorithms]], the entire field of [[Hamiltonian simulation]] — traces back to this talk.

---

## The argument structure

### 1. Simulating classical physics (Sections 1–2)

Feynman sets up the rules: a simulation should use resources proportional to the space-time volume of the system being simulated. No exponential blow-ups allowed.

Classical physics is local, causal, and reversible. A cellular automaton can imitate it. Each point in space-time has a state that depends only on its neighbours' states in the past. This is fine — classical simulation of classical physics works.

### 2. Simulating probability (Section 3)

What about probabilistic systems? You can't store the full probability distribution $P(x_1, x_2, \ldots, x_R, t)$ for $R$ particles — if each particle can be at $N$ positions, you need $N^R$ numbers. That's exponential in $R$.

But you don't need to *compute* the probability. You can instead build a probabilistic computer $C$ that *behaves with the same probabilities* as the physical system $\mathcal{N}$. Run both many times, collect statistics — they should match. This is Monte Carlo simulation, essentially.

A local probabilistic computer has two properties:
1. You can find the probability of local events by just ignoring the rest (marginalisation is free)
2. The transition rules are local — the "probability" of a state transition at point $i$ depends only on the neighbourhood

### 3. Why quantum mechanics breaks this (Sections 4–6)

Now try the same thing for quantum mechanics. The wavefunction $\psi(x_1, \ldots, x_R, t)$ has $N^R$ variables — same exponential scaling. Can we at least simulate the *probabilities* that quantum mechanics predicts?

Feynman reformulates quantum mechanics using the Wigner function:

$$W(x, p) = \int \rho(x + y, x - y) e^{ipy} \, dy$$

This looks like a probability distribution — it's real, it integrates to 1, and its marginals give the correct quantum probabilities. For a spin-1/2 system, he constructs analogous "probabilities" $f_{++}, f_{+-}, f_{-+}, f_{--}$ that satisfy:

- $\text{Prob(first index is +)} = f_{++} + f_{+-}$ ✓
- $\text{Prob(second index is +)} = f_{++} + f_{-+}$ ✓
- $\text{Prob(match)} = f_{++} + f_{--}$ ✓

All physical probabilities (sums of $f$'s) come out correct and non-negative. But the individual $f$'s can be negative. Feynman's example: $f_{++} = 0.6$, $f_{+-} = -0.1$, $f_{-+} = 0.3$, $f_{--} = 0.2$.

**The key statement:** "The only difference between a probabilistic classical world and the equations of the quantum world is that somehow or other it appears as if the probabilities would have to go negative, and that we do not know, as far as I know, how to simulate."

### 4. Bell's inequality makes it impossible (Sections 7–8)

To show this isn't just a mathematical inconvenience, Feynman gives the EPR-Bell argument. Two photons from an atomic cascade are measured through calcite crystals at angles $\phi_1$ and $\phi_2$. Quantum mechanics predicts:

$$P_{OO} = \tfrac{1}{2}\cos^2(\phi_2 - \phi_1)$$

For a local hidden-variable model: each photon must carry a deterministic instruction for every measurement angle (otherwise you couldn't predict one from the other). For six angles at $30°$ intervals, you can show that the probability of matching measurements at $30°$ separation is at most $2/3$ for any classical assignment. But quantum mechanics gives $\cos^2 30° = 3/4$.

"That's all. That's the difficulty. That's why quantum mechanics can't seem to be imitable by a local classical computer."

### 5. The proposal: quantum computers (Section 4)

Feynman proposes that a quantum computer — built from quantum mechanical elements obeying quantum mechanical laws — could efficiently simulate quantum systems. He conjectures:

1. Every finite quantum system can be described by a lattice of spin-1/2 systems (using the Pauli matrices $\sigma_x, \sigma_y, \sigma_z$, creation/annihilation operators $a, a^\dagger$)
2. A suitable class of quantum machines can imitate any quantum system
3. There should be a "universal quantum simulator" — the quantum analogue of a universal Turing machine

He's confident this works for bosonic systems. He's unsure about fermions ("I'm not sure whether Fermi particles could be described by such a system"). This uncertainty was resolved later by [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes|Abrams and Lloyd (1997)]].

---

## Historical significance

This paper did three things:

1. **Identified the exponential cost** of classical quantum simulation — not just as a practical difficulty, but as a structural consequence of quantum mechanics (negative Wigner function / Bell inequality violation)

2. **Proposed quantum computers** as the solution — the first articulation of the idea that quantum hardware is needed to simulate quantum physics

3. **Framed the research programme:** find the universal quantum simulator, understand the classes of quantum systems that are mutually simulatable

Feynman's conjecture that quantum computers can efficiently simulate local quantum systems was proved by [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd in 1996]] using [[Product-Formula Time-Slicing for Local Hamiltonians|product-formula time-slicing]]. The more general question — which quantum systems can be simulated efficiently, and with what resources — remains the central organising question of the field.

---

## Limits / caveats

- This is a vision paper, not a rigorous result. There are no formal definitions of "quantum computer" or "efficient simulation."
- Feynman's argument for classical intractability (negative Wigner function) is correct but informal. The formal version requires Bell inequality violations or, more precisely, contextuality results.
- The universal gate set question he raises was later answered by [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes|Deutsch (1985)]], who formalized the quantum Turing machine.
- Feynman's uncertainty about fermion simulation was resolved: the [[Jordan-Wigner Transformation for Chemistry Hamiltonians|Jordan-Wigner transformation]] maps fermions to spins.

---

## Reusable ideas

1. [[Negative Wigner Function as Classical Simulation Obstruction]] — The observation that quantum mechanics requires quasi-probabilities that go negative, and this negativity is precisely what prevents efficient classical simulation. This idea was later formalised and quantified in the resource theory of Wigner negativity.

---

## Key quotes

> "Nature isn't classical, dammit, and if you want to make a simulation of nature, you'd better make it quantum mechanical, and by golly it's a wonderful problem, because it doesn't look so easy."

> "The only difference between a probabilistic classical world and the equations of the quantum world is that somehow or other it appears as if the probabilities would have to go negative."

---

## References within this paper

- Ed Fredkin — inspired Feynman's interest in the connection between physics and computation
- Bennett, Fredkin, and Toffoli — reversible computation (proceedings of the same conference)
- Lloyd's 1995 PRL and Deutsch-Barenco-Ekert 1995 — universality of nonlinear two-body interactions (references [20, 21])
- Einstein-Podolsky-Rosen (1935) — the EPR paradox, which Feynman uses to demonstrate Bell inequality violation

---

## Cross-links

### Paper notes
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]] — proves Feynman's conjecture for local Hamiltonians
- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes]] — formalises the quantum computation model Feynman proposed informally
- [[Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012) — Paper Notes]] — extends quantum simulation to QFTs
- [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes]] — resolves Feynman's uncertainty about fermionic simulation

### Trick cards
- [[Negative Wigner Function as Classical Simulation Obstruction]]
