# quantum-algorithms-notes

A personal Obsidian knowledge base for quantum algorithms, complexity, and related theory. Started in early 2026 as a way to get back into the literature after a research hiatus.

> **Best experience: open this vault in [Obsidian](https://obsidian.md).** The notes are heavily cross-linked with `[[wikilinks]]` — reading them as plain markdown files works, but you lose the graph view, backlinks panel, and hover previews that make the connections between papers visible. See [Recommended Obsidian Setup](#recommended-obsidian-setup) below.

## What this is

Structured notes on quantum algorithms papers, built around a network of paper summaries and reusable "trick cards" — atomic technique notes that can be linked across papers. The goal is a graph of connected ideas rather than a flat list of summaries.

Paper notes cover the problem being solved, the main result, the algorithm or construction in detail, key theorems with exact complexity bounds, comparison with prior work, limitations, and reusable techniques. Trick cards are standalone explanations of a single idea — a technique, construction, or proof method that can be applied in different contexts. Everything is cross-linked.

The collection skews toward quantum algorithms ([[Hamiltonian simulation]], quantum walk, linear systems, ground state preparation), complexity (QMA, BQP), and quantum machine learning. Coverage is uneven and reflects what I was thinking about at the time, not a systematic survey of the field.

## What this isn't

- **Complete.** Papers were added based on whatever I was reading or thinking about at the time. Large and important areas are missing. If your paper isn't here, it's not a comment on its quality.

- **Error-free.** Notes were written by AI and spot-checked by me. I can't guarantee every complexity bound, proof sketch, or cross-reference is correct. If you find an error, open an issue or email me.

- **A reference.** Read the actual papers. These notes are for navigation and connection-building, not as a substitute for the source material.

## Structure

```
Quantum Foundations/
├── Paper Summaries/       (~187 paper notes)
├── Quantum Tricks/        (~513 technique cards)
├── Comparison Tables/     (resource estimate & method comparison tables)
│   ├── Quantum Chemistry — NISQ Experiments & VQE.md
│   ├── Quantum Chemistry — Fault-Tolerant Resource Estimates.md
│   ├── Quantum Chemistry — Circuit Primitives.md
│   ├── Quantum Chemistry — Technique Crosswalk.md
│   └── Hamiltonian Simulation — Time-Dependent Methods.md
├── Concept Hubs/          (topic overview pages)
├── Paper Index.md         (topical index)
├── Hamiltonian Simulation — Comparison Tables.md  (hub → links to Comparison Tables/)
├── FeMoCo Resource Estimation Timeline.md
├── Quantum algorithm zoo.md
└── README.md
```

**Paper Summaries** — one note per paper, covering: problem statement, main result, algorithm or construction in detail, key theorems with exact complexity bounds, comparison with prior work, limitations, reusable ideas (linking to trick cards), and cross-references to related notes.

**Quantum Tricks** — self-contained technique cards. Each one explains a single reusable idea: what it does, how it works, when to use it, what it costs, and where it breaks. They link back to the papers they came from and forward to related techniques.

**Comparison Tables** — resource estimate and method comparison tables, split by topic. The hub note [[Hamiltonian Simulation — Comparison Tables]] links to all five sub-notes. Includes NISQ experiments, fault-tolerant resource estimates (FeMoCo, CYP P450, Diamond, LNO, stopping power), circuit primitives (product formulas, Trotter depth, state preparation), a technique crosswalk mapping tricks to papers, and time-dependent simulation methods.

**Concept Hubs** — topic overview pages for heavily-linked concepts (e.g., [[Hamiltonian simulation]], [[Product Formulas]]s, amplitude amplification). These serve as landing pages — they define the concept briefly and link out to the relevant paper notes and trick cards. Good starting points for exploring a topic cluster.

**[[Paper Index]]** — all paper notes organised by topic.

**[[FeMoCo Resource Estimation Timeline]]** — standalone synthesis note tracking how FeMoCo quantum resource estimates dropped from ~10¹⁴ to ~10⁹ Toffolis across 8 years of algorithmic improvements.

## Recommended Obsidian Setup

### Core plugins (built-in, just enable them)

- **Backlinks** — shows which notes link to the current note. Essential for trick cards (you can see every paper that uses a technique). Enable "Backlinks in document" to see them inline.
- **Graph view** — visualise the link structure. Try filtering to just `path:Paper Summaries` or `path:Quantum Tricks` to see clusters. The [[Hamiltonian simulation]] cluster is particularly dense.
- **Page preview** — hover over a `[[wikilink]]` to see the target note without leaving the current one. Very useful for quickly checking a trick card while reading a paper note.
- **Outline** — table of contents for long paper notes.
- **Search** — Obsidian's search handles `[[wikilinks]]` natively and can filter by path, tag, etc.
- **Tags** — paper notes use tags like `#hamiltonian-simulation`, `#quantum-chemistry`, `#product-formulas`. Enable the tag pane to browse by topic.

### Community plugins (recommended)

- **Dataview** — query your vault like a database. Useful for things like "list all papers by Berry sorted by year" or "show all trick cards tagged #block-encoding". Install from Community Plugins → Browse.
- **Latex Suite** — if you want to edit the maths. The notes use `$...$` and `$$...$$` extensively. This plugin gives you snippets and auto-completion for LaTeX.
- **Admonition / Callouts** — some notes use `> [!note]` and `> [!warning]` callout blocks. Obsidian renders these natively since v0.14, but a callout plugin can add more types.
- **Graph Analysis** — goes beyond the built-in graph view. Can find clusters, shortest paths between papers, and identify hub notes. Good for seeing which trick cards are most connected.
- **Local Graph** — shows the immediate neighbourhood of the current note. Faster than the full graph for seeing what's connected to what you're reading.

### Settings worth changing

- **Files & Links → Default location for new notes** → set to vault root (so new notes don't end up in a random subfolder)
- **Files & Links → Use [[Wikilinks]]** → make sure this is ON (it should be by default)
- **Editor → Readable line length** → ON (the paper notes can get wide)
- **Editor → Show frontmatter** → OFF (cleaner reading experience; the metadata is there if you need it)

### Graph view tips

The vault has 680+ notes, so the full graph is dense. Try these filters:
- `path:Paper Summaries` — just the paper network
- `path:Quantum Tricks` — just the technique network
- `tag:#hamiltonian-simulation` — one topic cluster
- Depth slider → 1 or 2 for local exploration

Colour groups by folder (`Paper Summaries` vs `Quantum Tricks`) to see the bipartite structure: papers on one side, tricks on the other, with links crossing between them.

## AI disclosure

Notes were written by Claude (AI). I directed what to include and spot-checked the output, but I didn't write them. Errors are possible, particularly on complexity bounds and proof details. If you find one, open an issue.


