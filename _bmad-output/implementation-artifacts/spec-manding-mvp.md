---
title: 'MANDING v1 — single-file habit tracker MVP'
type: 'feature'
created: '2026-08-17'
status: 'done'
review_loop_iteration: 0
baseline_commit: 'NO_VCS'
context:
  - '{project-root}/_bmad-output/planning-artifacts/prds/prd-rumae-2026-08-16/prd.md'
  - '{project-root}/_bmad-output/planning-artifacts/ux-designs/ux-rumae-2026-08-17/DESIGN.md'
  - '{project-root}/_bmad-output/planning-artifacts/ux-designs/ux-rumae-2026-08-17/EXPERIENCE.md'
  - '{project-root}/_bmad-output/planning-artifacts/prds/prd-rumae-2026-08-16/addendum.md'
---

<frozen-after-approval reason="human-owned intent — do not modify unless human renegotiates">

## Intent

**Problem:** MANDING is fully specified (PRD FR-1–FR-19, DESIGN.md tokens, EXPERIENCE.md flows) but no implementation exists yet — greenfield project.

**Approach:** Build the entire MVP as one static `index.html` file (inline `<style>`/`<script>`, zero external dependencies) implementing all FRs from `prd.md`, styled per `DESIGN.md`, behaving per `EXPERIENCE.md`. Persist all state in `localStorage`, keyed by date.

## Boundaries & Constraints

**Always:** Single `index.html` file, no CDN/fonts/JS libraries, no build step, no server. All copy in Korean per PRD/EXPERIENCE microcopy tables. Every FR (1–19) implemented — not a subset. `{grape}` violet accent + flat neutral chrome per DESIGN.md; grape slots are the only shadowed element.

**Ask First:** None — PRD/UX discovery fully resolved all open decisions.

**Never:** No percentage/progress-bar UI (Design Principle 1). No ambient "yesterday's total" display (Non-Goal). No push notifications. No per-grape memos. No second accent color.

## I/O & Edge-Case Matrix

| Scenario | Input / State | Expected Output / Behavior | Error Handling |
|----------|--------------|---------------------------|----------------|
| First launch | No localStorage data | Empty 포도밭: title + [새로운 포도송이 추가] only, no checklist | N/A |
| Tap current-step grape | 포도송이 has 0 grapes today | Slot fills, FR-7 toast, input clears silently (no prompt) | N/A |
| Second tap same day | Same 포도송이 already has today's grape | No slot change, FR-4 calm inline message, no toast | N/A |
| Daily total ties yesterday then exceeds | Today's distinct-포도송이 count crosses yesterday's stored total | FR-9 toast fires once that day | If FR-10 also true same tap, show only FR-10 |
| New all-time record | Today's total ≥ stored all-time max | FR-10 toast; update stored max | Suppresses FR-9 same tap |
| 15th grape on a 포도송이 | Slot count reaches 15/15 | Moves to routine checklist (FR-13), disappears from growing list | N/A |
| Checklist row tap | Graduated item, not yet checked today | Marks checked, no grape, no toast (FR-15) | N/A |
| [다시 도전하기] | Checklist row | Row removed; new 0/15 포도송이 created with same name (FR-16) | N/A |
| [삭제하기] | Checklist row | Row removed permanently, no new 포도송이 | N/A |
| [오늘 마무리하기] | Any state | Opens review form (4 optional fields); save closes the day, persists date-keyed record | N/A |
| Day rollover | App opened on a new calendar date | Grape caps reset for all growing 포도송이; yesterday/all-time totals read from prior stored day | N/A |

</frozen-after-approval>

## Code Map

- `index.html` (new) — entire app: inline `<style>` (DESIGN.md tokens as CSS custom properties), inline `<script>` (state, rendering, localStorage), inline PWA `<link rel="manifest" href="data:application/manifest+json,...">` + meta tags. No existing code to reuse — greenfield.

## Tasks & Acceptance

**Execution:**
- [x] `index.html` -- scaffold document + DESIGN.md tokens as CSS vars (colors/type/spacing/radius) -- visual foundation
- [x] `index.html` -- localStorage data layer: 포도송이 list, per-day grape record, all-time max, review records, all date-keyed -- FR-4/8/9/10/19 depend on accurate day-keyed history
- [x] `index.html` -- 포도밭 render: growing 포도송이 cards (5-4-3-2-1 grape layout, current-step input, N/15 caption) + empty state -- FR-1/2/8/11/12
- [x] `index.html` -- completion interaction: tap-to-fill, daily cap check + calm message, edit/delete via ⋯ overflow -- FR-3/4/5/6
- [x] `index.html` -- celebration system: toast queue with FR-9/FR-10 mutual-exclusion priority rule -- FR-7/9/10
- [x] `index.html` -- graduation + routine checklist: auto-move at 15/15, next-item highlight, check (no grape), 다시 도전하기/삭제하기 -- FR-13/14/15/16
- [x] `index.html` -- 오늘의 리뷰 screen: 4 optional fields, save closes day, past-review read view -- FR-17/18/19
- [x] `index.html` -- unit-style manual pass through the I/O & Edge-Case Matrix above (Playwright-driven, screenshots + console-error assertions)

**Acceptance Criteria:**
- Given a fresh browser profile, when the file is opened, then the empty-state 포도밭 renders with no console errors.
- Given a 포도송이 at 14/15, when its grape is tapped, then it reaches 15/15 and immediately appears in the routine checklist instead of the growing list.
- Given today's total already hit a new all-time max on this tap, when FR-9's condition is also true, then only the FR-10 toast renders.
- Given the app is reopened on a new calendar date, when a previously-capped 포도송이 is viewed, then it accepts a new grape today.

## Design Notes

**PWA under `file://`:** True installability (manifest + service worker) requires a secure context (`https://` or `localhost`); opening `index.html` directly via `file://` cannot register a service worker, so offline-cache/installability is best-effort only in that mode. Implement the manifest (data-URI, per Boundaries) and standard meta tags regardless — they activate correctly once the file is served over http(s) or as a locally-served PWA — and skip service-worker registration entirely rather than shipping a broken/no-op one under `file://`. Document this limitation in an HTML comment near the manifest tag.

**Celebration priority:** compute both FR-9 and FR-10 conditions on every completing tap; if FR-10's is true, show only that toast and suppress FR-9 for the tap, per PRD FR-10 Consequences.

## Verification

**Manual checks (no build/test tooling in scope):**
- Open `index.html` directly in a browser; walk PRD UJ-1 and UJ-2 end-to-end.
- Walk every row of the I/O & Edge-Case Matrix above by hand.
- Resize to a narrow mobile viewport; confirm one-handed layout, 44px+ tap targets, no horizontal scroll.

## Suggested Review Order

**Data layer & persistence**

- Entry point — the date-keyed state shape every FR (4/8/9/10/19) reads and writes.
  [`index.html:422`](../../index.html#L422)

- Load path defensively backfills missing/legacy fields rather than trusting stored shape.
  [`index.html:433`](../../index.html#L433)

- Patch: save failures now surface a calm toast instead of failing silently.
  [`index.html:453`](../../index.html#L453)

**Core completion loop & celebration priority**

- The trickiest function — daily cap, FR-9/FR-10 mutual exclusion, and the patched once-per-day record gate.
  [`index.html:500`](../../index.html#L500)

- `recordShown` flag added so the 🏆 toast can no longer repeat within one day.
  [`index.html:548`](../../index.html#L548)

- Toast queue serializes overlapping celebrations so they never visually collide.
  [`index.html:625`](../../index.html#L625)

**Routine graduation & checklist (FR-13–16)**

- Grape-free, toast-free checking keeps routines free of decision cost.
  [`index.html:563`](../../index.html#L563)

- Patch: 다시 도전하기 now confirms before discarding 15/15 progress, matching 삭제하기.
  [`index.html:972`](../../index.html#L972)

- Permanent removal path, unchanged by this review.
  [`index.html:982`](../../index.html#L982)

**Daily review (FR-17–19)**

- Four optional fields persisted under today's date key; no lock-after-save.
  [`index.html:607`](../../index.html#L607)

- Form-to-state bridge invoked by the [저장] action.
  [`index.html:858`](../../index.html#L858)

**Rendering (peripheral — UI reads the state above, holds no logic of its own)**

- 5-4-3-2-1 grape layout per the user-approved visual mockup.
  [`index.html:654`](../../index.html#L654)

- Cluster card composes grid + progress caption + step row per DESIGN.md tokens.
  [`index.html:670`](../../index.html#L670)

- Home assembles empty state, growing list, checklist, and the finish action.
  [`index.html:731`](../../index.html#L731)

**Accessibility patch**

- Removed `maximum-scale=1` to restore pinch-zoom (WCAG 1.4.4).
  [`index.html:5`](../../index.html#L5)
