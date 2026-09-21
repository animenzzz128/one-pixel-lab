# Homework 2 — Written Answers

*(Working draft — fill in your name/section, then paste into the Canvas submission or export as your "document with your answers.")*

## Stop 1 — Explore One Pixel ML

I used the three built-in **Guided Stops** on the one-pixel.html page (Clean split, One odd label, Reversed labels) to try three labeling patterns.

**Pattern 1 — Clean split**
- *Rule I used:* Label a pixel "light" once its brightness crosses a threshold (roughly the midpoint of the 12 values), "dark" below it. This matches the pixels' natural brightness order.
- *Boundary I expected:* A single cutoff somewhere in the middle of the brightness range that gets every example right.
- *What happened:* Training swept all 60 candidate cutoffs and picked **120** (one of **30 tied, equally accurate options**) with **0 mistakes** — because the labels were already perfectly separable by brightness, many cutoffs in that gap score the same.

**Pattern 2 — One odd label**
- *Rule I used:* Same brightness threshold as Pattern 1, but one example's label was flipped so it contradicts the trend (a mid-brightness pixel labeled "light" even though it's on the "dark" side of the threshold).
- *Boundary I expected:* The model would still land on roughly the same cutoff, but now be forced to accept one wrong answer since no single threshold can satisfy a contradiction.
- *What happened:* Training again chose cutoff **120**, but this time with **1 mistake**. The model didn't chase the odd example — it sacrificed that one point because moving the boundary anywhere else would only create more errors elsewhere.

**Pattern 3 — Reversed labels**
- *Rule I used:* Label pixels backwards relative to brightness — low brightness called "light," high brightness called "dark" — the opposite of the natural association.
- *Boundary I expected:* The model would struggle badly, since a single "darker-than-cutoff vs. brighter-than-cutoff" rule can only represent one direction of relationship.
- *What happened:* This was the most interesting result. Training picked cutoff **0** (one of **23 tied options**) and still made **6 mistakes out of 12** — the worst of all three patterns. Because the classifier can only draw one boundary in one direction, a fully reversed pattern is the one thing it structurally cannot learn; it just settles for calling everything "light" and eating half the errors.

**Screenshot:** the Reversed-labels pattern (cutoff 0, 6 mistakes) — this is the one I found most interesting, since it's the pattern where the model's built-in assumption (brightness increases → more "light") completely fails to match the labels.

**Big idea confirmed:** the same 12 brightness values became three very different learning problems depending only on how I labeled them.

---

## Stop 3 — Choose one tiny classifier

- **My page will classify:** a simple cartoon face's mood.
- **The possible labels are:** happy or grumpy.
- **The classifier will look at:** two features — the eyebrow angle (frowning to raised) and the mouth curve (frowning to smiling).
- **One example it can learn from is:** eyebrows raised (+25°) and mouth curved up (+20°) → happy.
- **A visitor should understand that:** the mood label isn't "reading emotion" — it's a weighted sum of just two shape measurements, learned by testing many possible weightings against a small set of example faces and keeping the one with the fewest mistakes (the same idea as the One Pixel cutoff search, extended to two features instead of one).

---

## Stop 5 — Test like an experimenter

**Easy case:** Eyebrows +35°, mouth +35° (both maxed toward happy) → predicted **Happy**, score 44.9 (needs ≥ 0.0). Exactly what I expected — nothing surprising when both features strongly agree.

**Close case:** Eyebrows -5°, mouth +15° (one of the actual training examples, right near the boundary) → predicted **Happy**, score only **0.4** against a threshold of 0.0. This is about as close a call as the model can make — a tiny nudge either slider would flip it.

**Strange case:** Eyebrows +15° (mildly happy), mouth -40° (a full, maxed-out frown) → predicted **Happy**, score 0.4. This one exposes a real weakness: the trained model leans on eyebrows about 3x more than the mouth (weights 0.94 vs 0.34), so a dramatic frown can still lose to only mildly raised eyebrows. The page's own "why" panel flags this ("Eyebrows and mouth disagree here — the eyebrows matter more to this model"), which is exactly the kind of evidence the assignment asks for — the page doesn't just give a surprising answer, it explains why.

**Classmate test:** *(Do this part yourself — show the live page to a classmate without explaining it first, and ask: (1) what do you think you can change? (2) what do you think the classifier is doing? (3) what is confusing? Jot their answers here.)*
- What they thought they could change:
- What they thought the classifier was doing:
- What was confusing to them:

**Improvement made after testing:** Added a Reset-to-neutral button, colored the Happy/Grumpy result panel so the outcome is visible at a glance, and made the whole layout stack cleanly on a phone-width screen.

---

## Stop 6 — Final reflection

*(Write this section yourself, without AI assistance, per the assignment instructions — 2-4 sentences on what choice or correction improved the project and what you learned.)*
