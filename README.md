# Quantum Foundations

A working Obsidian vault for quantum algorithms: paper reconstruction notes, reusable technique cards, concept hubs, comparison tables, and coverage trackers.

Snapshot as of **2026-05-26**:

- **1,486 Markdown files total**
- **1,482 content notes**
  - **409 paper notes** in `Paper Summaries/`
  - **1,060 trick cards** in `Quantum Tricks/`
  - **6 concept hubs** in `Concept Hubs/`
  - **7 comparison tables** in `Comparison Tables/`
- **2 main trackers/indices**: `Paper Index.md` and `Quantum algorithm zoo.md`
- **Quantum Algorithm Zoo coverage**: **170 / 550 entries checked**
- **Algebraic / number-theoretic Zoo missing-paper batch**: **37 done**, **25 skipped for lack of usable arXiv/PDF**, **1 blocked book/umbrella entry**

This is not a textbook or a polished survey. It is a research knowledge base built for navigation, synthesis, and reconnecting ideas across the literature.

> **Open this in Obsidian.** The files are plain Markdown, but the point of the vault is the link structure: backlinks, hover previews, graph view, and local traversal through `[[wikilinks]]`.

## What is in the vault

### Paper notes
One note per paper where possible. These are closer to reconstruction notes than summaries: problem setting, main result, complexity statements, proof or construction skeleton, limitations, and links out to reusable techniques.

### Trick cards
Atomic technique notes extracted from papers. Each card is meant to answer: what is the idea, what does it buy, when should you use it, what does it cost, and what are its failure modes? These are the core of the vault.

### Concept hubs
Topic landing pages. Current hubs:

- `Amplitude Amplification and Estimation.md`
- `Hamiltonian simulation.md`
- `Hidden Subgroup Problem.md`
- `Lieb-Robinson bound.md`
- `Product Formulas.md`
- `Quantum Phase Estimation.md`

### Comparison tables
Cross-paper tables for resource estimates and method comparison. Current tables:

- `FeMoCo Resource Estimation Timeline.md`
- `Hamiltonian Simulation — Comparison Tables.md`
- `Hamiltonian Simulation — Time-Dependent Methods.md`
- `Quantum Chemistry — Circuit Primitives.md`
- `Quantum Chemistry — Fault-Tolerant Resource Estimates.md`
- `Quantum Chemistry — NISQ Experiments & VQE.md`
- `Quantum Chemistry — Technique Crosswalk.md`

### Indices and trackers

- `Paper Index.md` — topical browsing across paper notes. A paper may appear under multiple topics.
- `Quantum algorithm zoo.md` — coverage tracking against the Quantum Algorithm Zoo, with `✓` marking entries that have been processed into the vault.
- `zoo-algebraic-number-theoretic-missing-inventory.json` — batch inventory for the algebraic / number-theoretic Zoo catch-up work.

## Strongest current coverage

The vault is deep rather than uniform. The strongest areas are:

- **Hamiltonian simulation** — product formulas, randomized formulas, multiproduct formulas, LCU/Taylor/Dyson methods, interaction-picture simulation, QSP/QSVT/qubitization, sparse and local Hamiltonians, time-dependent and open-system simulation.
- **Quantum chemistry and electronic structure** — early chemistry algorithms, VQE-era experiments, FeMoCo resource estimates, circuit primitives, qubitization/tensor-factorization resource reductions, fermionic simulation and eigenstate preparation.
- **Linear systems and differential equations** — HHL and descendants, adiabatic QLSA, spectral/Chebyshev/Laplace/LCU methods, linear ODEs, nonlinear ODEs via Carleman linearisation, PDE caveats and lower bounds.
- **Quantum walks and query algorithms** — element distinctness, triangle finding, NAND/AND-OR trees, Markov-chain speedups, spatial search, backtracking, graph property testing, matrix product verification.
- **Amplitude amplification / estimation / search** — Grover and BBHT, minimum finding, fixed-point search, quantum counting, mean estimation, partition functions, finance/Monte Carlo applications.
- **Hidden subgroup / hidden shift / algebraic algorithms** — abelian HSP, selected nonabelian HSP work, hidden translation/coset methods, representation-theoretic multiplicities, Gauss sums, lattice and number-theoretic algorithms.
- **Complexity theory and verification** — BQP/QMA landmarks, communication complexity, nonlocal games, interactive-proof adjacent material, query lower bounds and adversary-style techniques.
- **Ground-state and eigenvalue methods** — phase estimation, eigenstate filtering, ground-state preparation, QET/QET-U, quantum subspace diagonalization, quantum Metropolis/Lindbladian preparation.
- **Quantum machine learning / variational methods** — selective, theory-heavy coverage: barren plateaus, trainability limits, quantum kernels, QBM/generative-learning notes where they connect to algorithmic questions.

## Sparse or intentionally limited coverage

Coverage is weaker for:

- quantum error correction outside fault-tolerance/resource-estimate context
- quantum networking and distributed systems
- experimental platforms unless they are algorithmically important
- quantum foundations/interpretations
- continuous-variable quantum computing beyond a few algorithmic PDE/linear-system notes
- QKD and cryptographic protocols unless they feed into algorithmic complexity or post-quantum cryptanalysis

## Folder structure

```text
Quantum Foundations/
├── Paper Summaries/          # 409 paper notes
├── Quantum Tricks/           # 1,060 reusable technique cards
├── Comparison Tables/        # 7 method/resource tables
├── Concept Hubs/             # 6 topic landing pages
├── Paper Index.md
├── Quantum algorithm zoo.md
├── zoo-algebraic-number-theoretic-missing-inventory.json
├── LICENSE.md
└── README.md
```

All notes are standard Markdown. Maths uses LaTeX (`$...$`, `$$...$$`). Links use Obsidian `[[wikilinks]]`.

## Best ways to navigate

- **Start with `Paper Index.md`** if you want a paper by area.
- **Start with a concept hub** for a topic-level overview.
- **Start with a trick card** if you remember the method but not the source paper.
- **Use backlinks** to move from a technique to every paper that uses it.
- **Use graph view** with folder filters to see the paper/trick structure.
- **Use `Quantum algorithm zoo.md`** when checking whether a Zoo entry has already been processed.

A useful graph filter is to colour by folder and view papers and tricks as a bipartite network: papers on one side, extracted methods on the other.

## Recommended Obsidian setup

### Core plugins

- **Backlinks**
- **Graph view**
- **Page preview**
- **Outline**
- **Tags**

### Useful community plugins

- **Dataview**
- **Graph Analysis**

### Settings

- **Use `[[Wikilinks]]`** → ON
- **Readable line length** → ON
- **Show frontmatter** → OFF for reading

## What this vault is and is not

### It is

- a linked literature knowledge base
- a place to extract reusable algorithmic ideas from papers
- a way to recover context quickly without reopening every PDF
- a map of how papers, subroutines, reductions, and proof tricks relate

### It is not

- **complete** — coverage reflects reading priorities
- **error-free** — exact bounds, assumptions, and proof details can be wrong
- **stable** — notes are revised and links may occasionally lag behind
- **introductory** — assumes graduate-level quantum computing background
- **a substitute for the source papers** — use it to orient and then verify

## AI disclosure

The vault was built with assistance from multiple frontier AI systems over time, then directed and spot-checked by a human curator. It was not written fully by hand, and it should not be treated as a substitute for the papers themselves.

If something important depends on an exact theorem statement, complexity bound, or hidden assumption, verify against the source.

## Licence

The vault now has a dedicated `LICENSE.md` file.

Unless stated otherwise, the original contents of this vault are licensed under **Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)**.

In plain English: you are welcome to share it, reuse it, and build on it, as long as you credit the source and are not using it commercially.

Third-party material — paper excerpts, abstracts, figures, PDFs, publisher content, and bibliographic metadata — remains with the original rights holders.

## Maintenance note

The counts above are a snapshot. If they drift, trust the folder contents and `Quantum algorithm zoo.md` over this README.
