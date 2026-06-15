# Review: Coherent Alias Sampling for PREPARE

Source note: [[Coherent Alias Sampling for PREPARE]]

## Primary Sources Checked

- Babbush et al., arXiv:1805.03662, Section III.D.
- Berry et al., arXiv:1902.02134 for sparse/QROAM adaptations.

## Verdict

Major issues. The classical-alias-to-quantum story is correct, but the card misstates what happens to garbage registers and when measurement-based uncomputation is legal inside PREPARE.

## Line-Anchored Comments

- Line 7: the additive precision dependence is a good headline. It should be framed as a cost in this alias/QROM implementation, not a general property of all arbitrary state preparation.
- Line 23: uniform preparation over non-power-of-two `L` is not a unary-iteration primitive in the same sense as indexed SELECT/QROM. If amplitude amplification/rejection is used, give the exact method or remove the `O(log L)` T claim.
- Lines 25-27: the QROM + comparator + conditional swap construction is clear.
- Line 29: misleading. After the coherent alias procedure, the full state is a pure state over output and auxiliary registers. Tracing out garbage gives the right marginal distribution, not a clean amplitude state solely on `|ell>`. For qubitization, PREPARE and PREPARE inverse/reflection must handle the actual coherent state.
- Lines 31-35: the precision discussion is good but source-specific; the displayed `mu` includes chemistry-specific parameters and should not be presented as universal alias-sampling precision.
- Lines 37-39: the `subprepare` cost is for the 2018 plane-wave-dual chemistry construction, not arbitrary `L`-term PREPARE. Label it as such.
- Line 57: "This uncomputation is free (measure and reset classically)" is too broad and likely wrong in this context. PREPARE's garbage cannot generally be measured before the reflection/PREPARE-dagger sequence without changing the block encoding. Measurement-based cleanup applies only when the register's role is compatible with deferred measurement and phase fixup.

## Missing or Suggested Additions

- Add a "garbage is allowed only as part of the prepared standard-form state" warning.
- Separate standard coherent inverse uncomputation from later measurement-based QROM output cleanup.

