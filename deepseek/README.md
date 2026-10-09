# AI Offline Assessor

A single‑file, browser‑based prototype that assesses student answers using **five on‑device AI agents**. It runs entirely locally after an initial model download — no student data ever leaves the device, and it works with Wi‑Fi turned off.

[![Offline Ready](https://img.shields.io/badge/offline-ready-brightgreen)]()
[![On-Device AI](https://img.shields.io/badge/AI-on--device-blue)]()
[![No Server](https://img.shields.io/badge/server-none-lightgrey)]()

---

## ✨ Features

- **Five independent AI agents** that share lightweight models:
  1. **Spelling** – flags misspellings using a bundled dictionary + domain vocabulary.
  2. **Expected‑answer matcher** – hybrid keyword & bigram coverage against the teacher’s model answer.
  3. **Semantic meaning** – uses sentence embeddings to compare meaning, not just words.
  4. **Sentence formation / grammar** – uses a small language model to suggest corrections.
  5. **Final assessor** – applies weighted scoring and produces a band (Full / Good / Partial / Weak).
- **Weighted scoring** out of 10, with per‑agent breakdowns.
- **Annotated student answer** – spelling and grammar issues highlighted inline.
- **Key idea chips** – shows which expected concepts were hit or missed.
- **100% on‑device** – after the first model download, all inference runs in the browser.
- **Zero server calls** – no student answers are uploaded.
- **Copy report** – export a plain‑text assessment report.

---

## 🚀 Quick Start

1. **Save** the provided HTML code as `index.html`.
2. **Open** it in a modern browser (Chrome, Edge, Firefox, Safari).
3. **Wait** for the two models to download (~100 MB total). This happens once; they are cached in IndexedDB.
4. **Paste** a teacher question, teacher answer, and student answer.
5. **Click** “Assess answer” (or press `Ctrl + Enter`).

After the models are cached, you can disconnect from the internet and the app will continue to work fully offline.

---

## 🧠 Models Used

| Model | Task | Size | Used by |
|-------|------|------|---------|
| `Xenova/all-MiniLM-L6-v2` | Sentence embeddings | ~25 MB | Agent 3 – Semantic meaning |
| `Xenova/flan-t5-small` | Grammar correction | ~75 MB | Agent 4 – Sentence formation |

Both are quantized (q8) and loaded via **Transformers.js** (ONNX Runtime Web). They are cached by the browser and reused on subsequent visits.

---

## 📴 Offline Operation

- On first load, the app downloads model weights from HuggingFace Hub.
- Weights are stored in the browser’s IndexedDB cache.
- After that, **no network requests are made** — all inference runs locally.
- The header badge indicates whether the app is online or offline‑ready.

---

## 🛠 Customization

- **Swap models:** Edit the `MODEL_CONFIG` object at the top of the script.
  - For higher grammar accuracy, try `Xenova/flan-t5-base` (~250 MB) or a dedicated GEC model.
- **Adjust weights:** Modify the `WEIGHTS` object in Agent 5 to change how much each agent contributes to the final score.
- **Extend the dictionary:** Update the `DICT` set in Agent 1 for domain‑specific vocabulary.
- **Add more agents:** The `AssessorAgents` registry allows you to plug in additional on‑device models.

---

## 📝 Prompt Used to Generate This App

The following prompt was used to create this prototype. You can reuse it to regenerate or extend the app.

```
Create a single-file HTML/JavaScript app called "AI Offline Assessor" that runs entirely in the browser.

Requirements:
- Use Transformers.js (via CDN) to load two lightweight models on first run:
  1. Xenova/all-MiniLM-L6-v2 for sentence embeddings (~25 MB)
  2. Xenova/flan-t5-small for grammar correction (~75 MB)
- Cache models in IndexedDB so subsequent runs are 100% offline.
- No server calls after initial download. No student data leaves the device.

UI:
- Three text areas: Teacher Question (optional), Teacher Answer, Student Answer.
- Buttons: Assess answer, Load sample, Clear, Copy report.
- Show a model download progress panel.
- Results panel with:
  - Large score out of 10 and a band label (Full / Good / Partial / Weak).
  - Progress bar.
  - Four agent cards: Spelling, Expected-answer matcher, Semantic meaning, Sentence formation.
  - Each card shows score, summary, and findings.
  - Annotated student answer with spelling (red) and grammar (yellow) highlights.
  - Key idea chips showing matched/missing terms from the expected answer.
- Press Ctrl+Enter to assess.

Agents (all run on-device):
1. Spelling: dictionary + domain vocabulary from teacher material.
2. Expected-answer matcher: keyword coverage (75%) + bigram coverage (25%).
3. Semantic meaning: cosine similarity of embeddings + sentence-level alignment.
4. Sentence formation: flan-t5-small generates corrected text, then diff tokens; plus rule-based checks for capitalization and punctuation.
5. Final assessor: weighted scoring (matcher 3.5, semantic 3.0, grammar 2.0, spelling 1.5) out of 10.

Design: clean, modern, responsive, with a two-column layout on desktop.
Include a network status badge and a note that everything runs locally.
```

---

## ⚠️ Notes

- This is a **prototype** for testing and demonstration.
- AI scores are indicative and may vary depending on the rubric or strictness of the teacher.
- For production use, consider adding a service worker for full offline hosting and more robust model fallbacks.
