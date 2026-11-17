# Weave Island — Engineering Interaction Specification V1.0
> Product: WeavePivot / 织枢. Status: proposed until notch-window PoC passes.

## 1. Goal and boundaries
A low-interruption macOS top-edge entry point, visually fused with the physical notch where present. Third-party software cannot alter physical hardware or the system menu bar itself. No private APIs, no replacing system UI, no claiming to own the notch. On notch-free screens use a compact top-center capsule.

## 2. Display geometry
Derive placement from NSScreen frame/visibleFrame, safeAreaInsets where applicable, display scale and window coordinate conversion. Do **not** hardcode physical notch dimensions, assume screen origin (0,0), or use screenshot pixels as points. Store placement per display and recompute on screen, resolution, scale, menu bar, sleep/wake, full-screen and Spaces changes. Default target display: screen containing pointer or active window (setting configurable). Avoid overlap with menu extras; fall back to separate capsule if safe placement cannot be guaranteed.

## 3. States and transitions
| State | Trigger | Appearance | Exit |
|---|---|---|---|
| dormant | idle | black integrated region; no logo, no glowing edge | hover/shortcut/task |
| awake | pointer enters hit region | subtle edge highlight, optional tiny mark | leave/activate |
| compact | click/shortcut or lightweight task | quick actions and task count | click/escape/timeout |
| expanded | explicit expand or drag target | action tiles, recent material, progress | escape/outside click |
| processing | background task | status + cancel if supported | completion/error |
| result | task finished | brief result and open/copy actions | timeout/manual dismissal |
| error | task failure | explanatory state with retry/details | retry/dismiss |

Priority: explicit user interaction > critical task feedback > passive hover. Background task updates must not steal keyboard focus. Never auto-expand while typing in another app.

## 4. Window and event behavior
Use AppKit NSPanel/NSWindow with SwiftUI content. Prototype nonactivating panel and appropriate collectionBehavior; verify click, keyboard, dragging, Mission Control, full-screen and multi-space behavior on actual macOS. Use explicit mouse tracking areas and debounce; do not poll pointer position continuously. Escape closes topmost layer; outside click closes only when no drag/task action requires persistence. Use native pasteboard and drag providers.

## 5. Interaction
- Hover: wake after brief debounce, no sound.
- Click: compact actions; second explicit gesture expands.
- Keyboard: configurable global shortcut with conflict detection; tab order predictable.
- Drag in: expanded drop target appears, validates UTTypes, shows accepted/rejected feedback.
- Drop: import as file reference or copy according to user preference, never delete source.
- Progress: user can inspect status, cancel only cancellable tasks; errors are retained until acknowledged or visible in history.
- Completion: transient success then retract; accessible announcement not repeated.

## 6. Initial geometry and timing hypotheses
Compact width ~420pt, height ~44pt; expanded width ~520pt, content height 92–280pt; actual size computed from content and safe geometry. Hover 150ms, expand 220–280ms, collapse 180ms. These values are **design hypotheses**, not confirmed hardware constraints. Avoid expanding over system menu extras; use separate overlay fallback when needed.

## 7. Compatibility and permissions
Notch/no-notch MacBook, external displays, mixed DPI, multiple menu bars, auto-hide menu bar, full-screen apps, Spaces, Stage Manager, screen recording permission denied, Accessibility permission denied, Reduce Motion and Reduce Transparency. The island itself must not require Screen Recording permission simply to display.

## 8. Failure and fallback
If notch alignment is uncertain, render floating capsule rather than cover the menu bar. If global shortcut registration fails, provide menu bar entry and remapping. If task APIs fail, show actionable explanation. If dragging is unsupported for a data type, refuse with reason. Never silently lose captured material.

## 9. PoC acceptance checklist
- Demonstrate dormant/awake/compact/expanded/result on a notch Mac and a notch-free display.
- Verify no obscured menu bar controls across resolutions.
- Test fullscreen, multiple Spaces, external display attach/detach, sleep/wake.
- Validate pointer hover boundaries, outside-click dismissal, keyboard navigation and VoiceOver.
- Measure idle CPU/GPU and animation smoothness; set quantitative budgets after baseline.
- Document macOS versions and App Store/independent distribution constraints.

## 10. Implementation outline
IslandWindowCoordinator (screen and lifecycle); IslandStateMachine (pure state transitions); IslandView (SwiftUI); IslandDragController; IslandTaskPresenter; IslandAccessibilityAdapter. Unit test state transitions independent of window rendering. Do not implement all logic inside a single View.
