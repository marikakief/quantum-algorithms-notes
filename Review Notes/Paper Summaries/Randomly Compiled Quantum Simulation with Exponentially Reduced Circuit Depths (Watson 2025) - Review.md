# Review: Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025)

Source note: [[Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025) — Paper Notes]]

## Primary Sources Checked

- Watson, "Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths," arXiv:2411.04240.

## Verdict

Major issues. The qFLO/Richardson story is mostly right, but the note contains a serious reference misidentification and a few cost-model statements that should be tightened.

## Line-Anchored Comments

- Lines 9-15: good one-line summary: qFLO is qDRIFT plus Richardson extrapolation for observable estimation, not a coherent state-output simulator.
- Lines 21-23: `lambda` and Hamiltonian normalization should be stated in the headline scaling. The arXiv abstract often suppresses this by assuming normalized units, but the vault should not.
- Lines 61 and 67-77: the distinction between maximum circuit depth, number of step sizes, shot count, and total gate count must remain explicit. A logarithmic-depth systematic-error improvement is traded for `O(1/epsilon^2)` sampling.
- Lines 83-90: the comparison table says randomized multiproduct formulas have `O(1)` shot cost and describes them as coherent. That is misleading. Faehrmann et al. is also an observable-estimation/randomized-sampling approach with a resolution-factor-dependent shot overhead; coherent LCU is the deterministic MPF alternative.
- Line 94: "done coherently" is not the right description of the randomized MPF observable-estimation protocol.
- Line 129: "LKW19 (Lin-Tong-Wu 2019 or similar)" is wrong. The conditioning result being invoked is Low-Kliuchnikov-Wiebe 2019, already present in the vault as [[Well-Conditioned Multiproduct Hamiltonian Simulation (Low-Kliuchnikov-Wiebe 2019) — Paper Notes]].

## Missing or Suggested Additions

- Add a small resource table with separate columns for maximum depth, number of distinct step sizes, shots, and whether the output is a state or an expectation value.
