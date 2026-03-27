
> **Source:** Goings, White, Lee, Tautermann, Degroote, Gidney, Shiozaki, Babbush, Rubin, arXiv:2202.01244; see also Lee and Head-Gordon (2019)
> **Tags:** #trick #quantum-chemistry #multireference #diagnostics #spin-contamination #coupled-cluster

## What it does

Distinguishes two types of spin symmetry breaking in mean-field wavefunctions: "artificial" breaking that disappears when you use better reference orbitals (and thus doesn't indicate strong correlation), versus "essential" breaking that persists regardless of reference and genuinely signals multiconfiguational character.

## The trick

Standard unrestricted Hartree-Fock (UHF) often produces spin-contaminated solutions ($\langle S^2 - S_z^2 - S_z \rangle \neq 0$) for open-shell transition metal complexes. This contamination is commonly taken as evidence of multireference character. But much of it is an artifact of HF's inability to describe dynamic correlation.

**The diagnostic procedure:**

1. Run UHF and compute spin contamination and max(|t₁|) from CCSD
2. Switch reference orbitals to Kohn-Sham DFT (e.g., B3LYP, BP86) and repeat
3. If spin contamination and large t₁ values disappear → **artificial** symmetry breaking. The system is single-reference; HF just gave a bad reference.
4. If they persist → **essential** symmetry breaking. The system likely has genuine multiconfiguational character.

Complement with max(|t₂|) from CCSD (insensitive to reference choice) and κ-OOMP2 natural orbital occupation numbers (NOONs). The t₂ diagnostic is more reliable than t₁ because doubles amplitudes capture genuine pairwise correlation, not orbital rotations.

**Threshold guidance:** max(|t₂|) > 0.1 is a reliable indicator of strong correlation. Values around 0.05 are borderline. Large max(|t₁|) alone (without large max(|t₂|)) likely indicates a poor reference, not strong correlation.

## When to reach for it

- Evaluating whether a transition metal complex needs multireference treatment before committing to expensive CASSCF/DMRG calculations
- Assessing quantum advantage claims: if a system is classified as "strongly correlated" based only on HF spin contamination, the claim may be overblown
- Interpreting symmetry-broken DFT results for spin-gap calculations
- Any situation where you need to justify the choice between single-reference and multireference methods

## Complexity

Negligible beyond the CCSD calculations you'd probably run anyway. The key cost is running CCSD with multiple reference orbital sets (UHF, UB3LYP, UBP86), which is just three CCSD runs.

## Caveat

This is a set of heuristics, not a theorem. A system can have essential symmetry breaking in the HF solution but still be tractable by single-reference coupled cluster at sufficiently high order. Conversely, the Cpd I doublet in the CYP study has modest max(|t₂|) ≈ 0.05 (below the strong correlation threshold) but three open-shell NOONs indicating triradical character — a case where no single diagnostic tells the full story. Always use multiple diagnostics together.

The κ-OOMP2 NOONs add genuine information beyond t₁/t₂: they reveal polyradical character even when the system isn't "strongly correlated" in the many-body entanglement sense.

## Related notes
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]]
- [[EQA Assessment Framework (State Preparation vs Classical Heuristic)]]
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]]
