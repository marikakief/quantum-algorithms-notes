
> **Source:** Bremner, Jozsa, Shepherd (2011); [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes|Aaronson & Arkhipov (2011)]]; general technique
> **Tags:** #trick #complexity-theory #quantum-supremacy #sampling #polynomial-hierarchy

## What it does
Proves that classically sampling from a quantum circuit class $\mathcal{C}$ is hard, by showing that $\mathcal{C}$ with post-selection equals PP, then using Toda's theorem to derive a PH collapse.

## The trick

The argument has three steps:

**Step 1:** Show post-$\mathcal{C}$ = PP. This means that if you allow post-selection (conditioning on certain measurement outcomes), then even the restricted circuit class $\mathcal{C}$ becomes as powerful as PP for decision problems.

**Step 2:** Assume for contradiction that the output distributions of $\mathcal{C}$ circuits can be classically sampled with multiplicative error $c < \sqrt{2}$. Then any post-$\mathcal{C}$ computation can be simulated by a post-BPP computation (the multiplicative error preserves the ratios in post-selected probabilities up to factor $c^2 < 2$, which still allows bounded-error decisions).

**Step 3:** By Step 1, post-BPP $\supseteq$ post-$\mathcal{C}$ = PP. Then Toda's theorem ($\text{PH} \subseteq \text{P}^{\text{PP}}$) and the known bound $\text{P}^{\text{post-BPP}} \subseteq \Delta_3$ give PH $\subseteq \Delta_3$. The polynomial hierarchy collapses — widely regarded as implausible.

Conclusion: classically sampling $\mathcal{C}$ with multiplicative error $< \sqrt{2}$ is likely impossible.

## When to reach for it
- Proving hardness of classical simulation for any restricted quantum circuit class (IQP, BosonSampling, random circuits, Clifford+$T$, etc.)
- The template Step 1 → Step 2 → PH collapse has been reused in essentially every quantum supremacy argument
- When you want to argue that a quantum device does something a classical computer can't, without proving $\text{BQP} \neq \text{BPP}$

## Complexity
The argument is non-constructive — it doesn't tell you *which* instances are hard, only that efficient classical simulation of *all* instances would have implausible complexity-theoretic consequences.

## Caveat
- The hardness is for **multiplicative error** sampling. Real-world quantum devices have **additive** noise, which is a different (and harder to handle) error model. Extending to additive error requires additional conjectures.
- The result is conditional on PH not collapsing — widely believed but unproven.
- Only applies to **sampling** hardness, not **decision** problems. BQP $\neq$ BPP remains open.

## Related notes
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes]] — first use for IQP
- [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes]] — BosonSampling; uses this template with PostBQP = PP via KLM
- [[Hadamard Gadget for IQP Reduction]] — the tool for Step 1 in the IQP case
