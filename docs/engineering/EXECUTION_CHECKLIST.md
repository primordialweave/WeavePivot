# WeavePivot — Codex Execution Checklist V1.2

Status: planned execution contract. No milestone is reported complete by this document. Follow AGENTS.md, FEATURE_MATRIX.md, PRD.md and DECISIONS.md.

## Rules for every Codex task
Read relevant docs → inspect repository → list files to create/change → map requirement IDs and tests → implement smallest slice → execute available checks → report **implemented / compiled / automated tested / hardware verified** separately → commit focused changes. On non-macOS, mark macOS tests Not run.

## M0 — Repository and native build
Input: ARCHITECTURE, REPOSITORY_STRUCTURE, ADR-007/008, AGENTS.
Deliver: Xcode project/app shell, menu bar entry, basic smoke test, exact build commands and environment. Freeze provisional deployment target for PoC; final release target later.
Gate: builds on a real Mac and app launches/quits; otherwise M0 remains blocked. Do not fabricate bundle ID/signing choices.

## M1a — POC-NOTCH (first Codex task)
Input: WEAVE_ISLAND_SPEC, TECHNICAL_FEASIBILITY, DESIGN_SYSTEM, TEST_PLAN, ADR-004/012.
Create under `poc/notch-window/`:
- README.md with objective, exact public APIs, OS requirements, risks and run commands.
- Minimal standalone SwiftUI/AppKit project and sources (window coordinator, display geometry, state machine, test harness; exact filenames determined by runnable project).
- Tests for state transitions, stale timeout cancellation and task/UI decoupling.
- `TEST_CHECKLIST.md` for notch/notchless, hover, click, drag, Esc, focus, full-screen, Spaces, menu bar auto-hide, screen attach/detach, Reduce Motion, VoiceOver.
- `docs/testing/reports/POC-NOTCH-YYYY-MM-DD.md` with environment, actual results, screenshots only if real, limitations and pass/conditional/fail.
Gate: real Mac tests or explicit 'not verified'; choose documented fallback if notch mode cannot meet safety constraints. No production feature code promotion without review.

## M1b — POC-CLIP
Input: PRD CLIP-01/02, TECHNICAL_FEASIBILITY, ADR-009/013.
Deliver `poc/clipboard/` runnable observer, typed payload experiments, pause/clear, dedup, provenance best-effort, idle CPU/wakeups report and privacy test evidence.
Gate: no unintended collection while paused; cache cleanup; acceptable measured idle cost; explicit source-attribution limits.

## M1c — POC-CAP
Input: PRD CAP-01, TECHNICAL_FEASIBILITY, TEST_PLAN.
Deliver `poc/screen-capture/` region capture demo, permission denied/revoked flows, coordinate/scale tests, copy/save demo and real hardware report.
Gate: correct crop on tested displays, cancellation without asset, recoverable denial; OCR optional.

## M2 — Native UI foundation
Input: approved M1 findings, Island annotations, Design System.
Deliver `WeavePivot/App/`, `Features/Island/`, `DesignSystem/`, `Tests/Unit/` with stable Island state machine, display fallback, accessible controls and semantic tokens.
Gate: ISL-01/02 and accessibility tests; screenshot comparison to approved annotations; no focus theft.

## M3 — Clipboard and persistence
Input: M1b, ADR-009, domain schema.
Deliver `Core/Domain/Asset*`, `Core/Infrastructure/` adapters, `Features/Clipboard/`, retention/clear settings, unit and integration tests.
Gate: CLIP-01/02, DATA-01; no external source deletion; restart and migration behavior verified.

## M4 — Capture + Desk closed loop
Input: M1c, Desk/Capture annotations, M3 asset repository.
Deliver `Features/Capture/`, `Features/Desk/`, drag/drop adapters and tests.
Gate: CAP-01, DESK-01, clipboard or screenshot → Desk → copy/drag out end-to-end.

## M5 — V0.1 beta readiness
Deliver release evidence: tests on notch/notchless configurations, privacy deletion audit, VoiceOver/keyboard, resource baselines, localization, signed/notarized build for chosen channel, installation/uninstallation.
Gate: all Must requirements pass, no open blocker/critical issues, ADR-007/008/009 closed; no claims for untested hardware.

## M6 — V0.2 expansion
Deliver user-visible Flow presets, ZIP safety and image conversion. Dock preview only if independent public-API PoC passes. Do not imply replacement of macOS Dock.

## M7 — V0.3 workflow editor
Deliver configurable task graph, validated parameters, retries, cancellation, history, persistence and destructive-action safety.

## Milestone evidence template
- Milestone and linked requirements:
- Input docs/prototype IDs:
- Files changed:
- Actual build/test commands and outputs:
- Test IDs Pass/Fail/Blocked/Not run:
- Hardware/OS/permissions:
- Accessibility/privacy checks:
- Remaining decisions, fallback and rollback:
- Reviewer approval and next milestone authorization:

## Stop conditions
Conflicting requirements, private API dependency, unexplained permission, destructive file behavior, unverified hardware promise, user-content upload, major scope change or absent required reference: stop affected work and request decision.
