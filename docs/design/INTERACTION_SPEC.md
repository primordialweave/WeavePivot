# WeavePivot Interaction Specification V1.1
> Cross-feature interaction contract. Applies to Island, Desk, Clipboard, Capture, Flow.

## 1. Global rules
Every operation is discoverable, cancellable where possible, keyboard-accessible and has visible outcome. Never change user files without explicit intent. Prefer native macOS conventions over novel gestures. No surprise focus stealing; no full-screen overlays unless explicitly invoked.

## 2. Navigation
Menu bar icon opens quick actions and settings; global shortcut opens Island compact state (or accessible fallback). Desk uses sidebar + content area. Esc dismisses transient overlays; Cmd+, opens settings; Cmd+F focuses search when applicable. Other shortcuts are configurable and must check collisions.

## 3. Selection and drag
Single click selects, Cmd-click toggles, Shift-click selects range in ordered views, double-click opens preview or default action. Dragging an asset exports appropriate UTType through NSItemProvider; drop targets highlight only valid types. File reference vs copied file must be communicated. Preserve original files unless user chooses destructive operation.

## 4. Clipboard
Observe changeCount with adaptive low-cost polling or appropriate system mechanism. Show origin/time/type when reliably available; never invent app attribution. Support pin, delete, copy again, search, pause capture, exclude apps, clear all. Sensitive content safeguards are best-effort and clearly disclosed. No automatic paste into another app without explicit user action and necessary permission.

## 5. Screenshot
V0.1 Must: user-initiated region capture. Window/full-screen capture are future enhancements only where supported. Escape cancels; screenshot goes to preview with Copy, Save, Add to Desk. OCR via Vision only after user action or opted-in workflow. Permission denial shows instructions and Retry; never repeatedly prompt without context. Respect scaled coordinates and color profiles.

## 6. Desk
Asset model includes stable ID, type, creation time, provenance when known, preview metadata, retention policy, file bookmark/reference and processing status. Views: grid/list, category filters, search, multi-select, details, batch action. Empty, loading, unavailable file, corrupted preview and no results are distinct states. Destructive actions require confirmation or Undo.

## 7. Flow
V0.1 provides internal single-operation task execution only; user-facing preset sequences (e.g. screenshot → OCR → copy) begin in V0.2. Each task shows queued/running/success/failure/cancelled where surfaced. V0.3 adds configurable chains. Task engine reports progress without blocking UI.

## 8. Feedback taxonomy
Success: brief, nonmodal toast; information: inline hint; warning: actionable nonblocking panel; failure: persistent error with Retry/Details; destructive: confirmation or Undo. Notifications only when relevant and permitted. VoiceOver announcements concise and rate-limited.

## 9. Privacy and permissions
Ask only at first use of protected feature. Explain why, what data is accessed and how to revoke. Local-first storage, clear retention controls and per-app exclusions. No telemetry containing clipboard text, images, file paths or OCR output. Permission withdrawal should degrade gracefully.

## 10. Accessibility and system settings
Support VoiceOver, full keyboard access, high contrast, Reduce Motion, Reduce Transparency, light/dark mode, localized labels, logical focus restoration. Hover-only interactions must have keyboard equivalents. Status never communicated by color alone.

## 11. Testing scenarios
Copy text/image/file, capture region, send to Desk, multi-select and drag out, cancel task, deny/revoke permissions, unplug external monitor, switch Spaces, sleep/wake, switch appearance, enable Reduce Motion, no-notch fallback, keyboard-only task completion. Verify no data loss, no duplicate destructive action, no focus trap.

## 12. Decision hierarchy
Platform safety and permissions > user control > functional clarity > accessibility > performance > visual flourish. Conflicts are documented in docs/decisions before implementation.

## 13. V1.1 event and conflict resolution
- Escape closes nested modal/preview first, then expanded Island; it does not cancel background tasks.
- Active drag suppresses auto-collapse; invalid UTTypes show refusal without importing data.
- Background task completion cannot activate a window or steal focus.
- Destructive removal acts only on owned cache and metadata; deleting a reference must not delete the external file.
- Global shortcuts must detect conflicts or provide a configurable alternative.
- Search, selection and drag operations remain keyboard-operable.
- Permission denial and revocation are distinct states, with Settings/retry actions where supported.

## 14. Screen state and flow traceability
Map each interaction to a requirement ID: CORE-01, ISL-01/02, CLIP-01/02/03, CAP-01/02, DESK-01/02, DATA-01, A11Y-01. Each implemented screen must document idle/loading/empty/error/focus/permission-denied states as relevant. A UI prototype is not approved until annotations include actions, keyboard focus and responsive constraints.
