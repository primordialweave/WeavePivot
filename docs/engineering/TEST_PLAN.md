# WeavePivot — Test Plan V1.0

> Date: 2026-11-17 | Status: test specification, **not executed** | Applies to V0.1 unless otherwise noted.

## 1. Quality objectives
Validate the complete journey: invoke Island → capture clipboard/screenshot → add to Desk → retrieve, copy or drag out. Protect user data, respect macOS permissions, avoid focus theft and preserve accessibility. This document defines *planned* tests; no result should be marked passed before execution on a real Mac.

## 2. Environments and coverage
Record exact macOS version, Xcode build, CPU architecture, Mac model, display scale, notch presence, number of screens, sandbox/signing mode, menu bar auto-hide, full-screen/Spaces and accessibility preferences. Minimum matrix:
- Notch-equipped MacBook, internal Retina display.
- Notch-free Mac or external display, ideally mixed DPI.
- Two macOS versions once deployment target is frozen.
- Light/dark, increased contrast, Reduce Motion, Reduce Transparency, VoiceOver.
- Screen Recording allowed, denied and revoked; file permission allowed/expired.
- App freshly installed, upgraded, restarted, relaunched after crash and removed.

## 3. Test levels
Unit tests: Island state machine, task cancellation, clipboard deduplication, retention, data models, file-path validation and error mapping. Integration: NSPasteboard, screenshot permission/capture, Vision OCR, asset store, drag/drop and security-scoped bookmarks. UI: state transitions, keyboard navigation, menu bar, multi-display, accessibility. Manual exploratory: Spaces, Mission Control, sleep/wake, Stage Manager and focus behavior.

## 4. Detailed cases
| ID | Priority | Scenario | Expected outcome |
|---|---|---|---|
| CORE-01 | P0 | Launch and quit from menu bar | No orphan windows/processes |
| CORE-02 | P0 | Global shortcut collision | Warning and configurable alternative |
| ISL-01 | P0 | Default notch state | Visually integrated; no persistent separate pill on supported notch hardware |
| ISL-02 | P0 | Hover/leave/expand/Esc | Deterministic reversible states |
| ISL-03 | P0 | Non-notch display | Accessible top capsule fallback |
| ISL-04 | P0 | External display attach/detach | Correct repositioning without crashes |
| ISL-05 | P0 | Fullscreen/Spaces/menu extras | No critical system controls obscured |
| ISL-06 | P1 | Background task completion | No focus stealing; dismissible feedback |
| CLIP-01 | P0 | Copy plain text, Unicode, multiline | Correct content and search |
| CLIP-02 | P0 | Copy image, URL, file URL | Correct typed asset or documented unsupported state |
| CLIP-03 | P0 | Pause history and exclude app | No new records from excluded source where detectable |
| CLIP-04 | P0 | Clear history and restart | Metadata and owned cached payload removed |
| CLIP-05 | P1 | Rapid copies/duplicates/large payload | Stable memory and documented deduplication |
| CAP-01 | P0 | Select region, cancel with Esc | Cancel creates no asset |
| CAP-02 | P0 | Deny/revoke Screen Recording | Actionable permission state; no crash |
| CAP-03 | P0 | Mixed DPI multi-monitor capture | Coordinates and crop correct |
| CAP-04 | P1 | OCR mixed Chinese/English | Text preview/edit and explicit copy |
| DESK-01 | P0 | Add text/image/file/link | Correct previews and metadata |
| DESK-02 | P0 | Drag material to Finder/other app | Correct type; original source preserved |
| DESK-03 | P0 | Delete temporary asset | Correct owned-cache cleanup, source not deleted |
| DESK-04 | P1 | Search, multi-select, keyboard navigation | Consistent selection and focus |
| DATA-01 | P0 | Restart during operation | No corrupted index or silent loss |
| DATA-02 | P0 | External file moved/permission expired | Missing-source recovery UI |
| A11Y-01 | P0 | Keyboard-only workflow | Every core task achievable |
| A11Y-02 | P0 | VoiceOver/Reduce Motion | Logical order and reduced transitions |
| SEC-01 | P0 | Logs and diagnostics | No clipboard text, OCR, screenshots or sensitive paths |
| SEC-02 | P0 | Unsupported drag/file types | Graceful refusal, no unsafe execution |
| DIST-01 | P0 | Signed/notarized install and uninstall | Correct entitlements and cleanup instructions |

## 5. Test data
Use synthetic multilingual text, large images, empty/invalid file URLs, revoked bookmarks, duplicate clips and controlled dummy sensitive strings. Never store real passwords, personal documents or private screenshots in fixtures, logs or CI artifacts.

## 6. Performance protocol
Use Instruments: Time Profiler, Allocations, Energy Log and relevant system diagnostics. Measure idle CPU, memory, wakeups, screenshot latency, search latency and animation frame stability. For each metric record hardware, 3+ runs, median, p95 when appropriate, sample count and regression against baseline. Numerical pass thresholds are **TBD after PoC baseline**, not invented here.

## 7. Defect classification
Blocker: data loss, privacy exposure, destructive file modification, crash in core path, unsupported system UI interference. Critical: permissions fail without recovery, core workflow unusable, inaccessible keyboard path. Major: reproducible workflow friction or high resource usage. Minor: visual inconsistency without functional impact.

## 8. Release gates
G0: requirements and supported macOS target frozen. G1: notch/clipboard/capture PoCs pass or fallback approved. G2: P0 unit/integration/UI tests executed. G3: zero open blockers or criticals, P0 cases pass on defined hardware matrix. G4: measured performance accepted, accessibility reviewed, signing/distribution tested. G5: beta feedback and rollback/migration plan documented.

## 9. Evidence and reporting
Store test reports under `docs/testing/reports/YYYY-MM-DD/` with test ID, build SHA, environment, steps, expected/actual, screenshot or sanitized log, status (pass/fail/blocked/not-run), linked issue and retest result. Never report 'passed' based only on source review.

## 10. V0.2+ extensions
ZIP extraction: path traversal, symlink, decompression bomb and overwrite safeguards. Dock preview: protected windows, permission denial, minimized/fullscreen windows, multi-display. Flow: retries, cancellation, idempotency, partial failure and history retention.
