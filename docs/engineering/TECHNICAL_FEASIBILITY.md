# WeavePivot — Technical Feasibility and PoC Plan V1.0
> 2026-11-17 | Status: **not yet tested on hardware**

## 1. Purpose
Validate platform-sensitive behavior before committing to production implementation. A conceptual screenshot does not prove API support. Record exact macOS version, hardware model, display topology, app sandbox mode and signing configuration for each experiment.

## 2. Risk register
| ID | Area | Risk | Severity | PoC | Fallback |
|---|---|---|---|---|---|
| R1 | Notch overlay | Incorrect placement or menu bar occlusion | High | POC-NOTCH | detached top capsule |
| R2 | Spaces/fullscreen | panel disappears or steals focus | High | POC-NOTCH | menu bar/shortcut window |
| R3 | Clipboard | polling energy cost, privacy behavior | High | POC-CLIP | lower frequency/manual capture |
| R4 | Capture | permission, Retina coordinates, multi-display | High | POC-CAP | native system screenshot handoff |
| R5 | Dock previews | unsupported window metadata or consent | High | POC-DOCK | app switcher without thumbnails |
| R6 | Sandbox | bookmarks/drag-export limitations | Medium | POC-FILES | explicit import/export |
| R7 | Distribution | Developer ID vs MAS restrictions | High | POC-DIST | supported channel split |

## 3. POC-NOTCH
Build minimal AppKit NSPanel with SwiftUI content, no feature services. Exercise dormant/hover/compact/expanded states. Detect screen frames and safe insets; use screen coordinate conversion and backingScaleFactor. Compare notch MacBook and notch-free monitor. Test external monitor hot-plug, Spaces, full-screen, auto-hide menu bar, menu extras, Stage Manager, sleep/wake, keyboard focus and mouse tracking. Never use private notch geometry APIs.

**Pass:** can display and dismiss without hiding critical menu bar UI, focus theft, hit-test gaps or crashes on target configurations. **Conditional:** fallback capsule required on some configurations. **Fail:** cannot reliably place or interact; ship detached capsule only.

## 4. POC-CLIP
Observe NSPasteboard changeCount with adaptive polling. Test text, RTF, HTML, images, URLs and file URLs; repeated copies; rapid changes; large payloads; locked/sensitive source applications; application exclusion; pause and clear. Inspect current macOS pasteboard privacy prompts/indicators, without bypassing them.

**Pass:** reliable supported types, bounded idle cost, exclusions work, no silent data loss. **Fallback:** manual clipboard capture and explicit opt-in.

## 5. POC-CAP
Prototype region selection and capture using supported public APIs (ScreenCaptureKit where appropriate); verify consent flow, denied/revoked permissions, multi-display/Retina coordinate mapping, window/full-screen variants if supported, color fidelity and clipboard/Desk export. OCR with Vision is a separate subtest.

**Pass:** user-initiated capture succeeds with correct coordinates; denied consent gives actionable recovery. **Fallback:** import screenshots created by macOS screenshot utility.

## 6. POC-DOCK
Determine feasible app/window enumeration and thumbnails through supported Accessibility and ScreenCaptureKit/CGWindow APIs, respecting TCC permissions and protected content. Do not inject into, replace or modify system Dock. Test multiple windows per app, minimized windows, full-screen, closed windows and permission withdrawal.

**Pass:** accurate supported window list and thumbnails with graceful unsupported cases. **Fallback:** simple app/window switcher or defer V0.2 feature. Do not promise system Dock hover interception.

## 7. POC-FILES / POC-DIST
Test security-scoped bookmarks, file drag-in/out, file URL persistence, ZIP extraction safety, app sandbox, background/menu bar lifecycle, login item policies, signing and notarization. Test separate distribution builds if needed. No implicit persistent file access outside granted scope.

## 8. Measurement protocol
For each PoC: document device, OS, build config, steps, expected/actual behavior, screen recording or logs without sensitive content, idle CPU/memory/energy, permissions, defects, decision (pass/conditional/fail), owner and date. Performance targets are assigned after baseline, not invented.

## 9. Exit criteria
- R1–R4 have documented pass or approved fallback.
- R5 can remain unproven for V0.1 but must not be marketed as shipped.
- No private API or permission circumvention.
- Each risk has a named owner and a dated decision.
- Findings update PRD, Feature Matrix, Island Spec and Architecture.

## 10. Open questions
Exact target hardware and macOS baseline, sandbox strategy, distribution, user consent/retention policy and whether screenshots must work in full-screen apps. Answer before production code freeze.
