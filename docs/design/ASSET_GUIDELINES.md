# WeavePivot — Asset Guidelines V1.1

> Date: 2026-11-17 | Status: asset-production specification; not a claim that assets already exist.

## 1. Philosophy
Create clean, reusable **source assets** first; compose interfaces in native SwiftUI/AppKit. AI-generated concept posters are mood references, not shippable UI textures or editable design sources. No screenshots of entire UI should be used as button backgrounds or as a substitute for live controls.

## 2. Directory layout
```text
design/
  prototypes/{island,desk,clipboard,capture,flow,settings}/
  assets/{branding,icons,patterns,textures,backgrounds,animations}/
  sources/              # editable source documents, when licensing permits
WeavePivot/Resources/
  Assets.xcassets/
  Localizable.xcstrings
```
The design directory is authoritative for source/export provenance; app Resources contain only optimized approved runtime assets.

## 3. Naming
Lowercase kebab-case ASCII. Examples: `brand-weavepivot-mark-primary.svg`, `icon-clipboard-history.pdf`, `pattern-jade-weave-light.svg`, `texture-jade-noise-light@2x.png`, `prototype-island-expanded-light-v1.png`. Suffix theme (`light`/`dark`), state, scale and version where relevant. Never use filenames such as `final-final2.png`.

## 4. Brand deliverables
- Logo: full horizontal, symbol-only, single-color, inverse and small-size optical variant.
- App icon: editable master plus `AppIcon.appiconset` required renditions generated from one approved master. Do not merely resize a low-resolution concept image.
- Menu bar mark: monochrome SF Symbol-like template image, legible at small sizes, no intricate fine lines.
- Favicons/social/banner assets only when actual website/release channel exists.
Keep all marks vector where possible. No redistribution of font files.

## 5. UI iconography
Use SF Symbols for common actions and native semantic color. Custom SVG/vector PDF only for distinct brand-specific glyphs. Maintain consistent stroke and optical alignment. Test 16, 20 and 24 pt; decorative color must not be the only state indicator.

## 6. Textures and decorative imagery
Use very low-contrast jade grain, woven patterns or ink-wash motifs sparingly, preferably on empty states, onboarding or marketing. Transparent PNG/WebP for raster overlays, SVG/PDF for vector. Supply light/dark variants when necessary. Avoid opaque panels baked into images; use native materials and gradients in code. Verify usage rights and record source/license.

## 7. Motion assets
Prefer SwiftUI/Core Animation for transitions, progress and interaction feedback. If raster animation is unavoidable, use optimized frame sequences with documented frame rate, loop count, duration, dimensions and reduced-motion fallback. Do not introduce continuous idle loops without a measurable UX purpose.

## 8. Prototype image specification
Each prototype must have a unique ID, module, state, theme, viewport size in points, display scale, version and approval status. Keep the visual reference separate from an annotation layer listing dimensions, focus order, actions, empty/error states and animation. A concept rendering without measurements is not implementation-ready.

### Initial prototype inventory
Island: dormant, hover, compact, expanded, drag-target, processing, result, error + notch/no-notch comparison. Desk: grid, list, multi-select, details, drop-target, empty. Clipboard: history, search, preview, privacy. Capture: selection, preview, annotation, permission denial. Flow: presets, running, failed, result. Settings: general, permissions, shortcuts, appearance, storage. These are **planned** assets.

## 9. Export and quality gates
- SVG/PDF: clean paths, correct viewBox, no embedded bitmap unless justified.
- PNG/WebP: transparent where required, correct 1x/2x/3x exports and color profile.
- Avoid hidden personal metadata, watermarks, copyrighted third-party illustrations and rasterized text.
- Review visual alignment on Retina, light/dark, Reduce Transparency and high contrast.
- Keep source and optimized exports linked in a manifest.

## 10. Asset manifest
Maintain `design/assets/manifest.csv` with ID, path, type, purpose, source, license, author/creator, date, version, approved, runtime target, export scale and notes. Unapproved assets must not be bundled into release builds.

## 11. Acceptance
An asset is complete only when the editable master, export, naming, provenance/license, intended usage and visual review are recorded. Missing assets should use native placeholder components, never AI-invented production files.

## 12. V1.1 asset acceptance and provenance
Asset inventory is a **planned deliverable**, not evidence of generated or approved imagery. Each approved source must have an editable master, license/provenance, theme, intended UI state, dimensions, exported variants and review record. For prototypes, add viewport points, scale factor, annotated safe areas, focus order, reduced-motion alternative and associated requirement IDs. Runtime UI is built from native controls, not a flattened concept image. Prefer a single source of truth for colors and motion in DESIGN_SYSTEM.

## 13. V1.2 prototype-to-implementation mapping
See [ASSET_INVENTORY.md](ASSET_INVENTORY.md) for planned prototype IDs, required UI states, independent asset list, development milestone and approval gates. Inventory entries do not mean assets already exist. Native controls and code-driven motion remain implementation responsibilities; only approved exports enter Assets.xcassets.
