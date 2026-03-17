
> **Tags:** #trick #low-energy #phase-estimation #block-encoding
> **Source:** arXiv:2505.01528 (SOSSA)
> **Full notes:** [[SOSSA — Sum-of-Squares Spectral Amplification]]

## What it does
Reduces query complexity for low-energy simulation from $\lambda/\varepsilon$ to $\sqrt{\Delta\lambda}/\varepsilon$, where $\Delta$ is the gap from the ground state energy to the SOS lower bound.

## The trick
1. Solve an SDP to find $H + \beta\mathbb{1} = \sum_j B_j^\dagger B_j$ with $-\beta$ a tight lower bound on $E_0$
2. Build the rectangular operator $H_{\text{SA}} = \sum_j |j\rangle \otimes B_j$ (a "square root" of $H + \beta\mathbb{1}$)
3. Small eigenvalues $E$ of $H + \beta\mathbb{1}$ become $\sqrt{E}$ in $H_{\text{SA}}$ — amplified near zero
4. Phase estimation on $H_{\text{SA}}$ resolves low-energy eigenvalues with fewer queries

## When to reach for it
- Low-energy physics (ground state, low-lying excitations)
- Hamiltonians where the SOS relaxation gives a tight bound (frustrated spin systems, SYK)
- Any setting where $\lambda_{\text{[[Linear Combination of Unitaries (LCU)|LCU]]}}$ is large but the relevant energy scale is small

## Complexity
$O(\sqrt{\Delta_{\text{SOS}} \cdot \lambda_{\text{SOS}}}/\varepsilon)$ queries. Classical SDP preprocessing: poly($L$) where $L$ = operator algebra dimension.

## Caveat
Higher-degree SOS tightens $\Delta$ but grows $\lambda$ (more terms in the SOS decomposition). The *product* $\Delta \cdot \lambda$ determines the win — degree must be optimised per system. For SYK specifically, degree-2 Majorana SOS achieves $\sqrt{\Delta_\text{SOS}\lambda_\text{SOS}} \sim N^{3/2}$ vs $\lambda_\text{[[Linear Combination of Unitaries (LCU)|LCU]]} \sim N^2$ — a $\sqrt{N}$ speedup. Whether higher degree helps depends on whether the tighter $\Delta$ compensates for the larger $\lambda$; the paper finds degree-2 sufficient for the SYK demonstration.

## Related Paper Notes

- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[SOSSA — Sum-of-Squares Spectral Amplification]]
