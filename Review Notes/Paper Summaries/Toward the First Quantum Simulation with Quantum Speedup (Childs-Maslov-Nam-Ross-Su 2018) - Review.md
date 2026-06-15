# Review: Toward the First Quantum Simulation with Quantum Speedup (Childs-Maslov-Nam-Ross-Su 2018)

Source note: [[Toward the First Quantum Simulation with Quantum Speedup (Childs-Maslov-Nam-Ross-Su 2018) — Paper Notes]]

## Primary Sources Checked

- Childs, Maslov, Nam, Ross, and Su, "Toward the first quantum simulation with quantum speedup," Proceedings of the National Academy of Sciences 115(38), 9456-9461 (2018), arXiv:1711.10980.

## Verdict

Major factual issues. The source note appears to conflate this PNAS paper with FeMoco/quantum-chemistry resource estimates. The paper's main target is optimized circuit/resource analysis for quantum simulation of spin systems, comparing product formulas, truncated Taylor series, and QSP; it mentions quantum chemistry as a harder benchmark but is not primarily a FeMoco phase-estimation estimate.

## Line-Anchored Comments

- Lines 5-18: this opening is mostly about FeMoco quantum chemistry, but the paper itself is about finding low-resource quantum simulation tasks, especially spin-system simulation, and comparing concrete circuit implementations. This section needs rewriting.
- Lines 7-9: FeMoco ground-state energy via second-quantized chemistry is not the central computational problem of this paper. That description belongs closer to Reiher et al. 2017 or later FeMoco resource-estimation notes.
- Lines 15 and 41: "54 spin-orbitals" is almost certainly wrong for the FeMoco active-space context; the common FeMoco active space is 54 spatial orbitals / 108 spin orbitals. More importantly, this is not the headline system in the Childs-Maslov-Nam-Ross-Su paper.
- Lines 16-18: the paper does optimize product formulas, but it also gives detailed circuit/resource comparisons for truncated Taylor series and quantum signal processing. The note narrows it too much to Trotterized quantum chemistry.
- Lines 22-39: the product-formula description is generically reasonable, but it does not summarize the paper's main comparative circuit-synthesis work.
- Lines 40-45: the concrete FeMoco numbers are not the right "for this paper specifically" facts. Replace with the paper's spin-chain/system-size resource tables and QSP-vs-PF conclusions.
- Lines 48-52: the "main estimate" and "quantum speedup regime" bullets should be removed or moved to a different chemistry resource-estimation note. The PNAS abstract says QSP appears preferred among rigorous algorithms, while higher-order product formulas prevail under empirical error estimates.
- Lines 55-62: the comparison table is not reliable as a summary of this paper because it mixes chemistry estimates and generic Hamiltonian-simulation asymptotics.
- Lines 68-72: some caveats are sensible in a general chemistry resource estimate, but they are not the right caveats for this paper. The better caveats are empirical versus rigorous error estimates, architecture/compiler assumptions, and the limited sense in which the chosen spin-system tasks demonstrate "first speedup."
- Lines 80-87: the references are related, but the note should cite the paper's actual implementation/circuit-count machinery and the Quipper/simcount outputs.

## Missing or Suggested Additions

- Rewrite around the actual paper structure: candidate early quantum-simulation tasks, circuit synthesis for PF/TS/QSP, concrete T/CNOT counts for 10-100 spin systems, and the empirical-vs-rigorous conclusion.
- If the vault wants a FeMoco resource-estimation note, keep it separate and link it to Reiher et al. 2017, Babbush et al., and later qubitization/resource-estimate papers.

