# WeavePivot — Repository Structure Contract V1.2

Status: proposed target layout, not evidence of existing directories. Owner: engineering. Changes require ADR review.

## Canonical layout
```text
/
├── README.md
├── AGENTS.md
├── WeavePivot.xcodeproj/                 # only after native project generation
├── WeavePivot/
│   ├── App/                             # lifecycle, menu bar, settings bootstrap
│   ├── Core/
│   │   ├── Domain/                      # Asset, Task, Permission, Settings, pure types
│   │   ├── Application/                 # use cases, coordinators, state machines
│   │   └── Infrastructure/              # platform adapters and persistence
│   ├── Features/
│   │   ├── Island/
│   │   ├── Clipboard/
│   │   ├── Capture/
│   │   ├── Desk/
│   │   └── Flow/                        # V0.2 user-facing; V0.1 internal TaskEngine in Core
│   ├── DesignSystem/                    # semantic tokens and reusable SwiftUI controls
│   └── Resources/
│       ├── Assets.xcassets/
│       └── Localizable.xcstrings
├── Tests/
│   ├── Unit/
│   ├── Integration/
│   └── UI/
├── poc/
│   ├── notch-window/
│   ├── clipboard/
│   ├── screen-capture/
│   └── dock-preview/                    # V0.2 conditional
├── design/
│   ├── prototypes/{island,clipboard,capture,desk,flow,settings}/
│   ├── assets/{branding,icons,patterns,textures,backgrounds,animations}/
│   └── sources/
└── docs/
    ├── product/
    ├── design/
    ├── engineering/
    ├── decisions/
    └── testing/reports/
```

## Directory ownership and dependency rules
- `Domain`: platform-independent types, no SwiftUI/AppKit/ScreenCaptureKit and no filesystem side effects.
- `Application`: domain-driven orchestration and state machines, depends on Domain interfaces, not concrete platform adapters.
- `Infrastructure`: implements protocols using AppKit, NSPasteboard, ScreenCaptureKit, Vision and storage; never imports feature views.
- `Features`: views/view models and feature presentation; must call Application interfaces, not direct polling, blocking disk I/O or hidden permission prompts.
- `DesignSystem`: semantic colors, typography, motion and controls; no feature-specific data logic.
- `App`: composition root, dependency injection, lifecycle and menu bar only.
- `poc`: independent experiments; no direct import into shipping app. Promotion requires code review, production API redesign and tests.
- `design`: editable masters and annotated reference images. Only approved, optimized exports go into `Resources/Assets.xcassets`.
- `Tests`: mirrors module boundaries; tests must not depend on real private clipboard contents.

## Creation policy
M0: create native project and only directories needed by first working build; don't add empty placeholder packages or dozens of targets.
M1: create each `poc/<name>/` only when beginning that spike; each contains README, runnable source/project, test checklist and report link.
M2: Island, DesignSystem, App and Unit tests.
M3: Clipboard, Core Domain/Infrastructure and storage tests.
M4: Capture, Desk and integration/UI tests.
M5: distribution, migration and performance evidence.
M6: user-facing Flow presets, archive/conversion and conditional Dock.
M7: configurable Flow.

## Naming and file placement
Swift types use UpperCamelCase filenames; one primary type per file. Resource identifiers use semantic names. No duplicate `Shared`, `Utils` or `Common` folders without a specific ownership contract. Do not rename or move existing user files without reviewing imports and tests.

## PoC promotion checklist
1. Hardware evidence with exact OS/device and limitations.
2. Accepted API and permission strategy; fallback approved.
3. Unit tests for pure logic, integration tests for platform adapter.
4. No secrets, user content, screenshots or private paths in fixtures.
5. Move reviewed logic into appropriate Core/Feature module; keep PoC as historical evidence.
6. Update architecture, ADR, test matrix and release scope.
