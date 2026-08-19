# Reconciliation: handoff-prompt_1.md → PRD (prd-rumae-2026-08-16)

Source documents read in full:
- `handoff-prompt_1.md` (original build brief, §0–§9)
- `.memlog.md` (discovery decision log, chronological, lines 1–33)
- `prd.md` (current PRD draft)
- `addendum.md` (UX-level detail carried forward, not part of PRD scope proper)

Method: every substantive idea in the handoff prompt is checked against the PRD/addendum, then against the memlog for an explicit decision to change it. Anything present in neither bucket is a Gap.

---

## 1. Captured

These ideas/mechanics from the handoff prompt are present in the PRD or addendum, in substance if not always in the original wording:

- **Identity/framing**: name "MANDING", motto "어제보다 나은 오늘", slogan ("의지력이 아닌 '인지 설계'로 완성하는 자동화된 일상") — PRD §0/§1 Vision reuses this language almost verbatim.
- **Single user, no login/server/account** — handoff §0, §6 → PRD §2.2 Non-Users, §5 Non-Goals, §6.1 In Scope.
- **Persona traits**: morning person who struggles to get up, wants many/high goals, wants small wins first, ultimate goal is routinizing most of life to save energy — handoff §1 → PRD §2.1 JTBD (each bullet maps 1:1).
- **Wake-up micro-action as the true first screen of the day** (not a task) — handoff §3-1 → PRD FR-1, FR-2, UJ-1 step ①. Editable wording preserved (FR-2).
- **"딱 하나만 존재" single current-step UI** — handoff §2 principle 3, §3-3 → PRD FR-4 (exactly one current-step field per 포도송이).
- **Non-scolding "too big → shrink smaller" flow** — handoff §2 principle 5, §3-3 → PRD FR-8, using the same non-scolding tone requirement.
- **Grape/cluster mechanic, 15 grapes per cluster** — handoff §3-4 → PRD Glossary "포도알"/"포도송이", FR-5, FR-9 (though scope changed — see Superseded).
- **Progress always visible, not hidden until some completion threshold** — handoff §2 principle 1 (spirit of "no hidden % gate") → PRD FR-9 ("졸업 순간까지 숨기지 않는다").
- **Celebration on action, not on hitting a distant target** — handoff §2 principle 6, §3-5 → PRD FR-10 ("어제보다 나은 오늘입니다!" on every qualifying tap).
- **Routine graduation as the app's endpoint, then auto-sequencing with zero decision cost** — handoff §3-6 → PRD FR-12, FR-13.
- **Routine mode day represented as a checklist** — handoff §3-6 "자동으로 착착 내밀어 준다" → PRD FR-13, UJ-2, memlog line 13 (UJ-2 added).
- **Single HTML file, inline CSS/JS, localStorage, no server/DB, no login** — handoff §6 → PRD §6.1 In Scope.
- **Multiple topics eventually supported / not one global step** — handoff's original design was actually single-track (see Superseded S5), but the *capability* to run several concern areas is honored, now expanded into the parallel-topic architecture.
- **Failure treated as "signal to shrink," never punished** — handoff §2 principle 5 → PRD FR-8 description, Glossary tone.
- **End-of-day reflection** — handoff §4 state 7 ("오늘 어제보다 나았나요?" 한 줄) → PRD §4.5 오늘의 리뷰 (FR-16–FR-18), expanded to 4 sections (see memlog line 24, and Gaps §3 for the principle-7 tension this expansion creates).
- **New topic seeded with just a name, no upfront breakdown** — handoff §2 principle 2 → PRD FR-3, memlog line 20.

---

## 2. Deliberately Superseded (memlog-cited)

Each entry: original handoff mechanic → what replaced it → memlog citation (line number from `.memlog.md`).

| # | Original (handoff-prompt_1.md) | Replaced by (PRD) | memlog citation |
|---|---|---|---|
| S1 | §3-4: global, uncapped lifetime "포도송이 ×N" counter across the whole app (cluster count has no ceiling, ever) | Grape clusters exist only **per topic**, purely as a 15/15 graduation mechanic; no global lifetime counter | line 18: "Global lifetime grape-cluster counter (포도송이 ×N, uncapped) removed entirely. Grape clusters now exist only per-topic, purely as a graduation mechanism" |
| S2 | §3-6, §5: routine graduation triggered by "같은 걸음이 서로 다른 3일 이상 반복" | Graduation triggered when a topic's own 15-slot cluster fills completely | line 17: "Routine graduation trigger: when a specific step/topic's own grape cluster (포도송이 = 15 grapes) fills completely, that item moves to routine mode — replaces the original vague '3+ different days' rule" |
| S3 | §3-2, §4 (ASK_NEXT state): after every completion, app asks "다음 한 걸음은?" with an [이미 완료] button to end the day | Question and button removed entirely; the current-step field just sits empty, unprompted, until the user fills it whenever ready | line 26: "RESOLVES Open Question 1: removed the '다음 하나는?' conversational question entirely... Per-track [이미 완료] button (old FR-8) is removed as a consequence" (also line 27 event) |
| S4 | §3-3: current-step card has an explicit **[완료]** button (+ [너무 커요]) | No generic [완료] button — tapping the grape-cluster board itself is the completion gesture, filling the next slot | line 19: "tapping the grape-cluster sticker board itself marks the action done and fills the next grape slot — replaces a generic [완료] button" |
| S5 | §3-3, §4: one global current-step card for the whole app (single linear STEP state) | Multiple topics ("포도송이") run in parallel, each with its own current-step loop and its own cluster | line 20: "Confirmed: multiple topic tracks run in parallel (not one global current-step card). Each topic... is its own track with its own current-step loop and its own grape cluster" |
| S6 | §3-6: app-wide binary mode — "탐색 모드" vs "루틴 모드" (the whole app is in one mode or the other) | Routine/exploration are per-item states that coexist the same day — graduated checklist items and active exploration tracks run side by side | line 16: "routine and exploration are NOT mutually exclusive app-wide modes — they are per-item states that coexist same day" |
| S7 | §3-5: ambient display of yesterday's total + a *second*, separate "🎉 어제를 넘었어요!" celebration when today's total surpasses yesterday's | Celebration simplified to fire on every exploration-mode tap; the surpass-yesterday celebration dropped | line 22: "SUPERSEDES entry 5: celebration simplified — ... fires on EVERY exploration-mode grape tap ..., and the separate 'surpassed yesterday's total' celebration is dropped" |
| S8 | addendum mockup only (not in handoff doc, but relevant): cluster header "포도송이 N개째" (ordinal, meaning ambiguous) | Replaced by a running count of active (non-graduated) topics: "N개의 포도송이 키우는 중" | line 21: "Cluster board label simplified: no per-cluster ordinal ('N개째') — replaced with a running count of currently-active (non-graduated) topic tracks" |

Note: PRD FR-10's "Out of Scope" note and §6.2, §8 Open Question 1 correctly reflect S7 (ambient yesterday-count display is intentionally left as an *open question*, not silently dropped — this is handled correctly, not a gap).

---

## 3. Gaps (no record of a deliberate decision to drop/change)

Ordered by severity — highest first.

### 3.1 [HIGH] "절대 원칙" section loses its status as a labeled, inviolable constraint set
Handoff §2 is a dedicated section titled "절대 원칙 (이걸 어기면 이 앱이 아니다)" — seven numbered rules explicitly framed as non-negotiable ("if you break this, it isn't this app"). The PRD has no equivalent section. The individual rules survive, but scattered piecemeal across Vision prose, §5 Non-Goals, and various FRs, indistinguishable in weight from ordinary requirements. A reader of the PRD alone has no signal that these particular rules are load-bearing/inviolable in a way other FRs are not, nor the explicit instruction (handoff §9) that they must not be violated "in UI, wording, or logic — anywhere." Risk: a future implementer treats a principle as negotiable/tradeable against convenience, something the original author explicitly forbade.

### 3.2 [HIGH] Emotional rationale behind Principle 1 (no % of final goal) is missing
Handoff §1 gives the *reason* the no-percentage rule exists: "이 사람은 큰 산을 보여주면 '내가 겨우 이거…' 하고 좌절한다" (seeing the big mountain triggers "is this all I've done…" despair). The PRD's Non-Goals (§5) keeps the rule ("퍼센트/진도율로 보여주는 어떤 화면도 만들지 않는다") but drops the *why*. Without the emotional rationale, a builder has no way to judge edge cases the rule doesn't literally cover (e.g., a "3 more clusters until milestone X" framing, or a numeric streak framed against a target) — cases that would violate the spirit even though they aren't literally a percentage.

### 3.3 [HIGH] PWA / installability requirement is absent from the PRD
Handoff §6 requires the app to be a PWA: manifest + meta tags so "홈 화면에 추가" launches full-screen without an address bar, explicitly tied to the persona's morning-trigger use case ("아침에 폰 들자마자 아이콘 하나 눌러 '다음 한 개만' 화면 진입"). Handoff §8 lists this under **필수** (required), and §9's final build instruction repeats it. Neither the PRD nor the addendum mentions PWA, manifest, or home-screen installability anywhere. This is not logged in the memlog as a deliberate cut. Given it's a concrete, testable technical requirement tied directly to the core "grab the phone first thing in the morning" JTBD, its disappearance looks like an oversight, not a decision.

### 3.4 [MEDIUM-HIGH] Undefined relationship between the wake-up micro-action and the 포도송이/포도알 system
Handoff §3-1/3-4 treats the wake-up action as the day's very first "걸음," and completing it earns the first 포도알 of the day within the single unified count. The PRD's parallel-topic architecture (§4.2) makes 포도알 exist only inside an explicitly created, named 포도송이 (FR-3), and §4.1 (기상 미세동작, FR-1/FR-2) never states which 포도송이 — if any — the wake-up action belongs to. Yet UJ-1 step ② narrates "포도알을 탭해 완료" for the wake-up card as if it feeds a cluster. This is a real architectural ambiguity (does a default/implicit 포도송이 exist for the wake-up ritual? Does it ever earn a grape at all, or is it outside the grape system entirely, contradicting the UJ text?) that is not flagged in PRD §8 Open Questions or §9 Assumptions Index, where it belongs.

### 3.5 [MEDIUM] Absolute Principle 7 ("기록은 감정이 아니라 행동") is in tension with the new 감사일기/확언 review sections
Handoff §2 principle 7: "감정('열심히 하겠다')이 아니라 행동('10분 걷기')을 기록한다." The memlog (line 24) adds 감사일기 (gratitude journal) and 확언 (affirmation) — explicitly emotional/aspirational content — to the daily review (PRD FR-17). This addition is *logged* as a decision, so it's not silently missing in the audit sense, but the memlog never acknowledges that it sits in tension with Principle 7, and the PRD (having dropped the "절대 원칙" section per 3.1) doesn't restate the principle anywhere that would prompt a reconciliation. A reader who re-reads handoff §2 after reading only the PRD would reasonably ask why the review screen invites emotional/intention content the original doc explicitly said not to record.

### 3.6 [LOW-MEDIUM] Verb-form action phrasing guidance is not carried into any FR
Handoff §7's feature-mapping table (row 5, "동사형 목표") states the step text should be nudged toward verb-form phrasing via input hints ("톤으로 반영"), directly operationalizing Principle 7 (record actions, not feelings). No FR in the PRD specifies an input placeholder/hint encouraging verb-form step text (FR-3 only covers the 포도송이 *topic name* input, not the 현재 걸음 text field's tone).

### 3.7 [LOW] Mobile portrait-first / large tap-target / one-handed-operation NFR not restated
Handoff §6: "모바일 세로 화면 우선(폰 비율). 큰 탭 타깃, 한 손 조작." Neither PRD nor addendum restates this. It plausibly belongs in a downstream UX spec (the same bucket addendum.md is explicitly reserved for), but unlike the grape-board visual mockup, it isn't captured anywhere yet, so there's a risk it's simply forgotten rather than deferred.

### 3.8 [LOW] Poetic/vocabulary framing not preserved (cosmetic, likely fine)
Handoff §0's central metaphor — "에너지를 많이 쓰는 탐색용 발자국을, 에너지를 안 쓰는 자동 루틴으로 승격시키는 기계" (a machine that promotes costly exploratory footprints into free automatic routine) — and §3-3's "계획은 미리 그린 지도가 아니라, 걸으면서 뒤에 남는 발자국이다" are not echoed in the PRD's Vision, which uses 포도송이/졸업 vocabulary instead (consistent with memlog line 31's deliberate terminology simplification from "트랙" to "포도송이"). Meaning is preserved; the specific "footprint" imagery is not. Flagged only for completeness — lowest severity, arguably a non-issue given line 31's intentional terminology consolidation.

---

## Summary table

| Bucket | Count |
|---|---|
| Captured | 14 major items |
| Deliberately Superseded (memlog-cited) | 8 |
| Gaps — High | 3 |
| Gaps — Medium | 2 |
| Gaps — Low | 3 |
