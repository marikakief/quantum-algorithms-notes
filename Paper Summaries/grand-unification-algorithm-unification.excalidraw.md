---
excalidraw-plugin: parsed
tags: [excalidraw]
---
# Text Elements
Grand Unification: Algorithms as QSVT Instances ^title1
QSVT ^center
Block-encode A, choose polynomial P, compile phases φ⃗ ^center2
Search ^search
P = sign(a) at κ=1/√N ^search2
Oracle: Grover diffusion ^search3
degree O(√N) ^search4
Phase Estimation ^pe
P = sign(cos(πa·2^j)) ^pe2
Oracle: A_j(θ̂) via LCU ^pe3
degree O(ε⁻¹) per bit ^pe4
Hamiltonian Simulation ^hamsim
P ≈ e^{-iλt} ^hamsim2
Oracle: block-encoding of H ^hamsim3
degree O(t + log(1/ε)) ^hamsim4
Matrix Inversion ^matinv
P ≈ 1/σ on [1/κ,1] ^matinv2
Oracle: block-encoding of A ^matinv3
degree O(κ log(1/ε)) ^matinv4
Eigenvalue Threshold ^evt
P = sign(λ-λ*) ^evt2
Oracle: block-encoding of H ^evt3
degree O(δ⁻¹ log(1/ε)) ^evt4
Different polynomial — same circuit template ^note1

# Drawing
```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "hand",
  "elements": [
    {
      "id": "title",
      "type": "text",
      "x": 100,
      "y": 10,
      "width": 600,
      "height": 28,
      "text": "Grand Unification: Algorithms as QSVT Instances",
      "fontSize": 20,
      "fontFamily": 1,
      "textAlign": "center",
      "strokeColor": "#1971c2"
    },
    {
      "id": "center-box",
      "type": "rectangle",
      "x": 270,
      "y": 200,
      "width": 260,
      "height": 80,
      "strokeColor": "#1971c2",
      "backgroundColor": "#d0ebff",
      "fillStyle": "solid",
      "strokeWidth": 3,
      "roughness": 0
    },
    {
      "id": "center-lbl",
      "type": "text",
      "x": 280,
      "y": 215,
      "width": 240,
      "height": 22,
      "text": "QSVT",
      "fontSize": 18,
      "fontFamily": 1,
      "textAlign": "center",
      "strokeColor": "#1971c2"
    },
    {
      "id": "center-lbl2",
      "type": "text",
      "x": 280,
      "y": 242,
      "width": 240,
      "height": 28,
      "text": "Block-encode A\nChoose poly P, compile φ⃗",
      "fontSize": 11,
      "fontFamily": 1,
      "textAlign": "center",
      "strokeColor": "#555"
    },
    {
      "id": "search-box",
      "type": "rectangle",
      "x": 20,
      "y": 50,
      "width": 200,
      "height": 100,
      "strokeColor": "#e67700",
      "backgroundColor": "#fff9db",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "roughness": 0
    },
    {
      "id": "search-lbl",
      "type": "text",
      "x": 25,
      "y": 58,
      "width": 190,
      "height": 20,
      "text": "Search",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#e67700"
    },
    {
      "id": "search-lbl2",
      "type": "text",
      "x": 25,
      "y": 82,
      "width": 190,
      "height": 60,
      "text": "P = sign(a) at κ=1/√N\nOracle: Grover diffusion\nDegree: O(√N log(1/ε))",
      "fontSize": 11,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#333"
    },
    {
      "id": "arr-search",
      "type": "arrow",
      "x": 220,
      "y": 100,
      "width": 55,
      "height": 130,
      "strokeColor": "#aaa",
      "strokeWidth": 1,
      "roughness": 0,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "points": [[0,0],[55,130]]
    },
    {
      "id": "pe-box",
      "type": "rectangle",
      "x": 20,
      "y": 200,
      "width": 220,
      "height": 110,
      "strokeColor": "#2f9e44",
      "backgroundColor": "#d3f9d8",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "roughness": 0
    },
    {
      "id": "pe-lbl",
      "type": "text",
      "x": 25,
      "y": 208,
      "width": 210,
      "height": 20,
      "text": "Phase Estimation",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#2f9e44"
    },
    {
      "id": "pe-lbl2",
      "type": "text",
      "x": 25,
      "y": 232,
      "width": 210,
      "height": 70,
      "text": "P = sign(cos(π·a·2^j))\nOracle: A_j(θ̂) via LCU\nDegree: O(ε⁻¹) per bit\nNo QFT needed",
      "fontSize": 11,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#333"
    },
    {
      "id": "arr-pe",
      "type": "arrow",
      "x": 240,
      "y": 240,
      "width": 30,
      "height": 0,
      "strokeColor": "#aaa",
      "strokeWidth": 1,
      "roughness": 0,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "points": [[0,0],[30,0]]
    },
    {
      "id": "evt-box",
      "type": "rectangle",
      "x": 20,
      "y": 360,
      "width": 200,
      "height": 100,
      "strokeColor": "#862e9c",
      "backgroundColor": "#f3d9fa",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "roughness": 0
    },
    {
      "id": "evt-lbl",
      "type": "text",
      "x": 25,
      "y": 368,
      "width": 190,
      "height": 20,
      "text": "Eigenvalue Threshold",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#862e9c"
    },
    {
      "id": "evt-lbl2",
      "type": "text",
      "x": 25,
      "y": 392,
      "width": 190,
      "height": 60,
      "text": "P = sign((λ-λ*)/α)\nOracle: block-encoding H\nDegree: O(δ⁻¹ log(1/ε))",
      "fontSize": 11,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#333"
    },
    {
      "id": "arr-evt",
      "type": "arrow",
      "x": 220,
      "y": 390,
      "width": 55,
      "height": -140,
      "strokeColor": "#aaa",
      "strokeWidth": 1,
      "roughness": 0,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "points": [[0,0],[55,-140]]
    },
    {
      "id": "hamsim-box",
      "type": "rectangle",
      "x": 570,
      "y": 50,
      "width": 210,
      "height": 100,
      "strokeColor": "#c92a2a",
      "backgroundColor": "#ffe3e3",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "roughness": 0
    },
    {
      "id": "hamsim-lbl",
      "type": "text",
      "x": 575,
      "y": 58,
      "width": 200,
      "height": 20,
      "text": "Hamiltonian Simulation",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#c92a2a"
    },
    {
      "id": "hamsim-lbl2",
      "type": "text",
      "x": 575,
      "y": 82,
      "width": 200,
      "height": 60,
      "text": "P ≈ e^{-iλt}\nOracle: block-encoding H\nDegree: O(t + log(1/ε))\nOptimal (matches LB)",
      "fontSize": 11,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#333"
    },
    {
      "id": "arr-hamsim",
      "type": "arrow",
      "x": 565,
      "y": 100,
      "width": -55,
      "height": 130,
      "strokeColor": "#aaa",
      "strokeWidth": 1,
      "roughness": 0,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "points": [[0,0],[-55,130]]
    },
    {
      "id": "matinv-box",
      "type": "rectangle",
      "x": 570,
      "y": 360,
      "width": 210,
      "height": 100,
      "strokeColor": "#364fc7",
      "backgroundColor": "#e7f5ff",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "roughness": 0
    },
    {
      "id": "matinv-lbl",
      "type": "text",
      "x": 575,
      "y": 368,
      "width": 200,
      "height": 20,
      "text": "Matrix Inversion",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#364fc7"
    },
    {
      "id": "matinv-lbl2",
      "type": "text",
      "x": 575,
      "y": 392,
      "width": 200,
      "height": 60,
      "text": "P ≈ 1/σ on [1/κ, 1]\nOracle: block-encoding A\nDegree: O(κ log(1/ε))\nSubsumes HHL",
      "fontSize": 11,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#333"
    },
    {
      "id": "arr-matinv",
      "type": "arrow",
      "x": 565,
      "y": 390,
      "width": -55,
      "height": -140,
      "strokeColor": "#aaa",
      "strokeWidth": 1,
      "roughness": 0,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "points": [[0,0],[-55,-140]]
    },
    {
      "id": "note-box",
      "type": "rectangle",
      "x": 200,
      "y": 490,
      "width": 420,
      "height": 40,
      "strokeColor": "#495057",
      "backgroundColor": "#f1f3f5",
      "fillStyle": "solid",
      "strokeWidth": 1,
      "roughness": 0
    },
    {
      "id": "note-text",
      "type": "text",
      "x": 208,
      "y": 500,
      "width": 404,
      "height": 20,
      "text": "All use the same circuit template — only the polynomial P differs",
      "fontSize": 13,
      "fontFamily": 1,
      "textAlign": "center",
      "strokeColor": "#333"
    }
  ],
  "appState": {
    "gridSize": null
  }
}
```
