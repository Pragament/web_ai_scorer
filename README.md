# AI Offline Assessor

A browser‑based prototype that assesses student answers using **on‑device AI agents**. It is designed to run entirely locally after an initial one‑time setup, with no student data ever leaving the device.

[![Offline Ready](https://img.shields.io/badge/offline-ready-brightgreen)]()
[![On-Device AI](https://img.shields.io/badge/AI-on--device-blue)]()
[![No Server](https://img.shields.io/badge/server-none-lightgrey)]()

---

## 🎯 Goals

- Provide **instant, partial‑credit feedback** on short student answers.
- Run **100% on the user’s device** after initial setup — no uploads, no server calls.
- Work **offline** once the required assets are cached.
- Keep the footprint **small and fast** so it can run in a normal browser tab.
- Use **multiple specialised agents** that share lightweight components and produce a transparent, weighted score.
- Give teachers and students an **explainable** result: what was correct, what was missing, and why the score was awarded.

---

## 📋 Requirements

### Functional

- **Input fields**
  - Teacher question (optional)
  - Teacher answer (expected / model answer)
  - Student answer (to be assessed)

- **Assessment output**
  - A total score out of 10.
  - A qualitative band (e.g., Full credit, Good, Partial, Weak, Little/no credit).
  - A breakdown per agent with individual scores and short explanations.
  - A closeness estimate showing how near the student answer is to the expected answer.

- **Agent breakdown** (five logical agents)
  1. **Spelling** – detect and report spelling errors.
  2. **Expected‑answer matcher** – measure coverage of key ideas and phrases.
  3. **Semantic meaning** – compare meaning, not just exact words.
  4. **Sentence formation / grammar** – detect grammar and sentence‑level issues.
  5. **Final assessor** – combine agent outputs using a weighted formula.

- **Annotated view**
  - Highlight spelling and grammar issues directly in the student answer.
  - Hovering a highlight shows the suggested fix.

- **Key‑idea view**
  - Show which expected concepts were covered and which were missed.

- **Controls**
  - Assess answer
  - Load sample
  - Clear
  - Copy report (plain‑text export)
  - Keyboard shortcut: `Ctrl + Enter` to assess

- **Model / asset setup**
  - On first use, download any required models or assets.
  - Show progress and status.
  - Cache everything so later runs need no network.

### Non‑Functional

- **Offline‑first**: after initial setup, the app must work with no internet connection.
- **Privacy‑preserving**: no student answer, teacher answer, or derived data is sent to any server.
- **Browser‑only**: no installation, no backend, no build step required to run.
- **Lightweight**: models and assets should be as small as practical for a browser environment.
- **Responsive**: usable on desktop and tablet; layout adapts to smaller screens.
- **Fast**: assessment should complete in a few seconds on a typical device.
- **Extensible**: new agents or models can be added without rewriting the whole app.

### Offline & Privacy

- All inference runs locally in the browser.
- Models are cached in the browser (e.g., IndexedDB).
- No analytics, no tracking, no external API calls after setup.
- A network status badge indicates whether the app is online or offline‑ready.

---

## 🧩 Features

- Five independent agents with weighted scoring.
- Partial credit with transparent reasoning.
- Inline annotation of spelling and grammar problems.
- Key‑idea coverage chips (hit / miss).
- Copyable plain‑text report.
- Works fully offline after first load.
- No account, no sign‑in, no server.

---

## 🖥 User Interface Overview

- **Two‑column layout** on desktop; single column on mobile.
- **Left panel**: input fields and action buttons.
- **Right panel**: assessment results, agent cards, annotated answer, key‑idea chips.
- **Top bar**: app title, offline/online badge, privacy note.
- **Setup panel** (first run only): model download progress.

---

## 📊 Assessment Model (Conceptual)

- Each agent returns a score between 0 and 1.
- Each agent has a **weight** reflecting its importance:
  - Expected‑answer matcher: 3.5
  - Semantic meaning: 3.0
  - Sentence formation / grammar: 2.0
  - Spelling: 1.5
- Weighted total is scaled to **out of 10**.
- Bands:
  - 9–10: Full credit
  - 7–8.9: Good — minor gaps
  - 5–6.9: Partial credit
  - 3–4.9: Weak — major gaps
  - 0–2.9: Little / no credit
- The final assessor also reports a **closeness percentage** based on idea coverage and semantic similarity.

> Note: AI scores are indicative and may vary depending on the rubric or strictness of the teacher.

---

## 🚀 Getting Started

1. Save the provided HTML file as `index.html`.
2. Open it in a modern browser.
3. Wait for the initial setup (model download) to finish.
4. Paste a teacher answer and a student answer.
5. Click **Assess answer** or press `Ctrl + Enter`.
6. After setup, disconnect from the internet — the app will continue to work offline.

---

## 🛠 Customization (High‑Level)

- Replace or add agents by implementing the same interface.
- Adjust agent weights to match your rubric.
- Extend the spelling dictionary or domain vocabulary.
- Swap in different on‑device models if higher accuracy is needed.

---

## 📄 License

MIT — free to use, modify, and distribute.

---

## 📝 Prompt

Use the following prompt to generate or regenerate this application. It is intentionally approach‑agnostic: it describes **what** the app should do and **how it should behave**, without prescribing specific libraries, models, or algorithms.

```
Create a single-file HTML/JavaScript application called "AI Offline Assessor".

Purpose:
Assess short student answers using multiple on-device AI agents. The app must run entirely in the browser, work offline after an initial one-time setup, and never send student data to a server.

Goals:
- Provide instant, partial-credit feedback on short student answers.
- Run 100% on the user’s device after initial setup.
- Work offline once required assets are cached.
- Keep the footprint small and fast.
- Use five specialised agents that share lightweight components.
- Produce an explainable, weighted score.

Inputs:
- Teacher question (optional)
- Teacher answer (expected / model answer)
- Student answer (to be assessed)

Outputs:
- Total score out of 10.
- Qualitative band (Full credit, Good, Partial, Weak, Little/no credit).
- Per-agent breakdown with scores and short explanations.
- Closeness estimate to the expected answer.
- Annotated student answer with spelling and grammar highlights.
- Key-idea chips showing which expected concepts were covered or missed.

Agents (five logical agents):
1. Spelling – detect and report spelling errors.
2. Expected-answer matcher – measure coverage of key ideas and phrases.
3. Semantic meaning – compare meaning, not just exact words.
4. Sentence formation / grammar – detect grammar and sentence-level issues.
5. Final assessor – combine agent outputs using a weighted formula.

Weighted scoring:
- Expected-answer matcher: 3.5
- Semantic meaning: 3.0
- Sentence formation / grammar: 2.0
- Spelling: 1.5
Total scaled to 10. Bands: 9–10 Full, 7–8.9 Good, 5–6.9 Partial, 3–4.9 Weak, 0–2.9 Little/no credit.

UI:
- Two-column layout on desktop; single column on mobile.
- Left panel: input fields and buttons (Assess answer, Load sample, Clear, Copy report).
- Right panel: assessment results, agent cards, annotated answer, key-idea chips.
- Top bar: app title, offline/online badge, privacy note.
- Setup panel (first run only): model download progress.
- Keyboard shortcut: Ctrl+Enter to assess.

Offline & privacy:
- All inference runs locally in the browser.
- Models are cached in the browser (e.g., IndexedDB).
- No analytics, no tracking, no external API calls after setup.
- A network status badge indicates online/offline state.

Non-functional:
- Browser-only, no installation or backend.
- Responsive and fast.
- Extensible: new agents or models can be added without rewriting the whole app.

Implementation notes:
- Use any lightweight on-device models or algorithms that fit the browser environment.
- Cache models and assets so subsequent runs are fully offline.
- Show a clear first-run setup experience with progress.
- Provide a copyable plain-text report.

Deliver a single, self-contained HTML file with embedded CSS and JavaScript.
