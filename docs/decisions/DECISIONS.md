# WeavePivot — Decision Log / Architecture Decision Records

> Created: 2026-11-17. This document separates **accepted product direction** from **unverified implementation assumptions**.

## Status vocabulary
Proposed = needs explicit decision or PoC; Accepted = approved direction; Superseded = replaced by newer ADR; Rejected = intentionally not chosen. Every ADR records date, context, decision, alternatives, consequences and validation requirements.

## ADR-001 — Native macOS architecture
- Date: 2026-11-17
- Status: Accepted as product direction; implementation not yet validated
- Decision: SwiftUI + AppKit using public macOS APIs; avoid Electron for core UI.
- Rationale: Native window integration, performance, accessibility and system interaction.
- Consequences: macOS-only build/test infrastructure and AppKit interop.

## ADR-002 — Unified interaction model
- Date: 2026-11-17
- Status: Accepted
- Decision: Island (entry) → Desk (material collection) → Flow (processing).
- Consequences: shared Asset/Task contracts; avoid isolated feature silos.

## ADR-003 — Jade visual identity
- Date: 2026-11-17
- Status: Accepted as direction
- Decision: warm celadon-jade, restrained Chinese-inspired details, native macOS material.
- Caveat: exact token values and accessibility contrast are pending testing.
- Consequences: semantic tokens and light/dark variants required.

## ADR-004 — Notch integration and fallback
- Date: 2026-11-17
- Status: Accepted UX intent; technical feasibility proposed
- Decision: default visually merges with black physical notch, no detached pill on compatible notched Macs. Use top capsule on non-notch/incompatible configurations.
- Constraints: no private API, no physical-notch modification, no menu bar occlusion.
- Validation: POC-NOTCH on real hardware.

## ADR-005 — V0.1 scope
- Date: 2026-11-17
- Status: Accepted for planning
- Decision: Island, clipboard history, screenshot, Desk closed loop. OCR/preset actions optional enhancement. Archive/Dock previews deferred to V0.2; configurable Flow to V0.3.
- Rationale: deliver a coherent vertical slice before breadth.

## ADR-006 — Local-first privacy
- Date: 2026-11-17
- Status: Accepted
- Decision: no account or cloud required for core features; sensitive content not sent externally by default; clear pause, exclusion and deletion controls.
- Caveat: best-effort exclusion and system privacy behavior must be verified.

## ADR-007 — Minimum macOS deployment target
- Date: 2026-11-17
- Status: Proposed / unresolved
- Alternatives: older compatibility target vs newest OS for APIs.
- Required evidence: feature availability matrix and test devices.
- Decision owner: project maintainer.
- Next action: freeze after POC-NOTCH/CAP.

## ADR-008 — Distribution and sandbox
- Date: 2026-11-17
- Status: Proposed / unresolved
- Alternatives: Developer ID notarized distribution, Mac App Store, dual channels.
- Required evidence: entitlements, TCC, login items, file bookmarks and Dock preview restrictions.
- Next action: run POC-DIST and choose initial channel.

## ADR-009 — Clipboard retention and storage
- Date: 2026-11-17
- Status: Proposed / unresolved
- Alternatives: memory-only, local persisted history, configurable default.
- Required evidence: privacy UX, storage costs, restore behavior, sensitive app exclusions.
- Next action: POC-CLIP and privacy review.

## ADR-010 — Brand assets and license
- Date: 2026-11-17
- Status: Proposed / unresolved
- Decision pending: logo final master, app icon, software license and third-party asset licensing.
- Next action: create asset manifest and review licenses before release.

## ADR template
```md
### ADR-XXX — Title
- Date:
- Status: Proposed / Accepted / Superseded / Rejected
- Context:
- Decision:
- Alternatives considered:
- Consequences:
- Validation evidence:
- Related files/issues:
```

## Change policy
Never silently rewrite an accepted decision. Add a new ADR referencing the prior one and update affected PRD, Feature Matrix, Design System and tests. Keep implementation-specific details out of product-vision statements.
