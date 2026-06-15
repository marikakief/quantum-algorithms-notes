# Review: Power of Data in Quantum Machine Learning (Huang-Babbush-McClean et al. 2021)

Source note: [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]]

## Primary Sources Checked

- Nature Communications article metadata: https://www.nature.com/articles/s41467-021-22539-9

## Verdict

Minor-to-major issues. The note is unusually good at explaining the paper's conceptual contribution, especially the geometric-difference diagnostic. The main concern is overclaim control: "training data makes hard quantum labels classically learnable" and "projected quantum kernels preserve quantum advantage" both require the paper's distributional, kernel, and data-access assumptions.

## Line-Anchored Comments

- Lines 16-20: "Up to 20% prediction advantage" should be tied immediately to engineered datasets and the paper's numerical setup. The current caveat appears later, but the headline should not read as a natural-data claim.

- Lines 24-34: The prediction-error expression should mention regularization/pseudoinverses and normalization of the kernel matrix. Raw `K^{-1}` is unstable when the kernel has small eigenvalues.

- Lines 36-44: The geometric difference discussion is clear. Add that it is a sufficient diagnostic for comparing two fixed kernels, not a universal no-go theorem against all classical learning algorithms.

- Lines 46-62: The dimension-test section is strong. It should explicitly distinguish the standard fidelity kernel's "too close to identity" failure from computational hardness of evaluating the feature map.

- Lines 72-88: The projected-kernel section risks suggesting the all-RDM shadow kernel restores full universality at practical cost. State the measurement and finite-sample overhead, and distinguish one-RDM projected kernels from all-order shadow kernels.

- Lines 90-98: The BPP/samp framing is useful, but "approaches BQP in power" is too informal. Say instead that sampled training data can change the prediction task relative to computing the target function from scratch.

- Lines 100-108: The discrete-log advantage should be framed as a specially constructed fault-tolerant learning problem, not evidence for broad practical QML advantage.

## Missing or Suggested Additions

- Add a short "data-access assumptions" box.
- Add one sentence explaining how the geometric difference can be computed from empirical kernel matrices and what numerical conditioning caveats apply.
