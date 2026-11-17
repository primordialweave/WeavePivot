# AGENTS.md — WeavePivot AI Development Contract
> Applies to repository root and all subdirectories unless superseded by a more specific AGENTS.md.

## Mission
Build WeavePivot (织枢), a native, privacy-first macOS productivity hub for Primordial Weave. Core interaction model: Island (entry) → Desk (materials) → Flow (processing).

## Required reading order
1. docs/product/PRODUCT_VISION.md
2. docs/product/PRD.md
3. docs/product/FEATURE_MATRIX.md
4. docs/design/DESIGN_SYSTEM.md
5. docs/design/INTERACTION_SPEC.md
6. docs/design/WEAVE_ISLAND_SPEC.md
7. docs/engineering/ARCHITECTURE.md and docs/decisions/DECISIONS.md when they exist.

Do not assume missing documents exist; note the gap and proceed only within approved scope.

## Non-negotiable rules
- SwiftUI + AppKit native implementation. No Electron/webview replacement for core UI.
- Public macOS APIs only. Do not simulate modification of physical notch or override system Dock.
- No screenshots, clipboard content, OCR text, file paths or user documents uploaded without explicit opt-in.
- Respect permissions, sandbox/distribution constraints and user accessibility settings.
- No embedded secrets, fabricated test results, fake screenshots presented as running app, or silent destructive file operations.
- No new dependency, external service, background agent, login item or telemetry without documenting justification and privacy impact.
- Do not rewrite frozen design tokens or interaction semantics merely to make implementation easier.
- Prefer modular services and pure testable state machines over monolithic SwiftUI views.
- Keep Chinese and English strings localizable; use SF Symbols for standard controls.

## Work protocol
For each task: (1) restate goal and affected files; (2) identify permissions/API risks; (3) implement smallest vertical slice; (4) add tests; (5) run build/lint/tests when environment supports them; (6) report exact commands, results, limitations and remaining risks. If Xcode/macOS runtime unavailable, say so explicitly and do not claim a successful native build.

## Priority
V0.1: app shell, Island PoC, clipboard, capture, Desk integration. V0.2: user-facing Flow presets, archive/conversion and Dock preview only after feasibility tests. V0.3: configurable Flow. Avoid scope creep.

## UX fidelity
Concept posters are inspiration, not production geometry. Follow design tokens and annotated independent prototypes. Default notch state visually merges with system black notch; no always-on neon capsule. Non-notch fallback required. All animations interruptible and Reduce Motion-aware.

## Git workflow
Use focused commits; never force-push or rewrite history without approval. Update relevant docs and tests alongside behavior changes. PRs should include summary, screenshots when actually captured, tested environments, permissions, accessibility and known limitations. Preserve user-authored work.

## Definition of done
Code compiles on supported macOS target, tests pass where available, permissions are handled, keyboard/VoiceOver paths exist, no known data loss, behavior matches approved spec, and deviations are documented. An unverified PoC is not a shipped feature.

## Stop and ask
When API capability is uncertain, permissions cannot be justified, requirements conflict, implementation would replace system UI, or a change affects architecture/privacy/data retention, stop and request a decision rather than guessing.

## V1.1 source-of-truth and escalation
Safety, user privacy and public macOS API limits override all specifications. Accepted ADRs govern decisions; FEATURE_MATRIX governs version/tier, PRD behavior, INTERACTION_SPEC and WEAVE_ISLAND_SPEC interaction, DESIGN_SYSTEM visuals, ARCHITECTURE implementation. TEST_PLAN governs evidence. This file governs AI agent behavior; AI_DEVELOPMENT_GUIDE provides prompts and workflow.

If a conflict remains, **stop the affected task**, quote conflicting sections and request a decision. Never treat a Could/Should feature as a V0.1 release blocker. V0.1 internal task executor does not imply user-facing Flow presets. Do not claim a native build or hardware PoC without actual macOS evidence.

## Mandatory task report
Every implementation PR must list requirement IDs, changed files, public APIs, permissions, actual build/test commands and results, hardware/OS where tested, accessibility checks, data-handling effects, known limitations and rollback. Unexecuted tests are Not run.

## V1.2 mandatory execution references
Before scaffolding, read `docs/engineering/REPOSITORY_STRUCTURE.md`. Before UI/asset work, read `docs/design/ASSET_INVENTORY.md`. For milestone tasks, read `docs/engineering/EXECUTION_CHECKLIST.md`. Do not invent directories, asset approvals, tests or build results. PoC experiments remain in `poc/` until reviewed for promotion into production modules.
