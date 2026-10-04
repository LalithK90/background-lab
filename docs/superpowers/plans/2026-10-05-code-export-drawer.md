# Code Export Drawer Implementation Plan

> **For agentic workers:** Use inline execution of these tasks in order; preserve independent HTML/CSS pairs and validate each renderer before continuing.

**Goal:** Add a responsive live-customization drawer to all seven demos that copies a single-file HTML version of the current showcase page and restores the chosen style/settings on reload.

**Architecture:** Each HTML/CSS pair receives matching drawer UI and an inline page-specific controller. The controller applies supported settings through the page's native renderer, clones the current document, serializes export state, inlines the page stylesheet via same-origin CSSOM, and provides clipboard fallback behavior. No shared runtime or build system is introduced.

**Tech Stack:** Vanilla HTML, CSS, JavaScript, CSSOM, Clipboard API, existing tsParticles and Three.js CDN dependencies, browser-based Playwright validation.

---

## File Map

- Modify `01-particles.html` and `01-particles.css`: tsParticles controls, initial-style restoration, drawer and export.
- Modify `02-aurora.html` and `02-aurora.css`: CSS blob controls, initial-style restoration, drawer and export.
- Modify `03-gradient.html` and `03-gradient.css`: gradient controls, initial-style restoration, drawer and export.
- Modify `04-waves.html` and `04-waves.css`: SVG/Canvas controls, initial-style restoration, drawer and export.
- Modify `05-fluid.html` and `05-fluid.css`: blob/Canvas controls, initial-style restoration, drawer and export.
- Modify `06-canvas.html` and `06-canvas.css`: renderer controls, initial-style restoration, drawer and export.
- Modify `07-webgl.html` and `07-webgl.css`: shader-uniform controls, initial-style restoration, drawer and export.
- Update `README.md`: explain opening the drawer, changing live settings, and copying full HTML.

## Controller Contract

Repeat this page-local state serializer in each page; page-specific code validates settings and applies them to its renderer:

```js
function serializeExportState(style, settings) {
  const state = { schemaVersion: 1, style, settings };
  document.body.dataset.exportState = JSON.stringify(state);
  return state;
}
```

`style` is the existing `data-style` value. Only controls supported by that page/renderer are displayed and serialized. Defaults are derived from the chosen style; edits do not mutate catalog defaults. On reload, `document.body.dataset.exportState` is parsed and validated before renderer initialization, and that style/settings override the page's default selection.

## Task 1: Shared Drawer Markup and CSS Pattern

**Files:** all seven HTML/CSS pairs listed above.

- [ ] Add one `View code` trigger per page and a hidden dialog drawer with selected-style label, labeled controls, a read-only HTML field, `Copy full HTML`, close button, and status region.
- [ ] Use this structure for the page-local drawer, preserving page-specific IDs only where needed by the renderer adapter:

```html
<button type="button" class="code-open" aria-controls="codeDrawer" aria-expanded="false">View code</button>
<div class="code-backdrop" data-close-drawer hidden></div>
<aside id="codeDrawer" class="code-drawer" role="dialog" aria-modal="true" aria-labelledby="codeDrawerTitle" hidden>
  <button type="button" class="code-close" data-close-drawer aria-label="Close code drawer">Close</button>
  <h2 id="codeDrawerTitle" tabindex="-1">Customize and copy</h2>
  <p id="selectedStyleName"></p>
  <div id="parameterControls"></div>
  <label for="generatedHtml">Full HTML</label>
  <textarea id="generatedHtml" readonly spellcheck="false"></textarea>
  <button type="button" id="copyFullHtml">Copy full HTML</button>
  <p id="copyStatus" role="status" aria-live="polite"></p>
</aside>
```

- [ ] Add responsive drawer/backdrop styling to each corresponding stylesheet: right panel on wide screens; bottom sheet on narrow screens; use local variables and current dark/glass visual language.
- [ ] Implement open, close-button, backdrop, Escape, and keyboard-focus behavior in the first page before repeating its verified markup pattern.
- [ ] Check focused behavior in the browser: drawer opens, focuses its heading, closes on Escape/backdrop/button, and does not intercept background controls while closed.

## Task 2: Export Core on Particles Pilot

**Files:** `01-particles.html`, `01-particles.css`.

- [ ] Add `readInitialExportState()` before `loadStyle('classic')`; it returns `null` when no serialized state exists and otherwise validates the style against `configs` and clamps numeric settings to control bounds.
- [ ] Apply controls to a fresh structured clone of `configs[selectedStyle]`, preserving catalog defaults. Route edited colors to particle color/link color, speed to supported `move.speed`, amount to supported particle count, and intensity to supported opacity/size values.
- [ ] Implement `buildExportHtml()` by cloning `document.documentElement`, setting `body.dataset.exportState`, removing transient drawer-open/focus/toast state, serializing `document.styleSheets` rules into a `<style>` element, removing `01-particles.css` link, and returning `<!doctype html>\n${clone.outerHTML}`. If CSSOM access fails or yields no rules, show an inline export error rather than returning incomplete HTML.
- [ ] Add copy flow: try `navigator.clipboard.writeText(html)`; on rejection/unavailable API, focus/select the read-only field and announce manual copy. Announce success only after fulfilled write.
- [ ] Browser-check the pilot from `file://`: edit values, switch style, export, verify `<style>` exists and `01-particles.css` link is absent, and confirm generated page restores the chosen style/settings.

## Task 3: CSS-Driven Pages

**Files:** `02-aurora.html/css`, `03-gradient.html/css`.

- [ ] Implement the same drawer/export contract in Aurora using each `styles[name]` record; edits produce a new per-render config and call `renderAurora(name)` without mutating the default record.
- [ ] Restore Aurora's exported style before its existing initial `renderAurora('purple')` call.
- [ ] Implement Gradient controls against the active stage class: color tint via a page-owned CSS custom property/filter, animation speed via active animation duration, intensity via supported opacity/blur. Export current class and control state; restore before first paint.
- [ ] Browser-check initial default, one color edit, one speed edit, switching styles, and copied-document startup for both pages.

## Task 4: Wave and Fluid Pages

**Files:** `04-waves.html/css`, `05-fluid.html/css`.

- [ ] Implement wave controls through active SVG path fill/stroke, dynamic-wave layer config, and flowing-canvas variables. Amplitude changes update path geometry or canvas layer amplitude; speed updates the active CSS animation or time multiplier.
- [ ] Restore wave style before activating a scene; start the flowing canvas loop only when its exported style is active and motion is allowed.
- [ ] Implement fluid controls through a cloned `fluidDynamicConfigs[name]` or a cloned predefined style config. Re-render current scene after edits; update blob color/size/count and motion duration where supported.
- [ ] Restore fluid style/settings before initial scene and before starting mouse/color-flow loops.
- [ ] Browser-check static and generated wave styles, flowing canvas, predefined fluid and generated goo scene, and copied-document startup on both pages.

## Task 5: Canvas and WebGL Pages

**Files:** `06-canvas.html/css`, `07-webgl.html/css`.

- [ ] Canvas controls update the selected renderer configuration without recreating unrelated renderer state; set renderer key/settings before initial `start()`. Scale renderer time for speed and apply supported color/count/spacing settings.
- [ ] WebGL controls update existing color uniforms, `u_speed`, `u_scale`, and mouse-response settings. Restore `data-style` before the initial `setShader()` and `start()` calls.
- [ ] Browser-check at least three Canvas renderer families and three WebGL shader families, then verify exported startup restores selected button, shader/renderer, and settings.

## Task 6: Cross-Page Export and Regression Validation

**Files:** all seven HTML/CSS pairs; `README.md`.

- [ ] Update README instructions to open `View code`, adjust the available controls, and use `Copy full HTML`; clarify that required CDNs remain external.
- [ ] For every page, verify trigger visibility, drawer keyboard/mouse closure, responsive layout, one live parameter change, style switching, complete HTML generation, inline CSS, retained CDN scripts where required, and correct first-load style restoration.
- [ ] Verify clipboard success when permitted and the select/manual-copy fallback when clipboard access is denied or unavailable.
- [ ] Check reduced-motion and hidden-tab behavior remain intact; run editor diagnostics on all seven HTML files and `git diff --check`.
- [ ] Inspect the final diff, commit the feature, and push `main`.
