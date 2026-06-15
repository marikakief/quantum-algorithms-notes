# Review: Processed Product Formula Kernel-Processor Decomposition

Source note: [[Processed Product Formula Kernel-Processor Decomposition]]

## Primary Sources Checked

- Morales et al., arXiv:2210.15817: https://arxiv.org/abs/2210.15817

## Verdict

Major issue in the caveat. The processor is a unitary product of exponentials; the problem is not nonunitarity.

## Line-Anchored Comments

- Lines 7-13: the kernel/processor idea is correctly described.
- Lines 23-25: the "use when `r >> 1`" caveat is exactly right; the endpoint processor overhead must be amortized.
- Line 29: the concrete 17-stage versus 21-stage comparison is useful.
- Line 33: "The processor `P` is not unitary in general" is false if `P` is implemented as a product of exponentials of anti-Hermitian/Hermitian Hamiltonian terms. It is unitary, though it is not itself an approximation to the target evolution.
- Line 33: the controlled-time/phase-estimation caveat is good; keep that part after removing the nonunitarity claim.

## Missing or Suggested Additions

- Replace the caveat with: `P` and `P^{-1}` must be applied coherently around the whole repeated kernel, which complicates controlled powers and intermediate measurements.
