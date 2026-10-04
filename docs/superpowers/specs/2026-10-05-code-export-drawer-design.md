# Code Export Drawer Design

Date: 2026-10-05
Status: Proposed for user review

## Purpose

Let a visitor customize the currently selected background, preview changes immediately, inspect the generated page, and copy a complete standalone HTML document that opens with that selected style and its current settings.

## Scope

Apply the feature consistently to all seven showcase pages:

- `01-particles.html`
- `02-aurora.html`
- `03-gradient.html`
- `04-waves.html`
- `05-fluid.html`
- `06-canvas.html`
- `07-webgl.html`

Each page retains its own same-named stylesheet and inline JavaScript. The feature adds no build system, framework, or shared stylesheet/script dependency.

## Interaction Design

- Add a persistent `View code` button to each page.
- Clicking it opens a right-side drawer on desktop and a bottom sheet on narrow screens. The active background remains visible behind the panel.
- The drawer identifies the selected style and exposes applicable live controls: background/primary/accent colors, animation speed, intensity, and particle or shape density when supported by that engine.
- Controls update the displayed background immediately and remain synchronized with the selected style.
- The code area displays the complete generated HTML. `Copy full HTML` copies it and shows a success message; if clipboard access is unavailable, the code is selected with a clear manual-copy prompt.
- Close controls include a close button, Escape key, and backdrop click. Focus remains usable by keyboard, and opening the drawer moves focus to its heading or first control.

## Parameter Model

Use small page-specific adapters rather than pretending every renderer has identical parameters. A setting is shown only when that renderer can apply it meaningfully.

- Particles: particle/link colors, movement speed, and count.
- Aurora: palette colors, motion speed, and glow strength.
- Gradient: gradient colors, animation speed, and intensity/blur where applicable.
- Waves: fill/stroke colors, wave speed, and amplitude.
- Fluid: blob colors, movement speed, and blob count/size.
- Canvas: particle/line colors, animation speed, and count or spacing.
- WebGL: shader colors, time speed, scale/intensity, and mouse response where supported.

Each page keeps its existing variation selector. Selecting a variation loads that variation's defaults into the drawer; adjusting a control changes the current preview only and does not mutate the catalog defaults.

## Full-Page Export

The export represents the current showcase page, not a reduced background snippet. It keeps the navbar, content card, style selector, drawer, and all behavior, but opens initially with the currently selected effect and edited settings.

- Generate from a clone of the live document so the selected variation and active UI state are preserved.
- Inline the current page stylesheet into the exported document and remove its relative stylesheet link, making the copied output a single HTML file.
- Preserve required third-party CDN script tags (tsParticles or Three.js); no external libraries are bundled into the export.
- Store the selected style and supported parameter values in export-safe markup/configuration. Each page's startup code reads this state and initializes the chosen effect instead of always forcing the original default.
- Keep relative navigation behavior usable in the source showcase. The exported full showcase remains a demo artifact; adapting the background alone into an application remains a separate integration task.

## Architecture

- Add matching drawer markup and styles to each HTML/CSS pair, following the current local conventions.
- Add a compact inline page-specific controller in each page. It owns control values, applies them to the active renderer, and produces the export document.
- Keep renderer-specific defaults/configs as the source of truth. Export state overrides only the selected style's supported values.
- Avoid a shared runtime asset so every page remains independently openable and the existing standalone constraint is retained.

## Error Handling and Accessibility

- Handle clipboard rejection and unavailable clipboard APIs with a selectable-text fallback.
- Guard setting changes against missing renderer instances and invalid values; keep the last valid preview and show a concise status message.
- Use labels for every control, expose drawer state with appropriate dialog semantics, and support Escape/keyboard focus.
- Respect `prefers-reduced-motion`; the drawer itself remains usable when animation is reduced.
- Keep animation lifecycle behavior intact: pause while the tab is hidden and avoid creating additional permanent animation loops for the drawer.

## Acceptance Criteria

1. Every one of the seven pages has a visible `View code` control.
2. Drawer layout works on desktop and mobile, can be opened/closed with mouse and keyboard, and does not obscure the whole background unnecessarily.
3. Each page exposes controls that actually affect its current style; selecting another style resets controls to that style's values.
4. The generated HTML is a single document with the page CSS inline and any required CDN dependencies retained.
5. Opening copied HTML reproduces the selected style and adjusted settings on initial load.
6. Copy succeeds where clipboard permission exists and offers a usable fallback otherwise.
7. All seven pages are manually/browser validated for representative style changes and exported startup behavior.
8. Existing style switching, mouse interaction, reduced-motion behavior, and hidden-tab pausing continue to work.

## Alternatives Considered

- **Static source viewer:** minimal effort, but users must find and edit renderer configuration themselves; rejected because it does not provide the requested live parameter editing.
- **Background-only embed snippet:** best for direct application integration, but the user selected a complete standalone page export; not the primary export in this feature.
- **One shared runtime module:** reduces duplicated controller code, but makes pages dependent on another local file and weakens the independent-page requirement; rejected.
