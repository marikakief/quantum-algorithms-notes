# Review: Polynomial-Time Quantum Algorithm for the Simulation of Chemical Dynamics (Kassal-Jordan-Love-Mohseni-Aspuru-Guzik 2008)

Source note: [[Polynomial-Time Quantum Algorithm for the Simulation of Chemical Dynamics (Kassal-Jordan-Love-Mohseni-Aspuru-Guzik 2008) — Paper Notes]]

## Primary Sources Checked

- Kassal, Jordan, Love, Mohseni, and Aspuru-Guzik, *Polynomial-time quantum algorithm for the simulation of chemical dynamics*, arXiv:0801.2986, PNAS 105, 18681 (2008): https://arxiv.org/abs/0801.2986

## Verdict

Major issues. The conceptual summary is good, but the note contains a real scaling inconsistency (`m^2` versus `m^3`) and overstates the efficiency of thermal-state preparation. Those are not cosmetic: they affect the paper's claimed resource accounting.

## Line-Anchored Comments

- Lines 15-19: The top-level description matches the source abstract: split-operator propagation, all electron-nuclear/interelectronic interactions, state preparation, and chemically meaningful observables are the paper's main claims.

- Line 16: The Coulomb potential evaluation scaling is inconsistent with lines 54-58 and line 119. The detailed arithmetic line says each pair costs cubic in precision, with total `(75/4)m^3 + (51/2)m^2` gates per pair. The headline should not say `O(B^2 m^2)` unless the source is using a different precision model that is explained.

- Line 29: `d = 3B - 6` is the internal coordinate count after removing global translation/rotation for a molecule. The grid algorithm itself can also be described in Cartesian particle coordinates. This line should specify the coordinate convention.

- Lines 31-35: The split-operator formula is convention-dependent. Depending on whether the state starts in position or momentum representation and on the QFT sign convention, the order may be `V`, QFT, `T`, inverse QFT. The note should say "up to QFT convention/order" rather than presenting the formula as canonical.

- Lines 41-47: The phase-kickback idea is correct, but `delta t = 2*pi/M` is too compressed. In practice the potential value, timestep, and fixed-point scaling must be mapped into an integer phase modulo `M`; `delta t` is not generally forced to that value.

- Lines 50 and 139: "No need to uncompute" is a useful point, but it relies on the arithmetic oracle being an addition into a Fourier eigenstate and not leaving additional scratch entangled with `x`. Any scratch used to compute `V(x)` still has to be handled reversibly.

- Lines 68-71: The Born-Oppenheimer crossover discussion is source-faithful but should be marked as an estimate under the paper's PES interpolation assumptions (`K >= 15`, target accuracy, and the chosen benchmark reaction), not a universal theorem about BO versus full dynamics.

- Line 83: The thermal-state sentence is too strong. Preparing a Boltzmann-weighted superposition or purification is not generally efficient merely because the Boltzmann factor is exponential in energy. The source gives a construction for chemically structured inputs; arbitrary thermal state preparation remains a hard task.

- Line 97: The amplitude-estimation improvement should say it improves additive estimation scaling under coherent access to the relevant yes/no measurement circuit; it does not make state preparation or projection onto complicated product eigenstates free.

- Line 119: Same scaling problem as line 16: this should be reconciled with the explicit `m^3` arithmetic at lines 54-58.

## Missing or Suggested Additions

- Add a short error-budget split: grid discretization, Trotter/split-operator error, fixed-point arithmetic error, and sampling/amplitude-estimation error.
- State explicitly that the algorithm is polynomial in particle number and precision parameters under smoothness/resolution assumptions; the grid size still has to resolve the physics.
- Add a caveat that "100 qubits" is a logical-qubit-era estimate and excludes full error-correction overhead.
