# quantum-algorithms-notes

A personal Obsidian knowledge base for quantum algorithms, complexity, and related theory. Started in early 2026 as a way to get back into the literature after a research hiatus.

## What this is

Structured notes on quantum algorithms papers, built around a network of paper summaries and reusable "trick cards" — atomic technique notes that can be linked across papers. The goal is a graph of connected ideas rather than a flat list of summaries.

Paper notes cover the problem being solved, the main result, the algorithm or construction in detail, key theorems with exact complexity bounds, comparison with prior work, limitations, and reusable techniques. Trick cards are standalone explanations of a single idea — a technique, construction, or proof method that can be applied in different contexts. Everything is cross-linked.

The collection skews toward quantum algorithms (Hamiltonian simulation, quantum walk, linear systems, ground state preparation), complexity (QMA, BQP), and quantum machine learning. Coverage is uneven and reflects what I was thinking about at the time, not a systematic survey of the field.

## What this isn't

- **Complete.** Papers were added based on whatever I was reading or thinking about at the time. Large and important areas are missing. If your paper isn't here, it's not a comment on its quality.

- **Error-free.** Notes were written by AI and spot-checked by me. I can't guarantee every complexity bound, proof sketch, or cross-reference is correct. If you find an error, open an issue or email me.

- **A reference.** Read the actual papers. These notes are for navigation and connection-building, not as a substitute for the source material.

## Structure

```
Quantum Foundations/
├── Paper Summaries/                          — one note per paper
├── Quantum Tricks/                           — atomic reusable technique cards
└── Hamiltonian Simulation — Comparison Tables.md
```

**Paper Summaries** follow a consistent format: problem statement, main result, algorithm/construction, key theorems, comparison table with prior work, limitations, reusable ideas (linking to trick cards), and cross-references to related notes.

**Quantum Tricks** are self-contained technique cards. Each one explains a single reusable idea — what it does, how it works, when to use it, what it costs, and where it breaks. They link back to the papers they came from and forward to related techniques.

## AI disclosure

Notes were written by Claude (AI), prompted and directed by me. I read the papers, specified what to include, and spot-checked the output — but I didn't write the prose. Errors are possible, particularly on complexity bounds and proof details. If you find one, open an issue.


