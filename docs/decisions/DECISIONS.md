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

## ADR-005 — Version scope V1.1
- Date: 2026-11-17
- Status: Accepted for planning
- Decision: V0.1 Island, clipboard text/image history, region screenshot, Desk closed loop and foundational privacy/accessibility. OCR/basic annotation optional, **user-facing Flow presets deferred to V0.2**, configurable Flow to V0.3. Archive/conversion V0.2; Dock preview conditional on PoC.
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

## ADR-011 — Documentation precedence and conflict escalation
- Date: 2026-11-17
- Status: Accepted for V1.1 baseline
- Decision: Platform safety/privacy > accepted ADRs > FEATURE_MATRIX (version/tier) and PRD (behavior) > INTERACTION_SPEC/WEAVE_ISLAND_SPEC > DESIGN_SYSTEM > ARCHITECTURE implementation details. AGENTS governs AI behavior; AI_DEVELOPMENT_GUIDE governs procedure.
- Consequence: Equal-authority conflicts require maintainer decision; agents may not silently resolve them.

## ADR-012 — Canonical Island motion and task decoupling
- Date: 2026-11-17
- Status: Accepted as design baseline, timings provisional pending hardware validation
- Decision: DESIGN_SYSTEM defines hoverWake 150ms, compactExpand 220ms, fullExpand 280ms, collapse 180ms and completionHold 600ms. Task execution state remains independent of Island presentation state.
- Consequence: Esc closes panel without cancelling tasks; drag suppresses timeout; task completion cannot steal focus.

## ADR-013 — Clipboard provenance and data ownership
- Date: 2026-11-17
- Status: Accepted as privacy principle; default retention unresolved under ADR-009
- Decision: Source application attribution and password detection are best effort; do not promise absolute app exclusion. Offer reliable pause/manual capture fallback. Clear All removes owned metadata/cache but never deletes externally referenced originals.
- Validation: POC-CLIP plus deletion and privacy tests.

## V1.1 outstanding release blockers
ADR-007 minimum macOS, ADR-008 distribution/sandbox, ADR-009 retention defaults, and PoC-backed notch fallback decision must be closed before final release configuration. Their unresolved status does **not** prevent isolated PoC implementation.
