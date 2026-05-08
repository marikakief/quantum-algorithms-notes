# Quantum Foundations

A working Obsidian vault for quantum algorithms: interconnected Markdown notes on papers, reusable technique cards, concept hubs, and comparison tables.

Current snapshot: **1,207 notes total**
- **344 paper notes**
- **847 trick cards**
- **6 concept hubs**
- **7 comparison tables**

This is not a textbook or a polished survey. It is a research knowledge base built for navigation, synthesis, and reconnecting ideas across the literature.

> **Open this in Obsidian.** The files are plain Markdown, but the point of the vault is the link structure: backlinks, hover previews, graph view, and local traversal through `[[wikilinks]]`.

## What is in the vault

### Paper notes
One note per paper. These are closer to reconstruction notes than summaries: problem setting, main result, complexity statements, proof or construction skeleton, limitations, and links out to reusable techniques.

### Trick cards
Atomic technique notes extracted from papers. Each card is meant to answer: what is the idea, what does it buy, when should you use it, what does it cost, and what are its failure modes? These are the core of the vault.

### Concept hubs
Landing pages for major topics such as Hamiltonian simulation, product formulas, phase estimation, amplitude amplification, hidden subgroup problem, and Lieb-Robinson bounds.

### Comparison tables
Cross-paper tables for resource estimates and method comparison, especially around Hamiltonian simulation and quantum chemistry.

### Indices and trackers
- **Paper Index.md** for topical browsing
- **Quantum algorithm zoo.md** for coverage tracking against the Quantum Algorithm Zoo

## Strengths of the current coverage

The vault is deep rather than uniform. Strongest areas:

- **Hamiltonian simulation** — product formulas, randomization, QSP/QSVT, Taylor/Dyson/interaction-picture methods
- **Quantum chemistry** — from VQE-era methods to fault-tolerant resource estimates
- **Linear systems and differential equations** — HHL, spectral methods, Carleman linearisation, non-unitary dynamics
- **Complexity theory** — QMA-completeness, BQP, query lower bounds, interactive proofs
- **Quantum walks** — Szegedy/MNRS, search, element distinctness, formula evaluation
- **Quantum machine learning** — selected theory-heavy notes rather than broad application coverage
- **State preparation and energy estimation**
- **Foundational algorithm papers** — Grover/Shor-era results and adjacent landmarks

Weaker or sparse: error correction beyond fault-tolerance theory, networking, experimental systems work, and continuous-variable quantum computing.

## Folder structure

```text
Quantum Foundations/
├── Paper Summaries/
├── Quantum Tricks/
├── Comparison Tables/
├── Concept Hubs/
├── Paper Index.md
├── Quantum algorithm zoo.md
└── README.md
```

All notes are standard Markdown. Maths uses LaTeX (`$...$`, `$$...$$`). Links use Obsidian `[[wikilinks]]`.

## Best ways to navigate

- **Start with a concept hub** for a topic-level overview.
- **Start with Paper Index.md** if you want a paper by area.
- **Start with a trick card** if you remember the method but not the source paper.
- **Use backlinks** to move from a technique to every paper that uses it.
- **Use graph view** with folder filters to see the paper/trick structure.

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

### It is not
- **complete** — coverage reflects reading priorities
- **error-free** — exact bounds, assumptions, and proof details can be wrong
- **stable** — notes are revised and links may occasionally lag behind
- **introductory** — assumes graduate-level quantum computing background

## AI disclosure

The vault was built with assistance from multiple frontier AI systems over time, then directed and spot-checked by a human curator. It was not written fully by hand, and it should not be treated as a substitute for the papers themselves.

If something important depends on an exact theorem statement, complexity bound, or hidden assumption, verify against the source.

## Maintenance note

If the counts above drift, trust the folder contents over this README.
