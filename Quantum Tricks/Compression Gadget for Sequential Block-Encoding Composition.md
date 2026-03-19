# Compression Gadget for Sequential Block-Encoding Composition

> **Source:** Fang, Lin & Tong, arXiv:2208.06941, Quantum **7**, 955 (2023). Adapted from [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low & Wiebe (2018)]].
> **Tags:** #trick #block-encoding #composition #ancilla-efficient #time-marching

## What it does

Composes $L$ block-encodings of operators $\Gamma_1, \ldots, \Gamma_L$ into a single block-encoding of their product $\Gamma_L \cdots \Gamma_1$, using each constituent unitary exactly once and only $O(\log L)$ extra ancilla qubits.

## The trick

Given unitaries $V_l$, each an $(\alpha_l', m_l', 0)$-block encoding of $\Gamma_l$, construct:

1. A **coherent counter** register of $\lceil \log_2 L \rceil$ qubits that tracks which step $l$ is being executed.
2. A **binary-tree routing circuit** that, conditioned on the counter value, applies $V_l$ to the system + ancilla registers in the correct order.
3. The counter is initialised in $|1\rangle$, incremented after each application, and the final measurement checks that it reached $|L\rangle$.

The resulting block-encoding has:
- Normalisation: $\alpha_{\text{comp}} = \prod_{l=1}^{L} \alpha_l'$
- Ancilla count: $\max_l m_l' + \lceil \log_2 L \rceil + 1$
- Each $V_l$ is used exactly once

**Why it matters:** The naive approach — tensor all $V_l$ and post-select — uses each $V_l$ once but requires $\sum_l m_l'$ ancilla qubits. The compression gadget reduces this to $\max_l m_l' + O(\log L)$ by reusing the same ancilla workspace across steps.

## When to reach for it

- Time-marching quantum ODE/PDE solvers where $L$ short-time propagators must be composed coherently
- Any sequential block-encoding composition: multi-step quantum walks, layered transformations, cascaded filters
- Interaction-picture simulation (the original context in [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low-Wiebe]])

## Complexity

$O(L)$ total applications of $\{V_l\}$ (one each), plus $O(L \log L)$ elementary gates for the routing logic. The depth is $\sum_l (\text{depth of } V_l)$ — no parallelism across steps.

## Caveat

The normalisation $\alpha_{\text{comp}} = \prod_l \alpha_l'$ is multiplicative. If the individual normalisations aren't tight (close to $\|\Gamma_l\|$), the product blows up and the success probability drops. This is why [[Uniform Singular Value Amplification for Block-Encoding Norm Matching|USVA]] is applied *before* compression — to bring each $\alpha_l'$ close to $\|\Gamma_l\|$.

## Related notes
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes]]
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]]
- [[Uniform Singular Value Amplification for Block-Encoding Norm Matching]]
- [[Oblivious Amplitude Amplification (Robust)]]
