# WeavePivot — Development Plan V1.1
> 2026-11-17 | Status: proposed, milestones are gates not promises

## 1. Objective
Deliver a usable V0.1 vertical slice: shortcut/menu bar → Island → clipboard or screenshot → Desk → copy/drag out. Avoid implementing a large set of isolated tools.

## 2. Preconditions
Existing: product vision, PRD, feature matrix, design system, island spec, interaction spec, AGENTS. Missing before release: tested PoC reports, architecture decision record, annotated independent screen prototypes, approved brand assets, test plan, distribution decision.

## 3. Workstreams and phases

### M0 — Decisions and reproducible baseline
- Confirm target macOS, Xcode version, distribution, bundle ID and signing.
- Initialize native app shell, SwiftFormat/SwiftLint policy if chosen, tests and CI.
- Review existing docs/decisions/DECISIONS.md and resolve milestone-critical open ADRs.
**Exit:** reproducible local build on a Mac and one passing smoke test.

### M1 — Feasibility spikes (before polishing)
- POC-NOTCH, POC-CLIP, POC-CAP in parallel if resources permit.
- Record hardware and permissions, performance baseline, screenshots.
- POC-DOCK research can proceed independently but does not block V0.1.
**Exit:** R1–R4 pass or have explicit approved fallbacks.

### M2 — Design-to-code foundation
- Semantic design tokens, core controls, localizable strings.
- Island state machine and window coordinator; notch-free fallback.
- UI snapshot/reference comparison and accessibility scaffolding.
**Exit:** all Island states demonstrable without data services; keyboard and Reduce Motion paths exist.

### M3 — Clipboard + asset storage
- NSPasteboard observer, typed Asset model, local persistence, retention.
- History/search/delete/pause/exclusion, file reference and image previews.
**Exit:** reliable supported pasteboard types, privacy controls and restart behavior.

### M4 — Screenshot + Desk integration
- Capture coordinator, permission flow, preview, copy/save/add-to-Desk.
- Desk grid/list, drag/drop, selection, import/export, unavailable-file states.
**Exit:** complete screenshot → Desk → export journey on notch and non-notch hardware.

### M5 — Stabilization and V0.1 beta
- Crash/error handling, cache cleanup, permission revocation, multiple displays, Spaces/fullscreen, accessibility and localization.
- Measure CPU, memory, wakeups, animation frame timing and capture latency.
- Fix release blockers, sign/notarize for chosen distribution.
**Exit:** all V0.1 acceptance tests pass and known limitations are documented.

### M6 — V0.2 expansion (separate milestone)
- User-facing Flow presets, ZIP extraction and preview, image conversion; Dock window preview only if PoC feasible.
**Exit:** no destructive extraction vulnerabilities; permissions and fallbacks verified.

### M7 — V0.3 Flow
- User-facing preset sequences introduced in V0.2 are upgraded to configurable workflows, task history, retry and cancellation.
**Exit:** safe repeatability and deterministic failure handling.

## 4. Dependencies
M0 → M1 → M2; M1 Clipboard → M3; M1 Capture + M3 → M4; M2 + M3 + M4 → M5. M6/M7 do not block V0.1.

## 5. Task breakdown and estimated effort
Effort estimates are planning ranges, not calendar commitments:
| Milestone | Rough effort | Main uncertainty |
|---|---|---|
| M0 | 2–4 developer-days | signing/setup |
| M1 | 5–12 | macOS permissions/window behavior |
| M2 | 5–10 | notch animation/focus |
| M3 | 6–12 | pasteboard types/storage |
| M4 | 8–16 | capture/multi-display/drag |
| M5 | 6–12 | regressions/distribution |

Re-estimate after PoC; do not interpret as fixed delivery dates.

## 6. Definition of ready
Each issue includes user story, screenshot or written UI state, input/output, API constraints, error and privacy handling, accessibility, tests, acceptance and rollback plan.

## 7. Definition of done
Code reviewed; tests pass on actual macOS; no unhandled permission denial; no data loss; no privacy-sensitive logging; keyboard and VoiceOver paths; docs updated; measured performance; screenshots from running build rather than generated concept art.

## 8. Issue labels and branches
Labels: area:island/desk/clipboard/capture/core, type:feature/bug/spike/docs, priority:p0/p1/p2, risk:permission/platform/data. Use short-lived branches and PR review; no force-push. One milestone per GitHub milestone.

## 9. AI coding workflow
Read AGENTS and relevant docs; propose smallest vertical slice; identify public APIs and system constraints; implement; add tests; run commands on macOS; report actual outputs and deviations. If no Mac/Xcode runtime is available, do not claim builds or PoCs passed. No silent architecture changes.

## 10. First ten executable issues
1. Record macOS target/distribution ADR.
2. Create native app shell with menu bar entry.
3. Add design token package and accessibility previews.
4. POC-NOTCH screen geometry and window.
5. POC-NOTCH state machine and focus tests.
6. POC-CLIP types, privacy and energy baseline.
7. POC-CAP permission and multi-display capture.
8. Define Asset model and local storage prototype.
9. Integrate clipboard history into Desk.
10. Integrate screenshot into Desk and run end-to-end tests.

## 11. Release checklist
Version/changelog; license and privacy policy; permission rationale; signed/notarized build if applicable; install/uninstall/cleanup; no debug entitlements; crash reporting opt-in if used; rollback and data migration plan.

## 12. V1.1 milestone acceptance and requirements
| Milestone | Deliverables | Blocking gate |
|---|---|---|
| M0 | runnable native shell, CI, ADR-007/008 initial decision | real macOS smoke build |
| M1 | POC-NOTCH/CLIP/CAP reports | pass or approved fallback |
| M2 | Island coordinator/state machine, tokens, keyboard/VoiceOver | ISL-01/02 tests |
| M3 | clipboard store, pause/clear/exclusion best effort | CLIP-01/02 + DATA-01 |
| M4 | screenshot + Desk export | CAP-01 + DESK-01 |
| M5 | V0.1 beta, signed test build, regressions | all Must requirements |
| M6 | V0.2 presets, ZIP, conversion; Dock conditional | scoped V0.2 acceptance |
| M7 | V0.3 configurable workflows | cancellation/retry safety |

### Change-management rule
A feature moves versions only with synchronized changes to FEATURE_MATRIX, PRD, PRODUCT_VISION, TEST_PLAN, this plan and ADR-005. Internal Task Engine in V0.1 is **not** a user-visible Flow preset. Accessibility, privacy, error recovery and measured performance are required from M2–M5, not deferred to V1.0.

### Execution order clarification
M0 may include provisional deployment target for a disposable PoC; production target is frozen after M1. M2 and M3 may run in parallel only when domain interfaces and Island contracts are stable. Each issue should reference one or more requirement IDs and linked test IDs.

### Effort disclaimer
Developer-day estimates are rough planning assumptions, not commitments. Re-estimate after hardware feasibility results and account for localization, QA, signing, notarization and distribution.

## 13. V1.2 execution contract and corrected prerequisites
Use [EXECUTION_CHECKLIST.md](EXECUTION_CHECKLIST.md) for milestone inputs, outputs, evidence and gate checks, and [REPOSITORY_STRUCTURE.md](REPOSITORY_STRUCTURE.md) for file placement. `DECISIONS.md` already exists; M0 must **review and resolve relevant open ADRs**, not create that document anew. M0 establishes a provisional target for a runnable PoC; final release choices are frozen after feasibility work. Never treat a written milestone as completed without test evidence.
