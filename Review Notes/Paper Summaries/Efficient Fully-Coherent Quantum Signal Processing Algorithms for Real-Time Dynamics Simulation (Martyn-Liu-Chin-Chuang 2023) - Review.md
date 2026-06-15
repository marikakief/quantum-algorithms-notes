# Review: Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023)

Source note: [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/2110.11327
- JCP DOI linked in the source note.

## Verdict

Minor issues. The note is mostly accurate and useful. The main fixes are nuance: direct QSP's parity obstruction is a constraint on one-shot single-polynomial implementation, not an impossibility of QSP-based simulation, and the theorem statement should keep global phase, normalization, and success/failure parameters explicit.

## Line-Anchored Comments

- Lines 10-14: The fully-coherent interface is explained well. Add that "single copy of the input state" is part of the paper's definition of fully coherent; this distinguishes it from repeat-until-success/postselection approaches.

- Lines 26-30: The three-way comparison is good. For the first two constructions, state whether the LCU success amplitude is exactly or approximately known, since this is the reason robust OAA is needed.

- Lines 66-69: The parity obstruction is slightly overstated. It blocks direct implementation of the full complex exponential as a single ordinary QSP response polynomial under the chosen parity convention; it does not mean QSP cannot simulate Hamiltonian evolution, since cosine/sine splitting, qubitization, QET variants, and LCU constructions exist.

- Lines 74-78: The affine transform should carry normalization explicitly: the block-encoding is of a shifted/rescaled version of `H/alpha`, and the final simulation time is adjusted accordingly.

- Lines 82-90: The EECE explanation is clear. Add that the recovered evolution may include a known global phase from the shift; this is harmless but should not be hidden when writing "equals `e^{-iHt}`."

- Lines 92-95: The arXiv abstract states query complexity additive in time, `ln(1/epsilon)`, and `ln(1/delta)`. Use the paper's norm convention, e.g. `||mathcal H|| |t|`, or explicitly identify it with `alpha |t|` under a block-encoding normalization.

- Lines 121-126: The comparison with qubitization should avoid implying qubitization is not fully coherent. The paper is solving a particular fully-coherent construction/interface issue and giving a one-shot QSP route with clean parameter dependence.

## Missing or Suggested Additions

- Add a "when to use this" note: one-shot fully coherent simulation is especially relevant when the simulation block is later controlled, concatenated, or inserted into a larger coherent algorithm.

- Add a short note distinguishing approximation error `epsilon` from failure probability `delta`, since the paper's contribution is partly the independent/additive treatment of these parameters.

