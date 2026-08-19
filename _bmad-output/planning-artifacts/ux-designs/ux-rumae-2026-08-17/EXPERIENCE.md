---
name: MANDING
status: final
sources:
  - "{planning_artifacts}/prds/prd-rumae-2026-08-16/prd.md"
  - "{planning_artifacts}/prds/prd-rumae-2026-08-16/addendum.md"
updated: 2026-08-17
---

# MANDING — Experience Spine

## Foundation

Single-surface mobile web app, installed as a PWA (standalone, no browser chrome). Portrait-only, one-handed operation. No UI system named — vanilla HTML/CSS/JS, single static file, no framework. `DESIGN.md` is the visual identity reference; this spine covers behavior. No dark mode in v1 `[ASSUMPTION: not requested; add later if needed]`.

## Information Architecture

| Surface | Reached from | Purpose |
|---|---|---|
| 포도밭 (Home) | App open (always) | An in-page 포도밭/루틴 segmented control (`ui.homeTab`, not a route — no push/pop, no URL/view change) switches between growing 포도송이 (current-step cards) and the graduated routine checklist in place. The only surface most days ever need. |
| 오늘의 리뷰 | [오늘 완료하기] on Home | Write the day's 4-section review, confirms/closes the day. |
| 지난 리뷰 | Small history affordance on Home | Read past daily reviews (date-keyed, per FR-19). Read-only. |

No tab bar across top-level surfaces — 포도밭 is still home base, and 오늘의 리뷰/지난 리뷰 are reached-and-returned-from, never persistent chrome (modal/full-screen push, one level deep). The 포도밭/루틴 control is a narrower, deliberate exception: it lives entirely inside the 포도밭 surface, never changes `ui.view`, and exists to solve a single complaint — 포도송이 cards and the 루틴 체크리스트 read as one continuous, hard-to-tell-apart scroll. Shared chrome ([지난 리뷰], [오늘 완료하기], [편집] when applicable) stays constant across both tabs; only the [+ 추가] button's destination and label change with the active tab ([+ 추가] → 새 포도송이 modal on 포도밭, 새 루틴 modal on 루틴). Switching tabs resets `ui.editMode` to false, since reordering only applies to growing 포도송이.

→ Composition reference: none rendered this pass (Fast path, short UX request) — build directly from this spine + `DESIGN.md` component tokens.

## Voice and Tone

Brand posture lives in `DESIGN.md`. Microcopy here:

| Do | Don't |
|---|---|
| "어제보다 더 나은 오늘이네요! ✨" | "오늘도 화이팅! 💪" |
| "오늘 완료! 내일 만나요:D" | "오늘 한도 초과!" / any warning-toned copy |
| "이 걸음, 새로 써볼까요?" (수정 시, 크기를 단정하지 않음) | "실패했습니다. 다시 시도하세요." |
| "작은 할 일들을 다 완료했나요?" (완료 확인, 강요 없는 질문형) | |
| Short, complete sentences, never scolding | Streak language ("3일 연속!"), guilt language ("놓치셨어요") |

## Component Patterns

Behavioral; visual specs live in `DESIGN.md.Components`.

| Component | Use | Behavioral rules |
|---|---|---|
| 포도밭/루틴 탭 | Home, directly under the header, above everything else | Two `.home-tab-btn` pills spanning the full width, matching a user-supplied mockup exactly: active tab is grape-filled with white text, inactive tab is white with a grape outline/text. Switches `ui.homeTab` only — no navigation, no `ui.view` change, no scroll-position loss. Exists specifically to stop 포도송이 카드 and 루틴 체크리스트 reading as one undifferentiated scroll (a complaint raised directly). Switching tabs resets 편집 mode off, since reordering only applies to growing 포도송이. |
| 포도송이 카드 | Home, one per growing topic, visible only while the 포도밭 tab is active | Header row: centered topic name with a `⋯` overflow button in the corner (a same-width invisible spacer on the opposite side keeps the name mathematically centered regardless of length/wrapping). Below that: `{grape-filled}/{grape-empty}` × 20 in a 4-column × 5-row grid (revised from the original 5-4-3-2-1 triangular mockup layout), current-step input, an "젤리를 위한 작은 체크리스트" to-do log, and a fixed [수행 완료] button, N/20 caption (FR-11, cap raised from 15). Grape-earning is fully decoupled from the to-do log (see rows below) — [수행 완료] or the next empty grape slot always opens the completion-confirm modal (revised FR-3), regardless of what's queued in the log. |
| 포도송이 카드 `⋯` 메뉴 | Corner of every growing 포도송이 card | Opens [수정] / [메모] / [삭제] / [취소] (삭제 muted, matching the pending-step/checklist overflow precedent). [수정] renames the cluster via a neutral question-form modal ("포도송이 이름을 새로 정해볼까요?", reuses `.text-input`, 40-char cap matching creation). [메모] opens a dated personal log, not a single free-text field: past entries render oldest-first as separate boxes, each stamped with its own date caption (`formatDateDots`) and its own `⋯` overflow for [수정]/[삭제] (same action-sheet pattern as the checklist/pending-step rows, no confirm dialog on delete — low-stakes personal notes); a `<textarea>` beneath the list (500-char cap, placeholder "이 포도송이에 대해 기록해두고 싶은 게 있나요?") adds a new entry dated today on [추가], multiple entries per day allowed, blank/whitespace-only submissions are silently ignored. Canceling out of an entry's `⋯` menu or its edit modal returns to the memo list (a dedicated back-to-memo action) rather than closing the whole sheet. A small dot on the cluster's `⋯` button (single accent color, no new icon shape) appears only while at least one memo entry exists — the app's one state-indicator glyph, added deliberately after weighing it against the no-clutter principle below. [삭제] removes the cluster entirely after a `window.confirm` ("\"<name>\" 포도송이를 삭제할까요? 되돌릴 수 없어요.", same register as the routine checklist's own 삭제하기) — irreversible, no undo. Renaming/memo/delete all close FR-1/deferred-work.md's original growing-cluster gap ("no way to delete or rename a growing 포도송이"). A 다시 도전하기 restart carries the old cluster's memoEntries forward in full, same as it already carries the name. |
| 현재 걸음 입력 | Inside each 포도송이 card | Persistent — never disappears after a step is added (revised FR-5), so several 작은 행동 can be queued back to back without re-tapping into the field; stays focused after each `[✓]`/Enter commit. Placeholder text "작게 시작해볼까요?" invites input in question form without presuming a size or example. Committing adds the text as a new row at the bottom of the "젤리를 위한 작은 체크리스트" log below, rather than replacing a single current step. |
| "젤리를 위한 작은 체크리스트" 로그 | Below the input, inside each growing 포도송이 card | Section label "젤리를 위한 작은 체크리스트" always renders, even with zero items. A personal to-do log, fully decoupled from grape-earning: each row has 걸음 text, a toggle pill (personal "했음" marker — no grape, no toast, doesn't remove the row), and the same `⋯` overflow as the routine checklist. New items always append to the bottom (oldest reads top, newest bottom); the row itself (text) carries over across days indefinitely until explicitly deleted, but the toggle's checked state is day-scoped — it resets to unchecked on day rollover, same `lastCheckedDate`-vs-today pattern as the routine checklist. `⋯` reveals [수정] / [삭제] (FR-6); [수정] opens a neutral replace-step input, [삭제] removes the row. Once today's grape is earned (`lastFilledDate === today`), every row in the log flattens to a plain gray, text-only box — no toggle, no `⋯` — so a claimed day can't quietly be re-edited; tapping [취소] (below) is the only way back into an editable state before tomorrow. |
| [수행 완료] 버튼 | Bottom of each growing 포도송이 card, below the "젤리를 위한 작은 체크리스트" log | Full-width primary button, present regardless of the log's contents (even with zero items queued) — but only while there's still room under the cap AND today's jelly isn't earned yet; past either condition it's replaced by [취소] and/or the graduate prompt (see rows below), never shown alongside them. Tapping it (when today's jelly isn't already earned) normally opens a confirm modal — "작은 할 일들을 다 완료했나요?" / [Yes], with two mutually-exclusive checkboxes beneath the body text, side by side: "하루 동안 보지 않기" and "일주일 간 보지 않기" (checking one unchecks the other). Confirming [Yes] does two things together: fills a jelly slot (firing FR-7/9/10 toasts exactly as before) and checks every row in the "젤리를 위한 작은 체크리스트" log as done-today, regardless of their individual toggle state beforehand. The modal is skipped straight to that same [Yes] effect in two cases: (1) the log has at least one item and every item is already toggled done-today — checking each one off already answered the question, so asking again would be redundant; (2) a global `skipConfirmUntil` suppression is active from a prior checkbox choice (today + 1 day, or today + 7 days for the week option) — until that date passes, every cluster's [수행 완료] tap skips the modal. Checking a box but tapping [취소] instead of [Yes] does not activate the suppression — only a real [Yes] commits it. If today's cap is already hit, tapping shows the calm "오늘 완료" message directly, no modal. Tapping the next empty slot is kept as an equivalent shortcut into the same confirm flow. |
| [취소] 버튼 | Replaces [수행 완료] (not beside it) once today's jelly is actually earned — sits above the graduate prompt, full-width, when both apply | Undo for a mistaken [수행 완료] tap. Reverses grapeCount, that slot's earned date and jelly-look assignment (`grapeVariants`), `lastFilledDate`, today's `dailyTotals` contribution — putting it back to exactly "not yet completed today," at which point [수행 완료] reappears in [취소]'s place so the flow can be repeated correctly. **No longer touches `graduated`** — reaching the cap doesn't auto-graduate anymore (see the row below), so by the time [취소] could ever be tapped the cluster is never already graduated. Doesn't touch `allTimeMax` or the FR-9/10 once-per-day toast flags (not worth perfectly rewinding for a personal habit tracker); doesn't touch the "젤리를 위한 작은 체크리스트" toggle states either — those stay user-editable independently. No confirmation dialog — undoing a just-made mistake should be as frictionless as making it. |
| [젤리를 다 모았어요! 루틴으로 이동!] 버튼 | Bottom of the card, once `grapeCount` reaches the cap — below [취소] when today's fill is what reached it, alone otherwise | **Fixes a bug caught during this session's testing**: reaching the cap used to auto-graduate the cluster in the same tap that filled the last slot, which moved the card straight to the 루틴 tab before the user ever had a chance to hit [취소] on a mistaken tap. Graduating is now a separate, deliberate step — the card sits at the cap (all slots visibly full) until this button is tapped. Tapping it sets `graduated = true` and resets `lastCheckedDate` to null (so it starts "not yet checked today" in the routine list), then the card leaves the growing list on the next render. No confirm dialog and no undo once tapped — same low-friction register as the rest of the completion flow, and [취소] was already the safety net for the one mistake this step could compound (an accidental extra fill). |
| [+ 추가] 버튼 | Home, persistent, top-right in the header row (compact pill, "+ 추가") | Shared across both tabs, but not static — its destination and label follow the active 포도밭/루틴 tab: opens the single-field 새 포도송이 name input (FR-1) on 포도밭, or the 새 루틴 name input on 루틴. No further setup fields either way. Deliberately small/secondary next to the 지난 리뷰 link — the empty-state screen keeps its own full-width primary CTA since it's the only action available there. |
| [편집] 버튼 → 순서 편집 모드 | Home header, next to 지난 리뷰 — only rendered when ≥2 growing 포도송이 exist | Tapping [편집] swaps the whole header for just a [완료] button and replaces every growing 포도송이 card with a compact draggable row: a `⠿` drag handle, name, N/15. **Reorders via drag only (Pointer Events), by explicit user request — this deliberately reverses the no-drag/no-swipe principle below for this one surface.** The routine checklist and 오늘 완료하기 button are hidden in this mode to keep it focused on reordering. Dragging live-previews the reflow (other rows slide via `transform` to open a gap); the real array only updates on drop (`commitClusterReorder`), which re-threads the new growing order back into storage while leaving every graduated cluster's absolute slot untouched — reordering growing items never disturbs the routine checklist's order. [완료] returns to the normal 포도밭 view; new order persists immediately on drop. **Known accessibility gap:** there is no keyboard or screen-reader-operable way to reorder — drag is the only path, an accepted tradeoff for this personal single-user tool. |
| 루틴 체크리스트 | Home, shown only while the 포도밭/루틴 tab is on 루틴 — no header of its own (the tab label plus the shared [+ 추가] cover that job now) | A `{colors.canvas-recessed}` zone wraps the rows, letting a routine the user already has going skip the grow-a-포도송이 metaphor entirely: `createRoutine` inserts it already graduated (`grapeCount: 15`, all-null `grapeDates`, matching how any legacy dateless grape renders). Zero-routine state shows "아직 등록한 루틴이 없어요." instead of an empty rows list. The very first empty-clusters screen (FR-1) also carries a secondary link, "이미 하고 있는 루틴이 있나요? 바로 추가하기", so a user with no interest in growing anything new still has a way in. Each row now carries its own explicit checkbox circle at the trailing edge (before `⋯`): unchecked is a hollow `{colors.hairline}`-outlined circle on a white row; checked is a filled `{colors.grape-focus}` circle with a white ✓, and the whole row picks up a `{grape-tint}` background — replacing the earlier FR-14 "next-due row gets the tint" highlight, which read as ambiguous once a checked state also needed a visual (was this row tinted because it's next, or because it's done?). Tap row to check off (FR-12/FR-15 — no grape, no toast); tapping an already-checked row again toggles it back off, so an accidental tap is never stuck until tomorrow. Row has a small overflow control for [다시 도전하기]/[삭제하기] (FR-16), unchanged for directly-added routines. |
| 오늘 완료하기 버튼 | Home, persistent, bottom | Always tappable regardless of 포도송이 state (FR-17). Opens 오늘의 리뷰. |
| 오늘의 리뷰 폼 | 오늘의 리뷰 surface | 4 stacked optional textareas on `{colors.canvas-soft}` background, each a `{colors.canvas}` card (FR-18). Placeholders: 잘한 점 "이런 점은 잘했어요." · 개선할 점 "내일은 이렇게 해볼까요?" · 감사일기 "감사했던 순간이 있었나요?" · 확언 "지금의 나에게 선언해보세요." Single [저장] action closes the day (FR-19). |
| 축하 토스트 | Global overlay, any surface | Text-only, auto-dismiss ~2s (FR-7/9/10). |

## State Patterns

| State | Surface | Treatment |
|---|---|---|
| No 포도송이 yet | Home | `{typography.title}`: "아직 키우는 포도송이가 없어요" + [새로운 포도송이 추가] button only. No checklist section rendered. |
| Cap already hit today | 포도송이 카드 | Tapping [수행 완료] or an empty grape slot when today's grape is already earned shows "오늘 완료! 내일 만나요:D" directly, no modal (FR-4), no state change, no error color. |
| Tapping an already-filled grape | 포도송이 카드 | Shows that specific grape's earn date instead of re-running completion — "YYYY.MM.DD에 획득한 젤리예요:D" (dot-formatted date, `formatDateDots`). Grapes earned before this feature existed have no recorded date and fall back to "획득한 젤리예요:D" with no date prefix. |
| Per-tap celebration | Global toast | "어제보다 더 나은 오늘이네요! ✨" on every grape-earning tap (FR-7). |
| Daily-total surpass | Global toast | "🎉 어제를 넘었어요!" — fires once per day, only if the record toast (below) doesn't also fire this tap (FR-9). |
| All-time record | Global toast | "🏆 역대 최고 기록이에요!" — takes priority over the surpass toast when both conditions hit the same tap (FR-10). |
| 포도송이 graduates | Home | Reaching the cap no longer graduates the card by itself (revised FR-13 — see [젤리를 다 모았어요! 루틴으로 이동!] above); once the user taps that prompt, the card moves from the growing list into the checklist below via plain re-render — no transition animation, per the locked minimal-motion decision. |
| 지난 리뷰 empty | 지난 리뷰 | "아직 쓴 리뷰가 없어요." |

## Interaction Primitives

- Tap [수행 완료] (or the next empty grape slot, as a shortcut) to open the completion-confirm modal; [Yes] fills a grape — independent of the "젤리를 위한 작은 체크리스트" log (revised FR-3). Tap a log row's toggle to mark it personally done; it stays in the log either way.
- Tap a checklist row to check it off; overflow control (small `⋯` or long-press-free tap target) reveals 다시 도전하기/삭제하기 (FR-16).
- No swipe gestures, no long-press, no drag-reorder — everything is a direct tap, keeping the one-handed/low-effort posture from the PRD's Design Principles. **Exception (explicit user request):** 포도송이 순서 편집 mode uses drag-to-reorder — see the [편집] row above.
- **Motion (locked, minimal):** every tappable element (grape slot, button, checklist row) gets a brief `transform: scale(0.95)` press feedback on active/tap, mirroring `DESIGN-apple.md`'s system-wide micro-interaction. Celebration toasts (FR-7/9/10) fade in/out only — no slide, bounce, or pulse. Graduation (FR-13) is a plain re-render, no transition animation.
- **Banned:** progress percentages/bars anywhere, streak counters, push notifications, confetti/particle effects, bounce/pulse/spring animations (text-only toasts per `DESIGN.md`).

## Accessibility Floor

Behavioral; visual contrast lives in `DESIGN.md`.

- Tap targets ≥ 44×44px on every grape slot, button, and checklist row (per PRD §5 NFR).
- Screen reader: each grape slot announces role + state ("젤리, 7번째, 완료" / "젤리, 8번째, 비어있음"). Celebration toasts announce as a polite live-region, not assertive (don't interrupt).
- Reduce Motion: skip the scale-press feedback and toast fade transitions; state changes render instantly.
- Focus order follows the 포도송이 card list top-to-bottom, then checklist, then the 오늘 완료하기 button.

## Inspiration & Anti-patterns

- **Lifted from Apple's restraint principle:** one accent, one reserved shadow, text-only state — applied to a completely different product (see `DESIGN.md` Brand & Style).
- **Rejected — progress percentages / streak flames (most habit apps):** directly violates PRD Design Principle 1 and 5; no % anywhere, no punishing-absence visual language.
- **Rejected — confetti/particle celebration effects:** the celebration toast stays text-only and calm, consistent with the "no gamification noise" posture — the grape itself already carries the visual payoff.

## Key Flows

### Flow 1 — 지수, 첫 젤리를 심다 (mirrors PRD UJ-1)

1. 지수가 앱을 처음 연다 → 빈 홈 화면, [새로운 포도송이 추가]만 보임.
2. "아침 시동" 포도송이를 만들고 "두 발 바닥에 대기"를 첫 걸음으로 입력.
3. 다음 빈 그레이프를 탭 → 완료, 토스트 "어제보다 더 나은 오늘이네요! ✨"
4. 현재 걸음 칸이 조용히 비워짐. 지수가 내킬 때 "물 한 잔 마시기" 입력.
5. [새로운 포도송이 추가]로 "달리기 1.5km" 포도송이 시작, 완료 시 같은 토스트.
6. **Climax:** 오늘 여러 포도송이에서 알을 채우다 오늘 총합이 어제를 넘는 순간 "🎉 어제를 넘었어요!" 토스트가 별도로 뜬다 — 숫자를 세지 않아도 "오늘이 더 나은 하루"라는 걸 몸으로 느낀다.
7. [오늘 완료하기] → 오늘의 리뷰 4칸 작성(비워둬도 됨) → 저장, 하루 종료.

### Flow 2 — 달리기 포도송이의 졸업 (mirrors PRD UJ-2)

1. 다음날 앱을 연다.
2. "달리기 1.5km"가 20/20에 도달했다. [젤리를 다 모았어요! 루틴으로 이동!]을 탭하자 루틴 탭의 루틴 체크리스트로 옮겨져 있다. 아직 체크 전이라 빈 체크박스 + 흰 배경.
3. 지수가 체크 → 그레이프도, 토스트도 없이 담담하게 완료 표시. 체크박스가 채워지고 행 배경이 `{grape-tint}`로 바뀐다.
4. **Climax:** 더 이상 "달리기, 다음 하나는?"을 고민할 필요가 없다는 것 자체가 보상 — 나머지 포도송이는 여전히 성장 중인 카드로 남아있다.
5. [오늘 완료하기] → 리뷰 작성 → 종료.
