# Deferred Work

- source_spec: `_bmad-output/implementation-artifacts/spec-manding-mvp.md`
  summary: `dailyTotals`, `dailyToast`, `reviews`, and now per-cluster `memoEntries` grow forever in localStorage with no pruning or archiving.
  evidence: Surfaced by step-04 blind-hunter and edge-case-hunter reviews independently. Low near-term risk (hundreds of small date-keyed entries/year) but unbounded over years of daily use.

- source_spec: `_bmad-output/implementation-artifacts/spec-manding-mvp.md`
  summary: `loadState()` does not filter out corrupted/non-object entries inside `parsed.clusters`, which could crash `render()` on malformed localStorage data.
  evidence: Surfaced by step-04 edge-case-hunter review; requires externally corrupted storage to trigger, not reachable through normal app usage.

- source_spec: `_bmad-output/implementation-artifacts/spec-manding-mvp.md`
  summary: No cross-tab sync — two tabs of the same app open at once can silently overwrite each other's saved state.
  evidence: Surfaced by step-04 edge-case-hunter review; low likelihood for a single-user habit app typically used in one tab.

- source_spec: `_bmad-output/implementation-artifacts/spec-manding-mvp.md`
  summary: Rapid repeated grape taps can queue many toasts back-to-back with no cap on the queue length.
  evidence: Surfaced by step-04 edge-case-hunter review; self-resolving (each toast only shows ~2s) and low real-world impact.

- source_spec: `_bmad-output/implementation-artifacts/spec-manding-mvp.md`
  summary: `apple-touch-icon` is an SVG data URI; iOS Safari requires PNG for home-screen icons, so the icon likely won't display correctly there.
  evidence: Surfaced by step-04 blind-hunter review; a known iOS constraint, functionality unaffected, cosmetic only, and PWA support was already scoped best-effort in Design Notes.

- source_spec: `_bmad-output/implementation-artifacts/spec-manding-mvp.md`
  summary: Modal action sheets lack `role="dialog"`/`aria-modal`, a focus trap, Escape-to-close, and focus return to the triggering element.
  evidence: Surfaced by step-04 blind-hunter review; real accessibility gap, lower priority for a single-user personal tool than a multi-user product.

- source_spec: `_bmad-output/implementation-artifacts/spec-manding-mvp.md`
  summary: True calendar day-rollover (spanning a real midnight) and PWA installability under a real http(s)/localhost origin were never actually exercised — only reasoned about structurally and via seeded localStorage snapshots.
  evidence: Surfaced by step-04 verification-gap review; both are practically hard to exercise in an automated single-file, no-build-tooling project. Worth a one-time manual check (e.g. temporarily changing system date, or serving the file over `localhost`) if the user wants extra confidence.
