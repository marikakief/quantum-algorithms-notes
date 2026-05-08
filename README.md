# Quantum Foundations

A zettelkasten-style knowledge base for quantum algorithms, built as an [Obsidian](https://obsidian.md) vault of interconnected Markdown files. ~1,200 notes covering ~340 papers and ~850 reusable technique cards, heavily cross-linked with `[[wikilinks]]`.

Started in early 2026 as a way to get back into the literature after a research hiatus. It has since become a fairly large working knowledge base rather than a small side project.

> **Open this in Obsidian.** The notes are plain Markdown, but the value is in the link structure — graph view, backlinks, and hover previews make the connections between papers visible. Reading these as flat files works, but you'll miss the point.

## What's in here

The vault has two primary note types, connected by wikilinks:

**Paper notes** (~340) — one per paper. Each covers: the problem being solved, main results with exact complexity statements, the full algorithm or construction, key theorems, comparison with prior work, limitations, and links to extracted techniques. These aren't summaries — they're detailed enough to reconstruct the main argument without reopening the PDF.

**Trick cards** (~850) — atomic, reusable technique notes. Each explains a single idea: what it does, how it works, when to reach for it, what it costs, and where it breaks. They link back to their source papers and forward to related techniques. This is the zettelkasten layer — ideas separated from the papers they came from, available to recombine.

The trick cards are the heart of the vault. A technique like "block encoding via LCU" or "Chebyshev spectral discretisation" shows up across many papers. Extracting it into a standalone card and linking it everywhere it appears turns a collection of paper summaries into a network of ideas instead of a long shelf of disconnected notes.

**Also:**
- **Concept hubs** (6) — landing pages for major topics: Hamiltonian simulation, product formulas, quantum phase estimation, amplitude amplification, hidden subgroup problem, Lieb-Robinson bounds. Define the concept, link out to relevant papers and tricks.
- **Comparison tables** (7) — resource estimates and method comparisons for quantum chemistry (NISQ experiments, fault-tolerant estimates, circuit primitives, technique crosswalk) and Hamiltonian simulation (time-dependent methods, general comparison).
- **Paper Index** — all papers organised by topic (~30 sections).
- **Quantum Algorithm Zoo tracker** — marks which zoo entries have vault notes.

## Topic coverage

The coverage reflects what I was reading, not a systematic survey. Strongest areas:

- **Hamiltonian simulation** — product formulas (Trotter error theory, randomised/multi-product/corrected formulas), LCU methods, QSP/QSVT, Taylor series, Dyson series, interaction picture. The densest cluster.
- **Quantum chemistry** — resource estimation pipeline from VQE through fault-tolerant (FeMoCo, materials, catalysis). Includes a FeMoCo resource estimation timeline tracking the drop from ~10¹⁴ to ~10⁹ Toffolis.
- **Linear systems and differential equations** — HHL through Carleman linearisation, spectral methods, LCHS for non-unitary dynamics. Includes the negative results (Linden-Montanaro-Shao).
- **Complexity theory** — QMA-completeness, BQP, quantum interactive proofs, query complexity lower bounds.
- **Quantum walks** — Szegedy, MNRS, element distinctness, formula evaluation, walk search on arbitrary graphs.
- **Quantum machine learning** — Boltzmann machines, barren plateaus, topological data analysis.
- **State preparation** — adiabatic, Lindbladian, spectral amplification methods.
- **Foundational** — Grover, Shor-era results, communication complexity, quantum information theory.

Weaker or absent: quantum error correction (beyond fault-tolerance theory), quantum networking, experimental implementations (beyond chemistry benchmarks), continuous-variable QC.

## Structure

```
Quantum Foundations/
├── Paper Summaries/       (~340 paper notes)
├── Quantum Tricks/        (~850 technique cards)
├── Comparison Tables/     (7 resource/method comparison tables)
├── Concept Hubs/          (6 topic overview pages)
├── Paper Index.md         (topical index, ~30 sections)
├── Quantum algorithm zoo.md
└── README.md
```

All files are standard Markdown. Maths uses `$...$` and `$$...$$` (LaTeX). Links use Obsidian `[[wikilinks]]`. Some notes use tags, but the vault is navigated mainly through links, the Paper Index, hubs, and backlinks rather than a frontmatter-heavy metadata scheme.

## Navigating

**Start with a concept hub** if you want an overview of a topic area. They define the concept and link to relevant papers and tricks.

**Start with the Paper Index** if you're looking for a specific paper or want to browse by topic.

**Start with a trick card** if you remember a technique but not which paper it came from. The backlinks panel shows every paper that uses it.

**Use graph view** filtered by folder (`path:Paper Summaries` or `path:Quantum Tricks`) to see the link structure. Colour by folder to see the bipartite structure: papers on one side, tricks on the other, with links crossing between them.

## Recommended Obsidian setup

### Core plugins (built-in)

- **Backlinks** — which notes link to the current one. Essential for trick cards. Enable "Backlinks in document."
- **Graph view** — the link structure visualised. Filter by path or tag to see topic clusters.
- **Page preview** — hover over a wikilink to see the target note inline.
- **Outline** — table of contents for long paper notes.
- **Tags** — browse by topic tag.

### Community plugins (recommended)

- **Dataview** — query the vault like a database ("list all papers by Berry sorted by year").
- **Graph Analysis** — find clusters, shortest paths, hub identification.

### Settings

- **Use [[Wikilinks]]** → ON
- **Readable line length** → ON
- **Show frontmatter** → OFF for reading

## On "zettelkasten"

This borrows from the zettelkasten method — particularly the idea of atomic notes (trick cards) that exist independently of their source and can be linked into new contexts. But it's more structured than a pure zettelkasten: paper notes are organised by folder and indexed by topic, not just linked freely. Think of it as zettelkasten principles applied to a literature review, with the trick cards as the permanent notes and the paper summaries as the literature notes.

## AI disclosure

Notes in this vault were produced with help from multiple frontier AI systems over time, not a single model. Different parts of the collection were generated, expanded, or revised using different frontier AIs, then directed and spot-checked by me. I did not write the whole vault by hand.

Errors are possible — especially on exact complexity bounds, technical assumptions, and proof details. If you find one, open an issue.

Read the actual papers. These notes are for navigation, synthesis, and connection-building, not as a substitute for the source material.

## What this isn't

- **Complete.** Coverage reflects what I was working on. Major areas are missing.
- **Error-free.** AI-generated, spot-checked. Complexity bounds may be wrong.
- **A textbook.** Assumes graduate-level quantum computing background.
- **Stable.** Notes get added and revised regularly. Cross-links may dangle temporarily.
- **Uniform.** The style is broadly consistent, but the vault was built over time and different note batches reflect slightly different drafting workflows.

## Current scale

Current snapshot of the `Quantum Foundations` folder:
- **~1,200 Markdown notes total**
- **344 paper notes**
- **847 trick cards**
- **6 concept hubs**
- **7 comparison tables**

If these numbers drift again, trust the folder contents over the README.
