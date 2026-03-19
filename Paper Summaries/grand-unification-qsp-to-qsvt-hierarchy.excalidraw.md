---
excalidraw-plugin: parsed
tags: [excalidraw]
---
# Text Elements
QSVT ^qsvt1
Quantum Singular Value Transformation ^qsvt2
General block-encoded A ^qsvt3
Signal: singular values σₖ ∈ [-1,1] ^qsvt4
Circuit: alternating Uₐ, Uₐ† with left+right projector phases ^qsvt5
QET ^qet1
Quantum Eigenvalue Transform ^qet2
Hermitian block-encoded H ^qet3
Signal: eigenvalues λₖ/α ∈ [-1,1] ^qet4
Circuit: Uₕ with projector-controlled phases e^{iφΠ} ^qet5
QSP ^qsp1
Quantum Signal Processing ^qsp2
Single qubit, scalar signal a ∈ [-1,1] ^qsp3
Signal: W(a) rotation matrix ^qsp4
Circuit: alternating W(a) and S(φ) = diag(e^{iφ}, e^{-iφ}) ^qsp5
generalize: scalar → block-encoded Hermitian ^arr1
generalize: Hermitian → general matrix (use SVD) ^arr2
Same phase angles φ⃗ work at all three levels ^note1
Output: polynomial P applied to eigenvalues/singular values ^note2

# Drawing
```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "hand",
  "elements": [
    {
      "id": "box-qsvt",
      "type": "rectangle",
      "x": 100,
      "y": 20,
      "width": 560,
      "height": 140,
      "strokeColor": "#1971c2",
      "backgroundColor": "#d0ebff",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "roughness": 0,
      "opacity": 100
    },
    {
      "id": "lbl-qsvt1",
      "type": "text",
      "x": 120,
      "y": 35,
      "width": 80,
      "height": 28,
      "text": "QSVT",
      "fontSize": 22,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#1971c2"
    },
    {
      "id": "lbl-qsvt2",
      "type": "text",
      "x": 210,
      "y": 38,
      "width": 340,
      "height": 20,
      "text": "Quantum Singular Value Transformation",
      "fontSize": 15,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#1971c2"
    },
    {
      "id": "lbl-qsvt3",
      "type": "text",
      "x": 120,
      "y": 68,
      "width": 520,
      "height": 18,
      "text": "Object: general block-encoded A     Signal: singular values σₖ/α ∈ [-1,1]",
      "fontSize": 13,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#333"
    },
    {
      "id": "lbl-qsvt4",
      "type": "text",
      "x": 120,
      "y": 92,
      "width": 520,
      "height": 18,
      "text": "Phase gates: e^{iφΠ} (left ancilla) and e^{iφΠ̃} (right ancilla)",
      "fontSize": 13,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#333"
    },
    {
      "id": "lbl-qsvt5",
      "type": "text",
      "x": 120,
      "y": 116,
      "width": 520,
      "height": 18,
      "text": "Circuit: e^{iφ₀Π} · Uₐ† · e^{iφ₁Π̃} · Uₐ · e^{iφ₂Π} · … · Uₐ†",
      "fontSize": 13,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#333"
    },
    {
      "id": "arrow-down1",
      "type": "arrow",
      "x": 380,
      "y": 160,
      "width": 0,
      "height": 50,
      "strokeColor": "#555",
      "strokeWidth": 2,
      "roughness": 0,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "points": [[0,0],[0,50]]
    },
    {
      "id": "lbl-arr1",
      "type": "text",
      "x": 390,
      "y": 177,
      "width": 280,
      "height": 16,
      "text": "specialize: Hermitian → general (SVD)",
      "fontSize": 12,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#555"
    },
    {
      "id": "box-qet",
      "type": "rectangle",
      "x": 100,
      "y": 210,
      "width": 560,
      "height": 140,
      "strokeColor": "#2f9e44",
      "backgroundColor": "#d3f9d8",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "roughness": 0,
      "opacity": 100
    },
    {
      "id": "lbl-qet1",
      "type": "text",
      "x": 120,
      "y": 225,
      "width": 70,
      "height": 28,
      "text": "QET",
      "fontSize": 22,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#2f9e44"
    },
    {
      "id": "lbl-qet2",
      "type": "text",
      "x": 200,
      "y": 228,
      "width": 300,
      "height": 20,
      "text": "Quantum Eigenvalue Transform",
      "fontSize": 15,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#2f9e44"
    },
    {
      "id": "lbl-qet3",
      "type": "text",
      "x": 120,
      "y": 258,
      "width": 520,
      "height": 18,
      "text": "Object: Hermitian H, block-encoded in Uₕ     Signal: eigenvalues λₖ/α ∈ [-1,1]",
      "fontSize": 13,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#333"
    },
    {
      "id": "lbl-qet4",
      "type": "text",
      "x": 120,
      "y": 282,
      "width": 520,
      "height": 18,
      "text": "Phase gates: e^{iφΠ} (projector onto |0̃⟩ ancilla subspace)",
      "fontSize": 13,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#333"
    },
    {
      "id": "lbl-qet5",
      "type": "text",
      "x": 120,
      "y": 306,
      "width": 520,
      "height": 18,
      "text": "Circuit: e^{iφ₀Π} · Uₕ · e^{iφ₁Π} · Uₕ · … · e^{iφₐΠ}",
      "fontSize": 13,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#333"
    },
    {
      "id": "arrow-down2",
      "type": "arrow",
      "x": 380,
      "y": 350,
      "width": 0,
      "height": 50,
      "strokeColor": "#555",
      "strokeWidth": 2,
      "roughness": 0,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "points": [[0,0],[0,50]]
    },
    {
      "id": "lbl-arr2",
      "type": "text",
      "x": 390,
      "y": 367,
      "width": 260,
      "height": 16,
      "text": "specialize: block-encoded scalar → Hermitian",
      "fontSize": 12,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#555"
    },
    {
      "id": "box-qsp",
      "type": "rectangle",
      "x": 100,
      "y": 400,
      "width": 560,
      "height": 140,
      "strokeColor": "#e03131",
      "backgroundColor": "#ffe3e3",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "roughness": 0,
      "opacity": 100
    },
    {
      "id": "lbl-qsp1",
      "type": "text",
      "x": 120,
      "y": 415,
      "width": 70,
      "height": 28,
      "text": "QSP",
      "fontSize": 22,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#e03131"
    },
    {
      "id": "lbl-qsp2",
      "type": "text",
      "x": 200,
      "y": 418,
      "width": 280,
      "height": 20,
      "text": "Quantum Signal Processing",
      "fontSize": 15,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#e03131"
    },
    {
      "id": "lbl-qsp3",
      "type": "text",
      "x": 120,
      "y": 448,
      "width": 520,
      "height": 18,
      "text": "Object: single qubit     Signal: scalar a ∈ [-1,1]",
      "fontSize": 13,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#333"
    },
    {
      "id": "lbl-qsp4",
      "type": "text",
      "x": 120,
      "y": 472,
      "width": 520,
      "height": 18,
      "text": "Signal gate: W(a) = [[a, i√(1-a²)],[i√(1-a²), a]]",
      "fontSize": 13,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#333"
    },
    {
      "id": "lbl-qsp5",
      "type": "text",
      "x": 120,
      "y": 496,
      "width": 520,
      "height": 18,
      "text": "Phase gate: S(φ) = diag(e^{iφ}, e^{-iφ})     Circuit: S(φ₀) W(a) S(φ₁) W(a) … W(a) S(φₐ)",
      "fontSize": 13,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#333"
    },
    {
      "id": "note-box",
      "type": "rectangle",
      "x": 100,
      "y": 580,
      "width": 560,
      "height": 50,
      "strokeColor": "#888",
      "backgroundColor": "#fff9db",
      "fillStyle": "solid",
      "strokeWidth": 1,
      "roughness": 0,
      "opacity": 100
    },
    {
      "id": "note-text",
      "type": "text",
      "x": 120,
      "y": 593,
      "width": 520,
      "height": 20,
      "text": "Key: the same phase angles φ⃗ work at all three levels. Complexity = degree of approximating polynomial P.",
      "fontSize": 13,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#555"
    }
  ],
  "appState": {
    "gridSize": null
  }
}
```
