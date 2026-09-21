# Emoji Mood Classifier

A tiny two-feature classifier built for CPSC 1710 Homework 2 — "From One Pixel to Your Classifier."

## What it is

**Emoji Mood Classifier** (`emoji-mood.html`) looks at a simple cartoon face and decides whether it's **Happy** or **Grumpy**. It's a small step up from the starter `one-pixel.html` lab: instead of one input (brightness), it uses two (eyebrow angle and mouth curve), and instead of picking a single cutoff point, it searches for the best *direction and threshold* across both features.

## How to open and use it

1. Open `one-pixel.html` first — it's the starter lab this project builds on.
2. Then open `emoji-mood.html` in any browser (double-click the file, or drag it into a browser tab). No install, no server, no build step.
3. Drag the two sliders — **Eyebrow angle** and **Mouth curve** — to reshape the face.
4. Watch the **Happy / Grumpy** panel update live, along with the "why" panel underneath it.
5. Click any tile in the **Training data** grid to jump straight to that example.
6. Click **Reset to neutral** to go back to a blank face.

## How it makes a prediction (plain language)

The model was "trained" on 12 example faces I made up and labeled myself (visible in the Training data grid). Training works almost exactly like the One Pixel lab's cutoff search, just extended to two numbers instead of one:

1. It tries many possible **directions** — different ways of weighing "how much do eyebrows count" versus "how much does the mouth count."
2. For each direction, it slides a **threshold** back and forth and counts how many of the 12 example faces it would get wrong.
3. It keeps whichever direction + threshold combination makes the fewest mistakes.

That produces two numbers (weights) — currently about **0.94 for eyebrows** and **0.34 for mouth** — plus a threshold. For any new face, the model multiplies your eyebrow angle and mouth curve by those weights, adds them up into a single **score**, and compares the score to the threshold. The page shows this whole computation (the score line, a bar for each feature's contribution, and the closest training example by distance) so nothing is hidden.

## One limitation I found

Because training gave eyebrows about three times more weight than the mouth, the model can be fooled: a face with a **full, maxed-out frown** but only **mildly raised eyebrows** (eyebrows +15°, mouth -40°) still gets classified as **Happy**. The model isn't reading the face the way a person would — it just found the weighting that made the fewest mistakes on the 12 examples it happened to see, and that weighting under-values the mouth. The page's "why" panel flags this directly whenever the two features disagree, so the limitation is visible rather than hidden.

## Development log (directing Codex)

1. **First version:** asked for a page with two sliders (eyebrow angle, mouth curve), a live SVG face, and a simple "sum the two numbers" rule for Happy/Grumpy. While testing it, found and fixed a bug: the mouth `<input>` slider and the mouth `<path>` in the SVG both had `id="mouth"`, so the JS was quietly rotating the wrong element and drawing `NaN` into the mouth curve.
2. **Made it explainable:** the label-only result was hard to trust, so I asked for the reasoning to be shown — a trained weight for each feature, the actual score computation, and the closest matching training example (distance-based evidence), not just weight-based evidence.
3. **Found a strange prediction and asked for an explanation:** while poking at the sliders I found that a full frown with only slightly raised eyebrows still gets called "Happy." Rather than hide this, I asked for a visible note that appears whenever eyebrows and mouth disagree, explaining which feature the model leans on and why (see *One limitation* above). I also redesigned the training set so one pair of examples shares the exact same face but opposite labels — guaranteeing the model must accept at least one honest mistake, the same lesson as Stop 1's "one odd label" pattern.
4. **After showing it to a classmate:** simplified the visual feedback (colored the result panel so Happy/Grumpy is obvious at a glance), added a **Reset to neutral** button, and made the layout stack cleanly on a phone-sized screen.

## Files

- `one-pixel.html` — the original starter lab (unmodified).
- `emoji-mood.html` — the new classifier built for Stops 3–5.
- `HW02-ANSWERS.md` — written answers for Stops 1, 3, 5, and 6.
- `README.md` — this file.
