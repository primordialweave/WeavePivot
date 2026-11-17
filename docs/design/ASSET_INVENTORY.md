# WeavePivot — Prototype and Asset Inventory V1.2

Status: **planned inventory**. None of the items below is asserted to exist or be approved. Source of truth for export standards: ASSET_GUIDELINES.md; for tokens: DESIGN_SYSTEM.md.

## Prototype checklist
| ID | Module | Required screens/states | Version | Gate |
|---|---|---|---|---|
| PROTO-ISL-01 | Island | dormant, awake, compact, expanded, drag-target, processing, result, error | V0.1 | M2 |
| PROTO-ISL-02 | Island | notched/notchless, fullscreen, external-display safe placement | V0.1 | M1→M2 |
| PROTO-CLIP-01 | Clipboard | history, text/image preview, search, paused, clear confirmation | V0.1 | M3 |
| PROTO-CAP-01 | Capture | region selector, preview, copy/save/add-to-Desk, denied permission, cancelled | V0.1 | M4 |
| PROTO-DESK-01 | Desk | empty, text/image grid, selected, drag target, export, missing-source | V0.1 | M4 |
| PROTO-SET-01 | Settings | shortcuts, permissions, retention, appearance | V0.1 | M3–M5 |
| PROTO-CAP-02 | Capture | annotation/OCR | Optional V0.1 | only if shipped |
| PROTO-FLOW-01 | Flow | presets, progress, success/failure | V0.2 | M6 |
| PROTO-ARCH-01 | Archive | ZIP preview, safe extraction, error | V0.2 | M6 |
| PROTO-DOCK-01 | Dock | window preview, unsupported fallback | Conditional V0.2 | PoC gate |
| PROTO-FLOW-02 | Flow | configurable workflow editor | V0.3 | M7 |

## Independent assets
| ID | Source asset | Technique | Runtime target | Priority |
|---|---|---|---|---|
| AST-BRAND-01 | logo master / wordmark / symbol | editable vector; manual typography verification | brand and About | brand approval |
| AST-ICON-01 | app icon master and optical variants | approved vector + exported AppIcon | Assets.xcassets | M5 |
| AST-MENU-01 | monochrome menu bar glyph | vector template or SF Symbol | menu bar | M0/M2 |
| AST-JADE-01 | restrained jade grain | optional seamless transparent raster | empty/onboarding only | optional |
| AST-WEAVE-01 | woven motif | optional vector pattern | non-interactive decoration | optional |
| AST-EMPTY-01 | empty-state illustration | optional vector/raster with alt copy | Desk | M4 |
| AST-MOTION-01 | Island transitions | **code**, not pre-rendered animation | Island coordinator | M2 |
| AST-CTRL-01 | buttons/cards/search/progress | **native SwiftUI components** | DesignSystem | M2 |

## Required metadata per prototype
ID, feature, state, theme, version, viewport in points, backing scale, notch/safe-area geometry, dimensions and spacing annotations, interaction and focus order, keyboard and VoiceOver behavior, animation tokens, requirement ID, approval status, reviewer, source file, export file, provenance and license.

## Production workflow
1. Verify that a prototype is needed at the current milestone; no V0.3 work in M1.
2. Create editable source and independent, clean assets; do not bake controls/text into a flattened image.
3. Annotate geometry and states, then review against Design System and real macOS constraints.
4. Export approved versions only; add record to `design/assets/manifest.csv`.
5. Build native SwiftUI/AppKit components; compare **running app screenshots** to approved reference.
6. Record visual deviations and accessibility results; do not declare fidelity from a generated image.

## AI asset generation boundary
Image generation may be used for **independent illustrative/background assets** when explicitly requested. Icons, text, interactions, focus, controls, and notch behavior must be verified or implemented natively. Do not automatically generate images merely because an inventory item is listed.

## Gate
PoC-NOTCH needs **no final logo or decorative artwork**. It needs only measurable UI states and simple native placeholders. M2 needs approved tokens and annotated Island states. M5 needs approved app/menu icon and licensing.
