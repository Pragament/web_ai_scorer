# Sample Q&A Pairs for Testing

Use these test cases to verify that each agent behaves as expected. Each case lists the **question**, **teacher answer**, **student answer**, and the **expected behavior / score band**. The cases are deliberately varied to exercise every agent.

---

## ✅ Baseline — Should Score Well

### Case 1 · Perfect Match (expect 9–10/10)
**Question:** Why did the villagers rebuild the bridge after the flood?
**Teacher:** The villagers rebuilt the bridge after the flood because it was the only route connecting their village to the town. Without it, they could not sell their crops, buy supplies, or reach the hospital in an emergency.
**Student:** After the flood, the villagers rebuilt the bridge because it was the only way to connect their village to the town. Without it, they could not sell crops, buy supplies, or get to the hospital in an emergency.

**Tests:** All agents at high scores. Semantic and matcher should be near 100%.

---

### Case 2 · Different Words, Same Meaning (expect 8–10/10)
**Question:** Why is photosynthesis important for plants?
**Teacher:** Photosynthesis is important because it lets plants convert sunlight into food, and it releases oxygen into the atmosphere.
**Student:** Plants need photosynthesis to turn light into energy for themselves. It also puts oxygen into the air.

**Tests:** Agent 3 (semantic) should score high despite low exact‑word overlap. Agent 2 (matcher) may score moderate.

---

### Case 3 · Strong Content, Minor Spelling (expect 8–9/10)
**Question:** What caused the American Civil War?
**Teacher:** The American Civil War was mainly caused by disagreements over slavery, states' rights, and economic differences between the North and South.
**Student:** The Civil War was mostly caused by disagreemants over slavery, states rights, and econmic differances between the North and the South.

**Tests:** Agent 1 (spelling) should catch *disagreemants*, *econmic*, *differances*. Other agents should score high.

---

## ⚠️ Partial Credit — Realistic Student Answers

### Case 4 · Main Idea Only (expect 5–7/10)
**Question:** Why did the villagers rebuild the bridge after the flood?
**Teacher:** The villagers rebuilt the bridge after the flood because it was the only route connecting their village to the town. Without it, they could not sell their crops, buy supplies, or reach the hospital in an emergency.
**Student:** The villager rebuild the brigde becaus it was the only way to conect there village to town. They cannot sell crops, buy supplys or go hospital in emergency.

**Tests:** The original sample. Spelling errors, grammar issues, but all key ideas present.

---

### Case 5 · Missing a Key Idea (expect 5–6/10)
**Question:** What are the three branches of the U.S. government?
**Teacher:** The three branches are the legislative branch (makes laws), the executive branch (enforces laws), and the judicial branch (interprets laws).
**Student:** The three branches are legislative, executive, and judicial.

**Tests:** Agent 2 (matcher) should flag missing functions (makes / enforces / interprets laws). Agent 3 (semantic) should give partial credit.

---

### Case 6 · Correct but Very Short (expect 4–6/10)
**Question:** What is the water cycle?
**Teacher:** The water cycle is the continuous movement of water through evaporation, condensation, precipitation, and collection.
**Student:** It is when water moves around.

**Tests:** Agent 3 (semantic) has thin‑answer penalty. Agent 2 (matcher) misses most key terms.

---

### Case 7 · Grammar Problems Only (expect 6–8/10)
**Question:** Why do we need to eat vegetables?
**Teacher:** We need to eat vegetables because they provide vitamins, minerals, and fiber that help our bodies stay healthy and fight disease.
**Student:** we need eat vegetables becuase they give vitamins minerals and fiber that help our body stay healthy and fight disease

**Tests:** Agent 4 (grammar) should flag missing capital, missing "to", missing final period, missing Oxford comma. Agent 1 (spelling) flags *becuase*.

---

## 🚨 Edge Cases — Must Handle Gracefully

### Case 8 · Negation Flip (expect 2–4/10)
**Question:** Why did the villagers rebuild the bridge after the flood?
**Teacher:** The villagers rebuilt the bridge after the flood because it was the only route connecting their village to the town. Without it, they could not sell their crops, buy supplies, or reach the hospital.
**Student:** The villagers rebuilt the bridge because they could sell crops, buy supplies, and reach the hospital easily.

**Tests:** Agent 3 (semantic) must detect the negation mismatch. This is the most important trap case — the student's answer says the *opposite* of the teacher's.

---

### Case 9 · Off‑Topic (expect 0–2/10)
**Question:** Why did the villagers rebuild the bridge after the flood?
**Teacher:** The villagers rebuilt the bridge after the flood because it was the only route connecting their village to the town. Without it, they could not sell their crops, buy supplies, or reach the hospital in an emergency.
**Student:** The Industrial Revolution began in Britain in the eighteenth century and changed how goods were made.

**Tests:** All agents should score very low. Semantic and matcher should be near zero.

---

### Case 10 · Copied Teacher Answer (expect 9–10/10)
**Question:** What is gravity?
**Teacher:** Gravity is the force that attracts objects with mass toward each other. On Earth, it pulls objects toward the center of the planet.
**Student:** Gravity is the force that attracts objects with mass toward each other. On Earth, it pulls objects toward the center of the planet.

**Tests:** Should score full marks. Baseline for "reference" comparison.

---

### Case 11 · Empty Student Answer (expect 0/10)
**Question:** What is gravity?
**Teacher:** Gravity is the force that attracts objects with mass toward each other.
**Student:** *(blank)*

**Tests:** Each agent should return a "No answer submitted" state without crashing.

---

### Case 12 · Gibberish (expect 0–1/10)
**Question:** What is the capital of France?
**Teacher:** The capital of France is Paris, which is the largest city in the country.
**Student:** asdf qwer zxcv paris france blah blah random words.

**Tests:** Spelling agent flags nonsense. Matcher gets partial credit for *paris* and *france*. Semantic stays low.

---

## 🧪 Agent‑Specific Stress Tests

### Case 13 · Spelling Hell (tests Agent 1)
**Question:** Describe the water cycle.
**Teacher:** Water evaporates, condenses into clouds, falls as precipitation, and collects in rivers and oceans.
**Student:** Watter evaperates, condences into cloudz, fallz as presipitashun, and colects in rivvers and ocens.

**Expected:** Spelling agent catches 8+ errors. Other agents still give partial credit because meaning survives.

---

### Case 14 · Semantic Only (tests Agent 3)
**Question:** Why is exercise good for you?
**Teacher:** Exercise is good for you because it strengthens the heart, improves mood, and helps control weight.
**Student:** Working out makes your ticker stronger, lifts your spirits, and keeps you from getting fat.

**Expected:** Agent 2 (matcher) scores low (few shared words). Agent 3 (semantic) scores high. Overall should be 7–9.

---

### Case 15 · Grammar Only (tests Agent 4)
**Question:** Describe the water cycle.
**Teacher:** Water evaporates, condenses into clouds, falls as precipitation, and collects in rivers and oceans.
**Student:** water evaporate condense into cloud fall as precipitation and collect in river and ocean

**Expected:** Content is nearly correct but grammar is broken (missing plurals, missing past tense, no capitals, no punctuation). Agent 4 scores low; others score moderate.

---

### Case 16 · Matcher Only (tests Agent 2)
**Question:** List three renewable energy sources.
**Teacher:** Three renewable energy sources are solar, wind, and hydroelectric power.
**Student:** Renewable energy includes solar, wind, and hydro power, plus geothermal and biomass.

**Expected:** Matcher gives high coverage (all three key ideas plus extras). Grammar and spelling clean. Should score 9–10.

---

## 📋 Quick Test Table

| # | Case | Focus | Expected Band |
|---|------|-------|---------------|
| 1 | Perfect match | All agents | Full (9–10) |
| 2 | Different wording | Semantic | Full (8–10) |
| 3 | Minor spelling | Spelling | Full (8–9) |
| 4 | Main idea only | All agents | Partial (5–7) |
| 5 | Missing key idea | Matcher | Partial (5–6) |
| 6 | Very short | Semantic | Partial (4–6) |
| 7 | Grammar only | Grammar | Good (6–8) |
| 8 | Negation flip | Semantic | Weak (2–4) |
| 9 | Off‑topic | All agents | Little (0–2) |
| 10 | Copied answer | Baseline | Full (9–10) |
| 11 | Empty | Robustness | None (0) |
| 12 | Gibberish | Robustness | Little (0–1) |
| 13 | Spelling hell | Spelling | Partial (4–6) |
| 14 | Semantic only | Semantic | Good (7–9) |
| 15 | Grammar only | Grammar | Partial (4–6) |
| 16 | Matcher only | Matcher | Full (9–10) |

---

## 🧠 How to Use These Tests

1. **Sanity pass** — Run cases 1, 10, 11 first. Confirm full marks, full marks, and zero.
2. **Agent isolation** — Run cases 13–16 to confirm each agent moves independently.
3. **Adversarial pass** — Run cases 8 and 9. These are the ones most likely to expose weaknesses in semantic scoring.
4. **Regression** — Save the scores from a run and compare after any model or weight change.
5. **Rubric tuning** — If Case 8 (negation flip) scores too high, increase the semantic weight or lower the matcher weight.

---

## 💡 Adding Your Own

When adding a test case, try to include:

- The **core idea** the student must express.
- At least one **spelling variation** of a key term.
- At least one **synonym** for the expected vocabulary.
- One **grammar trap** (tense, plural, article).
- One **structural trap** (missing detail, added detail, reversed meaning).

That combination will stress every agent in the pipeline with a single case.
