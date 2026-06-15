# Review: Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022)

Source note: [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]]

## Primary Sources Checked

- Goings et al., "Reliably assessing the electronic structure of cytochrome P450 on today's classical computers and tomorrow's quantum computers", arXiv:2202.01244 / PNAS 119 (2022).

## Verdict

Major issues. Much of the note is good, but the state-preparation conclusion overstates what the P450 data show.

## Line-Anchored Comments

- Lines 17-19: the active-space/runtime framing is good, but make explicit that the advantage claim is against the paper's DMRG baselines and hardware assumptions, not all possible classical workflows.
- Line 25: the "more than 70% of marketed drugs" statement should be attributed to CYP/P450 metabolism context, not to this particular active-site model.
- Line 37: the active-space construction and filling-fraction point are useful and should stay.
- Line 125: "runtime advantage for `M >= 1000` and active spaces `>= 31`" should be conditioned on the paper's surface-code and factory assumptions, and on QPE returning energies rather than a full chemistry workflow.
- Line 137: important overclaim. The paper's overlap evidence is about the largest determinant or computational-basis configuration in the DMRG/ASCI-like representation, not a general statement that simple Hartree-Fock initial states suffice. If the best determinant is not the HF determinant, the current wording is materially misleading.
- Lines 155-161: the caveats are good. Move the initial-state caveat next to line 137 so it cannot be missed.

## Missing or Suggested Additions

- Add a distinction between "largest determinant overlap" and "chemically easy-to-prepare determinant". That is the difference between a useful diagnostic and an actual state-preparation method.

