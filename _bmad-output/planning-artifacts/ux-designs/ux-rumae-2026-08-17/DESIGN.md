---
name: MANDING
status: final
description: A quiet, single-accent habit tool in the spirit of Apple's restraint — flat neutral chrome everywhere, one glossy grape-violet accent, and exactly one indulgent visual moment (the grape itself) carrying all the product's warmth.
colors:
  grape: "#7A2E8C"
  grape-deep: "#5B2168"
  grape-focus: "#9B4DC7"
  grape-tint: "#EFE1F5"
  ink: "#1D1D1F"
  body-muted: "#6E6E73"
  canvas: "#FFFFFF"
  canvas-soft: "#F5F5F7"
  canvas-recessed: "#EEECF1"
  hairline: "#E0E0E0"
  on-grape: "#FFFFFF"
typography:
  title:
    fontFamily: "SF Pro Display, system-ui, -apple-system, sans-serif"
    fontSize: 24px
    fontWeight: 600
    lineHeight: 1.2
    letterSpacing: -0.2px
  body:
    fontFamily: "SF Pro Text, system-ui, -apple-system, sans-serif"
    fontSize: 17px
    fontWeight: 400
    lineHeight: 1.47
    letterSpacing: -0.2px
  caption:
    fontFamily: "SF Pro Text, system-ui, -apple-system, sans-serif"
    fontSize: 14px
    fontWeight: 400
    lineHeight: 1.4
    letterSpacing: -0.1px
  micro:
    fontFamily: "SF Pro Text, system-ui, -apple-system, sans-serif"
    fontSize: 12px
    fontWeight: 400
    lineHeight: 1.3
    letterSpacing: 0
rounded:
  sm: 8px
  md: 12px
  lg: 18px
  pill: 9999px
  full: 9999px
spacing:
  xs: 8px
  sm: 12px
  md: 16px
  lg: 24px
  xl: 32px
components:
  grape-filled:
    background: "one of 3 user-supplied glossy jelly photos, randomly assigned per slot and kept for its lifetime (data URI, background-size 140% to crop out the source photo's white margin)"
    rounded: "{rounded.full}"
    shadow: "0 3px 6px rgba(0, 0, 0, 0.22)"
  grape-empty:
    background: transparent
    border: "1.5px solid {colors.hairline}"
    rounded: "{rounded.full}"
  button-primary:
    backgroundColor: "{colors.grape}"
    textColor: "{colors.on-grape}"
    typography: "{typography.body}"
    rounded: "{rounded.pill}"
    padding: "11px 22px"
  checklist-row:
    backgroundColor: "{colors.canvas}"
    textColor: "{colors.ink}"
    typography: "{typography.body}"
    border: "1px solid {colors.hairline}"
    rounded: "{rounded.md}"
  cluster-card:
    backgroundColor: "{colors.canvas}"
    rounded: "{rounded.lg}"
    padding: "{spacing.lg} {spacing.md}"
  checklist-section:
    backgroundColor: "{colors.canvas-recessed}"
    rounded: "{rounded.lg}"
    padding: "{spacing.md}"
  toast-celebration:
    backgroundColor: "{colors.ink}"
    textColor: "{colors.canvas}"
    typography: "{typography.caption}"
    rounded: "{rounded.pill}"
    padding: "10px 18px"
---

## Brand & Style

MANDING borrows Apple's discipline — quiet neutral chrome, one accent, no decorative noise — and spends its entire warmth budget on a single object: the grape. Everywhere else (buttons, cards, checklist rows, the review screen) is flat, restrained, textless-icon-free. The grape is the one place the UI is allowed to feel tactile and a little indulgent, because the grape is the whole point: every other pixel exists to let that one glossy dot of proof-of-action shine.

No gamified color-coding, no streak badges, no red error states (nothing here is ever "wrong," only "not yet"). Where `DESIGN-apple.md` was analyzed from Apple's marketing/store pages, its literal components (product tiles, configurator chips) don't transfer to a single-screen habit tool — what transfers is the *rule set*: one accent, one reserved shadow, quiet typography, generous air.

## Colors

- **Grape** (`{colors.grape}` — #7A2E8C) is the only accent. Every actionable element — the [새로운 포도송이 추가] button, filled grape slots, the highlighted next-routine-item — uses it and nothing else. `[ASSUMPTION: substitutes Apple's Action Blue with a grape-violet, since MANDING's product identity is the grape itself — same one-accent restraint, different hue. Confirm or swap back to blue if preferred.]`
- **Grape Deep** (`{colors.grape-deep}`) and **Grape Focus** (`{colors.grape-focus}`) are gradient/press/focus variants of the one accent — never a second brand color.
- **Grape Tint** (`{colors.grape-tint}`) is a pale wash used only behind a checked-today routine checklist row.
- **Ink** (`{colors.ink}` — #1D1D1F) is all text. **Body Muted** (`{colors.body-muted}`) is secondary copy (progress counts, dates).
- **Canvas** (`{colors.canvas}` — white) and **Canvas Soft** (`{colors.canvas-soft}` — #F5F5F7) alternate the same way Apple alternates tile backgrounds — Canvas Soft is now also the 포도밭 page's own background, with Canvas reserved for content sitting on top of it (포도송이 카드, checklist rows, and the 오늘의 리뷰 field cards).
- **Canvas Recessed** (`{colors.canvas-recessed}` — #EEECF1, a whisper of grape hue mixed into a near-neutral) wraps the 루틴 체크리스트 section as its own zone, sitting one step "deeper" than the page so 포도송이 (growing, Canvas cards) and 루틴 (settled, Canvas Recessed zone) read as two different kinds of content at a glance, not one continuous list.
- **Hairline** (`{colors.hairline}`) is the only border weight in the system — 1px, everywhere a division is needed. Grouping between major zones (포도송이 카드 vs. page, 루틴 체크리스트 vs. page) is done with background contrast instead, deliberately — no new border for that. Within a zone, siblings still use a hairline: consecutive 포도송이 카드 get a `border-top` between them (`.cluster-card + .cluster-card`) so a multi-card 포도밭 tab doesn't read as one fused block.

Avoid: a second chromatic accent, colored badges for state, red/orange error fills (FR-4's cap-hit message is calm, not a warning).

## Typography

SF Pro Display/Text per Apple's system, substituted with Inter off-Apple platforms (see `DESIGN-apple.md`'s font-substitute notes — same guidance applies here). Scale is deliberately small since MANDING has no marketing surfaces: `{typography.title}` (24px/600, slight negative tracking) for screen headers and the empty-state prompt, `{typography.body}` (17px/400) for all primary content including grape-cluster labels and checklist rows, `{typography.caption}` (14px) for progress counts (N/15) and dates, `{typography.micro}` (12px) for the "N개의 포도송이 키우는 중" summary line.

## Layout & Spacing

8px-based scale (`{spacing.xs}` 8 → `{spacing.xl}` 32), mirroring `DESIGN-apple.md`. Single column, mobile-portrait only (per PRD §5 Cross-Cutting NFRs) — no responsive breakpoints needed. Generous vertical air between 포도송이 cards (`{spacing.lg}`); tighter rhythm within a checklist (`{spacing.xs}` between rows).

## Elevation & Depth

Apple's rule — exactly one shadow, reserved for the product — applies literally: MANDING's "product" is the filled slot. `grape-filled` carries the system's only shadow (`0 3px 6px rgba(0, 0, 0, 0.22)`, neutral now rather than grape-tinted since the slot's own color comes from its photo), giving filled slots a soft lift. Every other surface (buttons, cards, checklist rows, toasts) is flat — no card shadows, no button shadows.

## Shapes

`{rounded.full}` for every grape slot (filled and empty) — circular, per the user-supplied cluster mockup. Filled slots crop their jelly photo to that same circle via CSS border-radius, no separate image masking needed. `{rounded.pill}` for the primary action button, matching Apple's capsule CTA grammar. `{rounded.md}`/`{rounded.lg}` for checklist rows and the daily-review section cards. Nothing square; nothing sharp.

## Components

- **`grape-filled`** — A completed 젤리. **Superseded the earlier flat-CSS-gradient approach** (a radial gradient approximating a glossy grape, chosen specifically to avoid raster imagery/photorealism) — the user supplied 3 actual glossy jelly photos and asked for them directly, so the resolved "no raster image" style-priority note from `addendum.md` no longer holds for this one element. Each filled slot is randomly assigned one of the 3 photos the first time it's earned, and keeps that look for its lifetime (stored per-slot, not re-randomized on re-render). Every other surface in the system is still flat CSS with no imagery — this is a deliberate, scoped exception for the one element that's always been the system's single "product" moment.
- **`grape-empty`** — An unfilled slot. Transparent fill, `{colors.hairline}` outline only.
- Cluster layout: 20 slots in a 4-column × 5-row grid (revised from the original 15-slot, 5-4-3-2-1 triangular mockup layout), empty slots still rendered in `{colors.hairline}` outline only — the mockup's cream badge and green leaf icon are dropped as out-of-system decoration (see Do's and Don'ts).
- **`button-primary`** — [새로운 포도송이 추가], [오늘 마무리하기], [다시 도전하기]. Grape fill, pill radius, `{typography.body}`.
- **`checklist-row`** — A graduated 포도송이 in the routine list (also reused, without the routine-only checked-state tint/checkbox, for the personal 젤리를 위한 작은 체크리스트 log inside a growing card). Flat card, hairline border. In the routine list specifically, each row carries a small trailing checkbox circle: hollow `{colors.hairline}` outline on white when unchecked, filled `{colors.grape-focus}` with a white ✓ when checked — and the checked row's whole background shifts to `{colors.grape-tint}`, no icon or badge needed. (Superseded a prior "next-due row gets the tint" treatment, which read as ambiguous once checked rows also needed a color.)
- **`cluster-card`** — One growing 포도송이. Solid `{colors.canvas}` background, no border — the white block against the page's Canvas Soft is what reads as "a card," not a hairline.
- **`checklist-section`** — The 루틴 체크리스트's own container. `{colors.canvas-recessed}` background wraps the header and all rows as one visually grouped zone, distinct from both the page and the 포도송이 cards above it.
- **`toast-celebration`** — FR-7/FR-9/FR-10's celebration messages. Ink-on-white pill, transient (auto-dismiss), text-only — no confetti graphics, no icons, matching Apple's text-only state philosophy.

## Do's and Don'ts

| Do | Don't |
|---|---|
| One accent (grape-violet) on every actionable element | A second chromatic accent, color-coded by 포도송이 topic |
| Reserve the system's one shadow for the grape itself | Shadows on cards, buttons, or checklist rows |
| Text-only celebration toasts | Confetti animations, icon badges, streak flames |
| Flat cards everywhere except the grape — border on small rows, background contrast (not new borders) on larger zones | Warm cream/tan backgrounds, multi-hue badges (dropped from the original mockup) |
| Calm, non-error styling on the FR-4 cap-hit message | Red/orange warning colors anywhere in the system |
