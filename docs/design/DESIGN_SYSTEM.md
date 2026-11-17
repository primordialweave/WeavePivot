# WeavePivot Design System V1.0
> Status: implementation baseline / pending device validation. 2026-11-17.

## 1. Principles
Native first; warm jade, not neon green; restrained Chinese-inspired geometry; high information clarity; reversible motion; accessible by keyboard and VoiceOver. The prior composite concept posters are visual references, **not** pixel-accurate implementation specifications. Decorative landscapes never reduce content contrast.

## 2. Themes and design tokens
All colors are semantic, adapt to light/dark and increased contrast, and must not be hardcoded in feature views.

| Token | Light proposal | Purpose |
|---|---|---|
| jade.primary | #67D0B1 | selected accents |
| jade.hover | #4FB08E | hover |
| jade.deep | #2F7E6A | dark text/icons on light surfaces |
| surface.canvas | #F7FAF8 | background |
| surface.panel | #E6F2EE | subtle panel |
| border.soft | #C8D1D6 | outline |
| text.primary | #1F2D2A | body |
| text.secondary | #607B78 | muted |
| state.success | #34C759 | success |
| state.warning | #FFB400 | warning |
| state.error | #FF5F56 | error |
| state.info | #6BA7FF | information |

These are proposed values, not color-contrast-certified. Prefer system semantic colors for labels, separators, and control surfaces; verify WCAG AA contrast in both appearances. Never encode state by color alone.

## 3. Typography
Use system SF Pro for Latin and system PingFang SC for Chinese through native font APIs, with Dynamic Type-compatible scaling where applicable. Proposed roles: pageTitle 28/36 semibold; sectionTitle 20/28 semibold; cardTitle 16/24 medium; body 14/22 regular; caption 12/18 regular; compactLabel 11/16 medium. Numeric values use monospaced digits. Do not bundle proprietary system font files.

## 4. Layout
Spacing scale: 4, 8, 12, 16, 24, 32 pt. Radii: small 8, medium 12, large 16, overlay 24 pt. Icon sizes 16/20/24 pt; touch/click targets should meet platform usability guidance, ideally >= 28x28 pt for compact pointer controls and larger for primary actions. Primary Desk window suggested 1280x800 pt, minimum 1024x640 pt; sidebar 240 pt, content padding 24 pt. Window sizes are initial hypotheses subject to screen tests.

## 5. Materials
Prefer NSVisualEffectView / native SwiftUI material, using opaque fallback in Reduce Transparency. Jade tint is a restrained accent; never apply green glow to all surfaces. Black notch region must remain optically black; green outline only in transient awake states. Avoid heavy shadows, continuous shaders, animated decorative backgrounds or excessive blur in idle mode.

## 6. Components
Implement: JadeButton(primary/secondary/quiet/destructive, enabled/disabled/loading); MaterialCard(default/selected/dragTarget); SearchField(empty/focused/results/error); FilterChip; AssetTile(text/image/file/link, selected/hover); ProgressRow; StatusBadge; EmptyState; PermissionExplainer; ConfirmDialog; IslandActionTile. Every component needs light/dark, hover, focus, pressed, disabled, loading where relevant, VoiceOver labels, keyboard navigation and Reduce Motion behavior.

## 7. Iconography
Use SF Symbols by default, stroke-weight consistent with system typography. Custom vector logo only for brand mark, not generic controls. Keep iconography monochrome unless color conveys state. Export approved logos as vector PDF/SVG and 1x/2x/3x PNG where needed.

## 8. Motion
Use platform spring/ease curves with interruptible state transitions. Proposed timing: hover wake 120–160ms; compact expand 180–240ms; full expand 240–320ms; collapse 160–220ms; completion 400–650ms then dismiss based on user setting. No idle looping. Respect Reduce Motion with opacity/instant state changes. Do not animate layout during active dragging.

## 9. Content and localization
Chinese simplified primary; English supported by localization keys. No text baked into image assets. Short functional labels, no marketing copy in persistent panels. Truncation and RTL readiness reviewed even if RTL is not in MVP.

## 10. Accessibility and QA
Keyboard-only operation, visible focus, VoiceOver reading order, system text scaling, high contrast, Reduce Transparency, Reduce Motion, color blindness. Snapshot tests at light/dark, 100%/scaled display, 13-inch and external display. Record visual deviations in design decisions, not ad-hoc view constants.

## 11. Source of truth
Design token definitions in DesignSystem module, Figma or equivalent editable source when available, documented exported reference screenshots. This document overrides decorative concept art. All changes require a dated decision entry.
