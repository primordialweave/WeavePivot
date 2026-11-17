# WeavePivot — AI Development Guide V1.1

> Date: 2026-11-17 | Companion to root `AGENTS.md`.

## 1. Purpose
Provide an executable, reviewable workflow for coding agents (Codex, Claude Code or similar) to build the actual native macOS application without hallucinating platform capabilities or substituting generated images for interactive controls.

## 2. Mandatory reading
Before work, read root AGENTS.md, PRODUCT_VISION, PRD, FEATURE_MATRIX, DESIGN_SYSTEM, INTERACTION_SPEC, WEAVE_ISLAND_SPEC, ARCHITECTURE, TECHNICAL_FEASIBILITY, DEVELOPMENT_PLAN, TEST_PLAN and DECISIONS as relevant. If a required file is absent, report it. Do not invent the content of missing files.

## 3. Task contract
Each task description must include: objective; scope and explicit non-goals; input and output; affected files; reference prototype/spec; supported macOS version; public APIs; permissions; accessibility; failure states; acceptance tests; rollback/migration considerations. Never implement an entire product in one undifferentiated prompt.

## 4. Agent execution cycle
1. Inspect repository and existing changes; preserve user work.
2. Summarize task, dependencies, risks and intended file changes.
3. Propose a small vertical slice and acceptance criteria.
4. Implement using SwiftUI/AppKit and public APIs.
5. Add unit/integration/UI tests appropriate to the slice.
6. Run available builds/tests; record exact commands and results.
7. Compare running UI with annotated prototypes where available.
8. Report changes, failures, deviations, remaining risks and next issue.
9. Update docs/decisions for any approved deviation.
Never claim successful macOS compilation when running on a non-macOS host.

## 5. Implementation sequence
Phase A: app shell, menu bar, settings, test target and CI. Phase B: isolated notch window PoC and state machine. Phase C: clipboard observer and privacy/retention. Phase D: Asset model and Desk storage. Phase E: screenshot capture and Desk export. Phase F: accessibility, performance, packaging and beta. User-facing Flow presets start in V0.2, configurable Flow in V0.3. Do not build Dock previews or arbitrary Flow editor before V0.1 gates.

## 6. UI fidelity rules
- Use native components and semantic design tokens; do not embed complete screenshot as a background.
- Concept images are visual direction only; request independent measured references before pixel-accuracy claims.
- Keep the physical notch black in dormant state; no permanent floating pill on supported notched Macs.
- Provide notch-free and reduced-motion alternatives.
- Use SF Symbols and system fonts, not redistributed font binaries.
- No focus stealing, hidden permissions or fake interaction.

## 7. Security and privacy
Clipboard content, screenshots, OCR and file paths are sensitive. Do not send to AI APIs or analytics, place in test fixtures or logs, or include in bug reports without explicit consent. Do not bypass TCC or use private APIs. Treat file operations as non-destructive by default, validate paths and types, and make retention/deletion explicit.

## 8. Prompt templates

### A. Feasibility spike
```text
Read AGENTS.md and the relevant docs. Implement ONLY POC-NOTCH using public
AppKit/SwiftUI APIs. Provide a minimal runnable Xcode project, tests where
possible, a hardware test checklist, and explicit limitations. Do not claim
physical notch control. Verify screen geometry, full-screen, Spaces and
non-notch fallback. Report actual build commands and results.
```

### B. Feature implementation
```text
Read repository specs. Implement issue [ID] as the smallest vertical slice.
Before coding, list affected files, APIs, permissions and acceptance tests.
Use shared domain services and design tokens. Include keyboard/VoiceOver,
error states, unit tests and a precise summary of verified vs unverified.
Do not modify unrelated modules or approved design decisions.
```

### C. UI review
```text
Compare the running screen capture with the approved independent prototype.
Report spacing, typography, states, accessibility and interaction deviations.
Do not substitute generated concept art for a running-app screenshot.
Propose minimal corrections and re-test.
```

### D. Regression and release
```text
Execute TEST_PLAN cases for this release on the recorded hardware matrix.
Report pass/fail/blocked/not-run, actual build SHA and environment. Verify
permissions, data deletion, keyboard, VoiceOver, idle resources and signing.
Do not mark unexecuted cases as passed.
```

## 9. Coding conventions
Prefer small composable Swift files, protocols around OS services, actor-isolated mutable state and @MainActor UI. Avoid giant view models, global singletons, private APIs, unnecessary dependencies and unbounded polling. Make localization keys explicit and keep platform-specific logic in adapters.

## 10. Review gates
A change is accepted only with test evidence, permission handling, accessibility, non-destructive behavior, no sensitive logging and updated docs. Any unverified hardware behavior remains labeled unverified. Stop for maintainer approval on scope expansion, new dependency, cloud service, background agent, storage-policy change or destructive behavior.

## 11. GitHub workflow
One issue per small unit, focused branch, reviewable PR and clear commit history. PR template: summary; screenshots from running build; permissions; accessibility; tests and exact commands; known risks; rollback. Do not force push or rewrite accepted decisions without approval.

## 12. First action for a new agent
Read docs, inspect the repo, identify unresolved ADR-007/008/009, and propose POC-NOTCH with a real hardware test plan. Begin with engineering validation, not a new visual concept poster.

## 13. V1.1 source-of-truth and conflict protocol
1. Public API constraints, privacy and user safety are non-negotiable.
2. Accepted ADRs control explicit decisions.
3. FEATURE_MATRIX controls version and delivery tier; PRD controls feature behavior and acceptance.
4. INTERACTION_SPEC and WEAVE_ISLAND_SPEC control interaction states; DESIGN_SYSTEM controls visual tokens.
5. ARCHITECTURE controls implementation boundaries; TEST_PLAN controls execution evidence.
6. AGENTS.md is the coding-agent behavioral contract; this guide supplies procedures and prompt templates.
7. If documents disagree, stop affected work, report exact file/section conflicts, propose a change set, and obtain maintainer approval; never silently choose one.

## 14. Prompt hardening and scope checks
Before coding, output a requirements checklist with IDs and Must/Should/Could/Out, API availability assumptions, permission impacts and test IDs. Reject V0.1 tasks that implement user-visible Flow presets or Dock interception unless the roadmap is formally revised. Always distinguish prototype image, runnable UI and tested hardware behavior.

## 15. PR evidence template
Include: objective and linked Issue; changed files; requirement IDs; actual build command/output; unit/integration/manual test IDs and statuses; tested hardware/OS; accessibility; permissions; screenshots from running app if applicable; known limitations; rollback; ADR changes. Mark any unexecuted test **Not run**, not Passed.

## 16. V1.2 required project scaffolding references
Before creating folders, consult [REPOSITORY_STRUCTURE.md](REPOSITORY_STRUCTURE.md). Before implementing UI, consult [ASSET_INVENTORY.md](../design/ASSET_INVENTORY.md). Before starting a milestone, consult [EXECUTION_CHECKLIST.md](EXECUTION_CHECKLIST.md). In every task report, distinguish directories proposed from directories actually created, and assets planned from assets approved. Do not start M2 without a reviewed POC-NOTCH outcome or explicit fallback decision.
