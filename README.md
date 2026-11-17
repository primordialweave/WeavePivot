# WeavePivot · 织枢

Native macOS productivity hub — Island (织岛), Desk (织台), Flow (织流).

> **Status: documentation baseline V1.1; no native app build or hardware PoC has been verified.**

## Product direction
SwiftUI + AppKit, local-first, privacy-first, warm celadon-jade aesthetics. On compatible notched MacBooks, the Island should visually integrate with the notch; on unsupported or notch-free displays it uses a top capsule fallback. Only public macOS APIs.

## Roadmap
- **V0.1 Must:** menu bar/shortcut, Island, text/image clipboard history, user-initiated region capture, Desk storage and copy/drag export, privacy/accessibility baseline.
- **V0.1 optional:** OCR/basic annotations, file/link history and advanced search.
- **V0.2:** user-facing Flow presets, ZIP/image conversion, Dock window previews only after PoC.
- **V0.3:** configurable workflows.
- **V1.0:** production maturity, accessibility, performance and distribution refinement.

## Documentation
| Category | File |
|---|---|
| Product | [Vision](docs/product/PRODUCT_VISION.md) · [PRD](docs/product/PRD.md) · [Feature matrix](docs/product/FEATURE_MATRIX.md) |
| Design | [Design system](docs/design/DESIGN_SYSTEM.md) · [Island specification](docs/design/WEAVE_ISLAND_SPEC.md) · [Interaction specification](docs/design/INTERACTION_SPEC.md) · [Asset guidelines](docs/design/ASSET_GUIDELINES.md) |
| Engineering | [Architecture](docs/engineering/ARCHITECTURE.md) · [Technical feasibility](docs/engineering/TECHNICAL_FEASIBILITY.md) · [Development plan](docs/engineering/DEVELOPMENT_PLAN.md) · [Test plan](docs/engineering/TEST_PLAN.md) · [AI development guide](docs/engineering/AI_DEVELOPMENT_GUIDE.md) |
| Governance | [Decision log](docs/decisions/DECISIONS.md) · [AI agent contract](AGENTS.md) |

## Development readiness
Read AGENTS.md and the linked documents before coding. Accepted ADRs govern decisions; FEATURE_MATRIX governs version/tier, PRD behavior, interaction specs behavior states, DESIGN_SYSTEM visuals and ARCHITECTURE implementation. Do not claim PoC success without tests on real macOS hardware.

**Open decisions:** minimum macOS version (ADR-007), distribution/sandbox (ADR-008), clipboard retention defaults (ADR-009), notch fallback after PoC. These block release configuration but not isolated experiments.

## Next milestone
Create POC-NOTCH, POC-CLIP and POC-CAP as isolated native macOS experiments; record evidence in docs/testing/reports. No generated UI image is a substitute for a running build.

## V1.2 execution references
- [Canonical repository structure](docs/engineering/REPOSITORY_STRUCTURE.md)
- [Prototype and asset inventory](docs/design/ASSET_INVENTORY.md)
- [Milestone execution checklist](docs/engineering/EXECUTION_CHECKLIST.md)

These documents specify planned structure and outputs; they do not imply that source directories, assets or runnable macOS PoCs already exist.
