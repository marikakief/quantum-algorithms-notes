---
excalidraw-plugin: parsed
tags: [excalidraw]
---
# Text Elements
QSVT Circuit (odd degree d) ^title1
left ancilla |0̃⟩ ^lanc
system register ^sys
right ancilla |0̃'⟩ ^ranc
e^{iφ₀Π} ^p0
Uₐ† ^ua1
e^{iφ₁Π̃} ^p1
Uₐ ^ua2
e^{iφ₂Π} ^p2
Uₐ† ^ua3
e^{iφ_{d-1}Π̃} ^pd1
Uₐ ^uad
e^{iφ_dΠ} ^pd
… ^dots1
… ^dots2
… ^dots3
Result: (⟨0̃| ⊗ I ⊗ ⟨0̃'|) U_QSVT (|0̃⟩ ⊗ I ⊗ |0̃'⟩) = P^(SV)(A/α) ^result1
Left projector phases act on left ancilla ^note1
Right projector phases act on right ancilla ^note2
d applications of Uₐ or Uₐ†, alternating ^note3

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
      "x": 60,
      "y": 15,
      "width": 400,
      "height": 28,
      "text": "QSVT Circuit  (odd degree d)",
      "fontSize": 20,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#1971c2"
    },
    {
      "id": "wire-l",
      "type": "line",
      "x": 60,
      "y": 90,
      "width": 700,
      "height": 0,
      "strokeColor": "#1971c2",
      "strokeWidth": 2,
      "roughness": 0,
      "points": [[0,0],[700,0]]
    },
    {
      "id": "wire-s",
      "type": "line",
      "x": 60,
      "y": 150,
      "width": 700,
      "height": 0,
      "strokeColor": "#333",
      "strokeWidth": 3,
      "roughness": 0,
      "points": [[0,0],[700,0]]
    },
    {
      "id": "wire-r",
      "type": "line",
      "x": 60,
      "y": 210,
      "width": 700,
      "height": 0,
      "strokeColor": "#e03131",
      "strokeWidth": 2,
      "roughness": 0,
      "points": [[0,0],[700,0]]
    },
    {
      "id": "lbl-l",
      "type": "text",
      "x": 5,
      "y": 80,
      "width": 55,
      "height": 18,
      "text": "|0̃⟩",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#1971c2"
    },
    {
      "id": "lbl-s",
      "type": "text",
      "x": 5,
      "y": 140,
      "width": 55,
      "height": 18,
      "text": "system",
      "fontSize": 12,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#333"
    },
    {
      "id": "lbl-r",
      "type": "text",
      "x": 5,
      "y": 200,
      "width": 55,
      "height": 18,
      "text": "|0̃'⟩",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#e03131"
    },
    {
      "id": "gate-p0",
      "type": "rectangle",
      "x": 70,
      "y": 72,
      "width": 60,
      "height": 36,
      "strokeColor": "#1971c2",
      "backgroundColor": "#d0ebff",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "roughness": 0
    },
    {
      "id": "lbl-p0",
      "type": "text",
      "x": 72,
      "y": 81,
      "width": 56,
      "height": 18,
      "text": "e^{iφ₀Π}",
      "fontSize": 12,
      "fontFamily": 1,
      "textAlign": "center",
      "strokeColor": "#1971c2"
    },
    {
      "id": "gate-ua1",
      "type": "rectangle",
      "x": 148,
      "y": 70,
      "width": 60,
      "height": 158,
      "strokeColor": "#333",
      "backgroundColor": "#f8f9fa",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "roughness": 0
    },
    {
      "id": "lbl-ua1",
      "type": "text",
      "x": 156,
      "y": 140,
      "width": 44,
      "height": 18,
      "text": "Uₐ†",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "strokeColor": "#333"
    },
    {
      "id": "gate-p1",
      "type": "rectangle",
      "x": 225,
      "y": 192,
      "width": 60,
      "height": 36,
      "strokeColor": "#e03131",
      "backgroundColor": "#ffe3e3",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "roughness": 0
    },
    {
      "id": "lbl-p1",
      "type": "text",
      "x": 226,
      "y": 201,
      "width": 58,
      "height": 18,
      "text": "e^{iφ₁Π̃}",
      "fontSize": 12,
      "fontFamily": 1,
      "textAlign": "center",
      "strokeColor": "#e03131"
    },
    {
      "id": "gate-ua2",
      "type": "rectangle",
      "x": 302,
      "y": 70,
      "width": 60,
      "height": 158,
      "strokeColor": "#333",
      "backgroundColor": "#f8f9fa",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "roughness": 0
    },
    {
      "id": "lbl-ua2",
      "type": "text",
      "x": 314,
      "y": 140,
      "width": 36,
      "height": 18,
      "text": "Uₐ",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "strokeColor": "#333"
    },
    {
      "id": "gate-p2",
      "type": "rectangle",
      "x": 379,
      "y": 72,
      "width": 60,
      "height": 36,
      "strokeColor": "#1971c2",
      "backgroundColor": "#d0ebff",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "roughness": 0
    },
    {
      "id": "lbl-p2",
      "type": "text",
      "x": 380,
      "y": 81,
      "width": 58,
      "height": 18,
      "text": "e^{iφ₂Π}",
      "fontSize": 12,
      "fontFamily": 1,
      "textAlign": "center",
      "strokeColor": "#1971c2"
    },
    {
      "id": "dots-l",
      "type": "text",
      "x": 455,
      "y": 82,
      "width": 30,
      "height": 18,
      "text": "…",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "strokeColor": "#1971c2"
    },
    {
      "id": "dots-s",
      "type": "text",
      "x": 455,
      "y": 142,
      "width": 30,
      "height": 18,
      "text": "…",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "strokeColor": "#333"
    },
    {
      "id": "dots-r",
      "type": "text",
      "x": 455,
      "y": 202,
      "width": 30,
      "height": 18,
      "text": "…",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "strokeColor": "#e03131"
    },
    {
      "id": "gate-uad",
      "type": "rectangle",
      "x": 500,
      "y": 70,
      "width": 60,
      "height": 158,
      "strokeColor": "#333",
      "backgroundColor": "#f8f9fa",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "roughness": 0
    },
    {
      "id": "lbl-uad",
      "type": "text",
      "x": 508,
      "y": 140,
      "width": 44,
      "height": 18,
      "text": "Uₐ",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "strokeColor": "#333"
    },
    {
      "id": "gate-pd",
      "type": "rectangle",
      "x": 578,
      "y": 192,
      "width": 60,
      "height": 36,
      "strokeColor": "#e03131",
      "backgroundColor": "#ffe3e3",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "roughness": 0
    },
    {
      "id": "lbl-pd",
      "type": "text",
      "x": 579,
      "y": 201,
      "width": 58,
      "height": 18,
      "text": "e^{iφ_{d}Π̃}",
      "fontSize": 12,
      "fontFamily": 1,
      "textAlign": "center",
      "strokeColor": "#e03131"
    },
    {
      "id": "gate-uadlast",
      "type": "rectangle",
      "x": 648,
      "y": 70,
      "width": 60,
      "height": 158,
      "strokeColor": "#333",
      "backgroundColor": "#f8f9fa",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "roughness": 0
    },
    {
      "id": "lbl-uadlast",
      "type": "text",
      "x": 656,
      "y": 140,
      "width": 44,
      "height": 18,
      "text": "Uₐ†",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "strokeColor": "#333"
    },
    {
      "id": "result-box",
      "type": "rectangle",
      "x": 60,
      "y": 260,
      "width": 660,
      "height": 38,
      "strokeColor": "#2f9e44",
      "backgroundColor": "#d3f9d8",
      "fillStyle": "solid",
      "strokeWidth": 1,
      "roughness": 0
    },
    {
      "id": "result-text",
      "type": "text",
      "x": 70,
      "y": 270,
      "width": 640,
      "height": 18,
      "text": "Result: (⟨0̃|⊗I⊗⟨0̃'|) U_QSVT (|0̃⟩⊗I⊗|0̃'⟩) = P^(SV)(A/α)   where P is the QSP polynomial",
      "fontSize": 13,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#2f9e44"
    },
    {
      "id": "note1",
      "type": "text",
      "x": 60,
      "y": 308,
      "width": 660,
      "height": 16,
      "text": "Blue gates act on left ancilla (Π = |0̃⟩⟨0̃|⊗I).   Red gates act on right ancilla (Π̃ = I⊗|0̃'⟩⟨0̃'|).",
      "fontSize": 12,
      "fontFamily": 1,
      "textAlign": "left",
      "strokeColor": "#555"
    },
    {
      "id": "note2",
      "type": "text",
      "x": 60,
      "y": 328,
      "width": 660,
      "height": 16,
      "text": "Gray blocks span all three wires — Uₐ and Uₐ† act on system + both ancillae.",
      "fontSize": 12,
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
