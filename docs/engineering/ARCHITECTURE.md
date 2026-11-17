# WeavePivot — Technical Architecture V1.1
> 2026-11-17 | Status: proposed architecture, not yet implemented

## 1. Architectural goals
Native macOS utility built with SwiftUI + AppKit, offline-first and privacy-first. Prioritize idle efficiency, interruptible interaction, recoverable errors, testability, and safe file operations. This is a design baseline, not evidence that a macOS build or PoC has passed.

## 2. Constraints and open decisions
- macOS minimum deployment target: **TBD after PoC**; do not silently pick the latest SDK as the minimum.
- Distribution: Developer ID notarization vs Mac App Store sandbox **TBD**. Validate permissions, background behavior and accessibility constraints for both.
- Build with Xcode and Swift Package Manager. Pin third-party dependencies; prefer Apple frameworks.
- No private API, injection into Dock, system menu-bar modification, or assumptions about the physical notch. User-facing Flow presets are V0.2; V0.1 TaskExecuting is an internal service only.

## 3. Layer diagram

```text
WeavePivotApp (lifecycle, menu bar, settings, launch)
  ├─ UI / Island (NSPanel host + SwiftUI)
  ├─ UI / Desk (SwiftUI window)
  ├─ UI / Clipboard (SwiftUI panel)
  ├─ UI / Capture (AppKit coordinator + SwiftUI editor)
  └─ UI / Flow (V0.2+ user-facing presets; V0.1 internal Task Engine only)
             ↓
  Application / Commands / UseCases / StateMachines
             ↓
  Domain / Asset / Task / Permissions / Settings
             ↓
  Infrastructure / Pasteboard / Capture / OCR / Storage / File Access
```

UI must not directly own pasteboard polling, file I/O or permission prompts. Application services expose async operations; UI receives observable snapshots. State machines should be deterministic and testable without windows.

## 4. Suggested repository structure

```text
WeavePivot/
  App/
  Core/Domain/
  Core/Application/
  Core/Infrastructure/
  Features/Island/
  Features/Desk/
  Features/Clipboard/
  Features/Capture/
  Features/Flow/
  DesignSystem/
  Resources/
Tests/Unit/
Tests/Integration/
Tests/UI/
poc/notch-window/
poc/clipboard/
poc/screen-capture/
poc/dock-preview/
docs/
```

Use local Swift packages only when a real compilation or ownership boundary exists; do not fragment the MVP into dozens of packages.

## 5. Core domain contracts
- Asset: UUID, kind (text/image/file/link), createdAt, optional source metadata, preview state, retention policy, bookmark or local blob reference.
- Task: UUID, kind, progress (indeterminate or 0...1), lifecycle (queued/running/succeeded/failed/cancelled), error summary, cancellation capability. A Task's lifecycle is independent from Island presentation state.
- PermissionState: unknown/notDetermined/granted/denied/restricted where meaningful; represent platform-specific cases explicitly.
- IslandState: dormant/awake/compact/expanded/processing/result/error; task lifecycle is not forced to equal UI lifecycle.
- UserSettings: capture enabled, exclusions, history limits, hotkeys, island display selection, motion preferences.

Proposed interfaces:
```swift
protocol AssetRepository {
    func insert(_ asset: Asset) async throws
    func search(_ query: AssetQuery) async throws -> [Asset]
    func remove(id: UUID) async throws
}
protocol ClipboardObserving {
    func start() async
    func stop() async
}
protocol TaskExecuting {
    func execute(_ request: TaskRequest) async -> TaskResult
    func cancel(id: UUID) async
}
```
These are sketches; actual Swift types must be designed and compiled during implementation.

## 6. Event and concurrency strategy
Swift concurrency with actor-isolated mutable services; @MainActor for view models and AppKit windows. Prefer typed events/observable state rather than a global untyped event bus. Throttle clipboard changeCount checks and debounce hover; never use an unconstrained high-frequency timer. Structured cancellation for OCR, thumbnailing and file operations. No blocking work on main thread.

## 7. Storage and security
Local metadata store (SwiftData/Core Data/SQLite decision after prototype) plus managed binary cache; do not put large images in UserDefaults. Store external file access using security-scoped bookmarks when required. Validate UTTypes, file size, symlink handling, path traversal and decompression limits. Retention, delete and clear-all must remove both index entries and owned cache data. Logs must exclude clipboard text, screenshots, OCR text and sensitive file paths.

## 8. Permissions
Capture: appropriate ScreenCaptureKit or system API and Screen Recording consent as required. Clipboard: NSPasteboard with behavior reviewed against target macOS privacy changes. OCR: Vision on-device. Dock preview: separate feasibility study for accessibility/window enumeration and screenshots. Ask permission at point of use, never at first launch en masse. A permission failure must not crash the app.

## 9. Island window subsystem
IslandWindowCoordinator handles NSScreen changes, coordinate conversions, panel lifecycle and hit testing. IslandStateMachine owns transitions; IslandView renders. Use safe-area and display geometry, not hardcoded notch pixels. Avoid obscuring menu extras. Fall back to standalone capsule on incompatible displays or when safe positioning fails. Test full screen, Spaces, auto-hide menu bar and external monitors.

## 10. Error handling
Typed errors with user-safe messages, recovery actions and diagnostics. File import must not destroy originals; export uses atomic write where possible. Background task state persists long enough for the user to inspect failures. Recovery after crash must reconcile incomplete tasks and orphaned cache items.

## 11. Testing and performance
Unit: state transitions, data policies, task cancellation, retention. Integration: pasteboard formats, screenshot permissions, bookmarks, cache cleanup. UI: keyboard, VoiceOver, light/dark, reduced motion, multi-screen. Profile idle CPU, memory, wakeups, GPU and capture latency on reference hardware; set thresholds only after measurement. CI can run formatting/static checks on Linux if possible, but real AppKit/SwiftUI builds require macOS runners.

## 12. Decisions required before scaffolding
Confirm minimum OS, distribution path, clipboard retention defaults, app bundle ID, signing team, whether OCR is V0.1 optional, and whether menu bar-only launch is preferred. Record decisions in docs/decisions/DECISIONS.md.

## 13. V1.1 domain schema and ownership boundaries
Proposed `Asset` fields: id UUID; kind enum; createdAt; updatedAt; displayName; optional sourceApplication (only when reliably known); sourceKind; payloadLocation (inline text / managed blob / external bookmark); byteCount; contentType UTType identifier; previewStatus; retentionClass; optional expiresAt; contentFingerprint for deduplication; isPinned; errorState. Sensitive payloads must not be printed in logs or telemetry. Distinguish **owned** cached bytes from **external** referenced files; deletion never removes external sources.

`Task`: id, operation, inputAssetIDs, state, progress, startedAt, completedAt, cancellable, userFacingError, resultAssetIDs. Store task progress separately from Island's visual state; closing the Island does not cancel a task. Task cancellation must be cooperative, and a completed task must not be resurrected by stale callbacks.

`Settings`: historyEnabled, retentionLimitCount, retentionLimitDays, excludedApplications (best effort), preferredDisplayPolicy, globalShortcut, reduceMotionOverride (normally follow system), telemetryConsent (default off if ever added). The defaults for persistence and retention remain unresolved until ADR-009; no implementation may invent a permanent policy.

## 14. Concrete data-flow boundaries
1. NSPasteboard observer → type validation → privacy gate → deduplication → AssetRepository → UI snapshot.
2. User capture request → permission coordinator → screen selection → capture result → managed blob → AssetRepository → Desk/clipboard export.
3. Desk drag-in → UTType/file bookmark validation → asset reference or copy → preview generation on background executor.
4. Clear history → transactionally remove metadata → owned blob and thumbnail cleanup → verify no orphaned caches; never delete user originals.
5. Flow V0.2 uses internal TaskExecuting; no user-configurable graph in V0.1.

## 15. Storage and API decisions pending PoC
Compare SwiftData/Core Data/SQLite against migration, full-text search, binary cache lifecycle, testability and target macOS. Choose one through ADR after a minimal benchmark. Screenshot implementation must be validated against available public APIs and actual permission prompts on selected macOS versions. Clipboard app-origin exclusion is **best effort**, not guaranteed; fallback to pause/manual capture when source attribution is unavailable.

## 16. Architecture release gates
No feature view performs synchronous disk I/O, screen capture or pasteboard polling. Services must expose cancellation, typed errors, authorization status and ownership semantics. Every platform-specific adapter needs a fake implementation for unit tests. Freeze deployment target and distribution before release build configuration, not necessarily before isolated PoCs.
