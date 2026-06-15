# Tranche 14 Resolution

Scope: primality/factoring notes, symmetric-key Grover-resource estimates, semigroup DLP notes, and the Regev/Kuperberg lattice-DHSP cluster reviewed in Tranche 14.

## Paper notes

- `A Quantum Primality Test with Order Finding (Donis-Vela-Garcia-Escartin 2017)`: implemented. Added the one-sided prime-certification caveat, separated composite-input rejection under independent random bases from prime-input primitive-root probability, and scoped the complexity comparison to the paper's arithmetic/order-finding model rather than modern fault-tolerant resource estimates. Also clarified that a base with order `N-1` is quantum-checkable unless extra classical factor data for `N-1` is supplied.

- `Factoring Safe Semiprimes with a Single Quantum Query (Grosshans-Lawson-Morain-Smith 2017)`: implemented. Clarified that the fixed base `2` matters for reusable compiled circuits and is not selected using factor knowledge. Renamed the quantum-call column to QOFA calls and separated the safe-semiprime theorem from the empirical salvage discussion for general composites.

- `A Log-Log Speedup for Exponent One-Fifth Deterministic Integer Factorisation (Harvey-Hittmeir 2021)`: implemented. Reframed the result as a rigorous asymptotic improvement despite unchanged main exponent, replaced the small-factor discussion with the deterministic Pollard-Strassen/Coppersmith-Harvey-style search used in the paper's reduction, and added the large-order/common-factor caveats around the `beta^{m^2}` construction.

- `Primality Proving via One Round in ECPP and One Iteration in AKS (Cheng 2003)`: implemented. Clarified that `r` is a prime factor of `n-1` in Cheng's construction, explained the order-forcing condition in the `a=1` case, and marked Rabin-Miller as a prover-side witness-finding tool rather than a verifier assumption.

- `Implementing the Asymptotically Fast Version of the Elliptic Curve Primality Proving Algorithm (Morain 2005)`: implemented. Added that practical ECPP certificates can use group orders `cN'` with factored/smooth cofactor, treated the timings as historical scaling evidence, and distinguished deterministic certificate verification from heuristic prover/search performance.

- `Constructing Elliptic Curve Isogenies in Quantum Subexponential Time (Childs-Jao-Soukharev 2014)`: implemented. Reworded the GRH discussion so the note says the result avoids extra smoothness heuristics under GRH, not that it is unconditional.

- `Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015)`: implemented. Scoped diffusion and comparison counts to one Grover iteration, clarified the depth/qubit tradeoff from parallel AES boxes, and stated that the estimates are logical Clifford+T counts without geometry, routing, surface-code, or factory accounting.

- `Estimating the Cost of Generic Quantum Pre-Image Attacks on SHA-2 and SHA-3 (Amy-Di Matteo-Gheorghiu-Mosca-Parent-Schanck 2016)`: implemented. Replaced the overstrong lower-bound phrasing with historical/model-dependent resource estimates and added the SHA-256/SHA3 tradeoff: SHA3 has lower T-depth in the paper's model but needs many more factories.

- `A Note on Quantum Related-Key Attacks (Roetteler-Steinwandt 2013)`: implemented. The note now makes the Q2/coherent related-key oracle requirement explicit before the polynomial-time key-recovery statement. The existing text already explained the known-plaintext tuple uniqueness and canonical-pair cost caveats, so no further change was needed there.

- `A Reduction of Semigroup DLP to Classic DLP (Banin-Tsaban 2013)`: implemented. Replaced ambiguous oracle language with an ordinary cyclic-group DLP oracle, noted that Shor may supply this oracle quantumly, added the tail-washout reason for the large random exponent interval, and kept the warning that the reduction does not make cyclic-group DLP easier.

- `Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013)`: implemented. Added a separation paragraph so ordinary one-generator semigroup DLP, shifted semigroup DLP, and constructive membership are not conflated. Also made explicit that the constructive-membership lower bound is black-box and may not apply to explicit semigroups with exploitable structure.

- `Quantum Computation and Lattice Problems (Regev 2002)`: implemented. Added the missing title, distinguished lattice dimension `n` from DCP/subset-sum modulus `N`, replaced the overstrong post-quantum-cryptography lineage claim with the LWE predecessor/bridge framing, rewrote the DCP/DHSP theorem summary so samples plus efficient post-processing are required, updated the Kuperberg and EHK references to vault links, and added the unique-SVP-only/subexponential-route caveat.

- `Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013)`: implemented. Tightened QRACM language to mean coherent indexed access to exponentially large classical lists rather than full quantum storage, and added that time/space exponents are not full architecture or area-time attack costs.

- `A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005)`: implemented. Added the missing title, normalized the labelled-qubit state, clarified the retained sign/outcome accounting in the sieve, corrected the fixed-radix greedy-sieve bound to `O(3^{sqrt(2 log_3 N)})`, removed the false later-paper improvement claim, and added a resource summary contrasting Kuperberg 2005, Regev 2004, and Kuperberg 2013.

- `A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004)`: implemented. Added the missing title, made `n = log N` explicit, promoted the fact that the `2^{O(l)}` enumeration is classical work inside each batch attempt, clarified that "no piles" means no stored exponential quantum list rather than no regeneration, and cross-linked the 2013 collimation tradeoff.

- `Another Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2013)`: implemented. Added the missing title, inserted a collimation equation, added a comparison table for quantum space/classical space/time/QRACM across Kuperberg 2005, Regev 2004, and Kuperberg 2013, and scoped the "more useful" assessment to resource separation rather than a global ranking.
