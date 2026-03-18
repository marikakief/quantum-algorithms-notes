> **Source:** Michael J. Bremner, Richard Jozsa, Dan J. Shepherd, *Classical simulation of commuting quantum computations implies collapse of the polynomial hierarchy*, Proc. R. Soc. A **467**, 459–472 (2011); arXiv:1005.1407
> **Links:** [arXiv](https://arxiv.org/abs/1005.1407) · [Proc. R. Soc. A](https://doi.org/10.1098/rspa.2010.0301)
> **Tags:** #quantum-supremacy #IQP #complexity-theory #polynomial-hierarchy #sampling #foundational

---

## The problem

Can the output distributions of quantum circuits built from only **commuting** gates be efficiently sampled classically? Such circuits — called IQP (Instantaneous Quantum Polynomial time) — are much weaker than universal quantum computation. They can't even do basic arithmetic. But can classical computers reproduce their sampling behaviour?

## What the paper does

Provides strong evidence that even these severely restricted quantum circuits produce output distributions that classical computers cannot efficiently sample. Specifically:

**Main result (Corollary 1):** If the output distributions of uniform IQP circuit families could be classically weakly simulated to within multiplicative error $c < \sqrt{2}$, then the polynomial hierarchy collapses to its third level ($\text{PH} = \Delta_3$).

This is widely regarded as implausible (comparable in spirit to $\text{P} = \text{NP}$), so the conclusion is that IQP circuits are likely classically hard to simulate — even approximately.

This paper is one of the founding results in the **quantum computational supremacy** programme. It showed that you don't need a full universal quantum computer to demonstrate a quantum advantage — even a very limited model of quantum computation (commuting gates only) likely suffices.

---

## IQP circuits

An IQP circuit on $n$ qubits has this structure:

$$|0\rangle^{\otimes n} \xrightarrow{H^{\otimes n}} \xrightarrow{D} \xrightarrow{H^{\otimes n}} \text{measure}$$

where $D$ is a product of gates that are all **diagonal in the $Z$ basis** (equivalently, all diagonal in the $X$ basis without the Hadamards). Because all gates in $D$ commute, the order doesn't matter — hence "instantaneous."

Equivalently: all gates are diagonal in the $X$ basis, applied between standard-basis input and computational-basis measurement. The gates can be arbitrary diagonal unitaries acting on $O(\log n)$ qubits each.

IQP circuits look a lot like the [[Hadamard Sandwich for Global Function Properties|Hadamard sandwich]] structure of Deutsch-Jozsa, but with more complex diagonal phases.

---

## The argument

### Step 1: post-IQP = PP (Theorem 1)

The key technical surprise. Add post-selection (condition on certain qubits measuring 0) to IQP circuits. Despite IQP being far weaker than BQP, post-selected IQP equals the classical class PP — the same as post-BQP (Aaronson 2005).

**The Hadamard gadget:** Any intermediate Hadamard gate can be replaced by a CZ gate, an ancilla, and a post-selection. Given a state $|\psi\rangle_a |0\rangle_e$:
1. Apply $H_a$, $\text{CZ}_{ae}$, $H_e$
2. Post-select qubit $a$ on outcome 0
3. The resulting state on line $e$ is $H|\psi\rangle$

This converts any universal quantum circuit (which uses intermediate Hadamards for non-commutativity) into an IQP circuit with post-selections. Since post-BQP = PP (Aaronson), we get post-IQP = PP.

### Step 2: Classical simulation → PH collapse (Theorem 2)

If IQP output distributions could be classically sampled with multiplicative error $c < \sqrt{2}$, then post-BPP = PP. Here's why:

- Take any language $L \in \text{post-IQP} = \text{PP}$, decided by post-selected IQP circuits $C_w$
- The post-selected probabilities involve ratios $S_w(x) = \Pr[O_w = x \wedge P_w = 0\ldots0] / \Pr[P_w = 0\ldots0]$
- A multiplicative approximation to the output distribution preserves these ratios up to factor $c^2$
- If $c^2 < 2$, the approximation still decides $L$ with bounded error → $L \in \text{post-BPP}$
- So post-BPP $\supseteq$ post-IQP = PP

Then by Toda's theorem ($\text{PH} \subseteq \text{P}^{\text{PP}}$) and the known bound $\text{P}^{\text{post-BPP}} \subseteq \Delta_3$:

$$\text{PH} \subseteq \text{P}^{\text{PP}} = \text{P}^{\text{post-BPP}} \subseteq \Delta_3$$

The polynomial hierarchy collapses.

---

## Other results

**Theorem 3:** If the IQP output register has only $O(\log n)$ lines, the distribution *can* be efficiently sampled classically. The hardness requires polynomially many output lines.

**Gate set:** It suffices to use only 1- and 2-qubit gates with diagonal entries that are integer powers of $e^{i\pi/8}$.

**Generality of the method:** The same argument applies to *any* circuit class whose post-selected version equals PP. This includes constant-depth quantum circuits (QNC$^0$) — so even depth-4 quantum circuits are likely classically hard to simulate.

---

## Key results

| Statement | Consequence |
|---|---|
| post-IQP = PP | Commuting gates + post-selection = full PP power |
| Classical weak sim. with mult. error $< \sqrt{2}$ for IQP | PH collapses to $\Delta_3$ |
| IQP with $O(\log n)$ output lines | Classically efficiently simulable |
| Method applies to any class $\mathcal{C}$ with post-$\mathcal{C}$ = PP | QNC$^0$, Clifford+$T$ restrictions, etc. |

---

## Historical context and impact

| Work | Contribution |
|---|---|
| Shepherd & Bremner (2009) | Introduced IQP; first complexity-theoretic study |
| Aaronson (2005) | post-BQP = PP |
| Terhal & DiVincenzo (2004) | Depth-4 circuits hardness via gate teleportation |
| **This paper (2011)** | **post-IQP = PP; Hadamard gadget; IQP hardness of sampling** |
| Aaronson & Arkhipov (2011) | BosonSampling — similar PH-collapse argument for linear optics |
| Bremner, Montanaro, Shepherd (2016) | Average-case hardness and noise tolerance for IQP |

This paper, together with Aaronson-Arkhipov's BosonSampling, launched the quantum supremacy programme — the idea that near-term quantum devices, even without full error correction, could demonstrate computational advantages via sampling problems.

---

## Limits / caveats

- **Multiplicative error tolerance is $c < \sqrt{2}$ (≈ 41%).** This is a specific noise model. Real devices have different error profiles (depolarising, readout errors) that don't map neatly to multiplicative error. The later work by Bremner, Montanaro & Shepherd (2016) addresses average-case and additive-error hardness.
- **The result is conditional** on PH not collapsing — which is widely believed but unproven.
- **IQP circuits are sub-universal** — they can't solve BQP-complete problems. The hardness is in *sampling*, not in *decision* problems.
- **$O(\log n)$ output lines** are efficiently simulable classically. The hardness specifically requires polynomially many output qubits.

---

## Reusable ideas

1. [[Hadamard Gadget for IQP Reduction]] — replace any intermediate Hadamard gate with a CZ, an ancilla, and a post-selection. Converts any quantum circuit into an IQP circuit at the cost of post-selections. The key to proving post-IQP = PP.
2. [[Post-Selection Boosting for Hardness of Sampling]] — show post-$\mathcal{C}$ = PP for your restricted circuit class $\mathcal{C}$; then any classical simulation of $\mathcal{C}$ implies post-BPP = PP, which collapses PH via Toda's theorem. A template reused in BosonSampling, random circuit sampling, and other supremacy arguments.

---

## References within this paper

- Shepherd & Bremner (2009) — introduced IQP (ref [4])
- Aaronson (2005) — post-BQP = PP (ref [6])
- Toda (1991) — $\text{PH} \subseteq \text{P}^{\text{PP}}$ (ref [9])
- Terhal & DiVincenzo (2004) — depth-4 circuit hardness (ref [13])
- [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes|Deutsch-Jozsa (1992)]] — the Hadamard sandwich structure that IQP generalises

---

## Cross-links

### Paper notes
- [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes]] — IQP circuits generalise the Deutsch-Jozsa structure ($H$-diagonal-$H$)
- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes]] — foundational computation model

### Trick cards
- [[Hadamard Gadget for IQP Reduction]]
- [[Post-Selection Boosting for Hardness of Sampling]]
- [[Hadamard Sandwich for Global Function Properties]] — the structure IQP generalises
- [[Phase Kickback from Oracle Queries]] — related phase-encoding technique
