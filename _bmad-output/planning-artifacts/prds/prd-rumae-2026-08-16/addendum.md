# Addendum — MANDING PRD

Depth captured during discovery that belongs to a downstream UX spec / architecture doc rather than the PRD narrative itself.

**Design-system priority (resolved 2026-08-17):** where the grape mockups below conflict with `DESIGN-apple.md`, the Apple system wins. Treat the sticker-book cluster board and the jelly-texture grape as mood/inspiration references, not literal specs — adapt (e.g. approximate the jelly look with the Apple palette and restrained CSS) or drop whichever details don't fit, rather than forcing both visual languages to coexist.

## Grape cluster board — visual reference (user-provided mockup, 2026-08-16)

User shared an image mockup of the per-topic grape cluster board:

- Triangular arrangement of 15 circular "grape" slots, laid out in rows of 5-4-3-2-1 (bowling-pin triangle, wide at top).
- Header label above the board: "포도송이 N개째" (this cluster's ordinal — exact meaning of N to be clarified: per-topic ordinal vs. lifetime ordinal across all topics).
- Small pill/badge under the header: "작은 성취 맛보기" ("a taste of small achievement") in a warm tan/cream badge.
- A small green leaf icon at the top of the cluster, vine-stem style.
- Filled slots are solid purple/lavender circles with a darker purple outline; empty slots presumably render as outline-only or lighter fill (mockup shown fully filled as an end-state example).
- Background is a warm off-white/cream (not the cool white of DESIGN-apple.md) — this sticker-book aesthetic reads as a distinct visual register from the Apple-style token set already on file; reconcile during UX/architecture phase (two design languages may coexist for different surfaces, or one should yield).
- **Confirmed during PRD finalize (reconciliation pass):** the tension is structural, not just warm/cool — DESIGN-apple.md is strict single-accent minimalism (one blue, explicitly bans a second accent color) vs. this mockup's multi-hue palette (purple/tan/green/cream). **Resolved:** per the design-system priority note above, DESIGN-apple.md wins — the cream badge / multi-hue palette here should be reinterpreted through Apple's neutral + single-accent tokens rather than shipped as-is.

## Individual grape (포도알) texture reference (user-provided image, 2026-08-17)

User wants each individual 포도알 to render as a glossy, chewy jelly sphere — not a flat sticker dot. Reference image: a single spherical gummy/jelly ball, magenta-to-deep-purple gradient, translucent with visible internal light refraction, wet-looking specular highlight top-right, subtle surface bubble/imperfection texture, soft contact shadow beneath. Reads as a photorealistic jelly candy, not a flat vector icon.

This is in tension with the flatter "solid purple/lavender circle with darker outline" grape-slot rendering implied by the cluster-board mockup above, and with DESIGN-apple.md's flat minimalism generally. **Resolved:** per the design-system priority note above, this is mood reference only, not a literal spec — approximate the glossy/chewy feel with CSS gradients + box-shadow within Apple's palette constraints (no raster image, no full photorealism) rather than pursuing the literal jelly-candy look if it clashes.

## Minor UX notes carried from handoff-prompt_1.md (not FR-level)

- Step-input fields should hint at verb-form phrasing (action-shaped, e.g. "달리기 1.5km 뛰기" rather than a noun label) — original doc leaned on this implicitly through its examples. Worth a placeholder/hint string in the UX spec, not a PRD requirement.
- Interaction: tapping the grape board itself is the completion gesture — there is no separate generic [완료] button. Tapping fills the next empty slot and registers the action as done.

This level of visual/interaction detail exceeds PRD scope (capabilities, not implementation) but must carry forward to whoever builds the UX spec / does the HTML/CSS build.
