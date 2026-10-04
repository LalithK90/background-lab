# Background Lab

A free, open-source collection of production-quality **animated background techniques** built with
plain **HTML + CSS + vanilla JavaScript** — no React, no Vue, no build tools, no npm. Every page works
by itself: open the `.html` file in a browser, or copy the file + its matching same-named `.css` file
straight into any project (Spring Boot + Thymeleaf, plain static sites, anything that serves HTML).

**Live demo:** https://lalithk90.github.io/background-lab/

## Why this exists

Picking the "right" animated background for a landing page usually means testing a dozen ideas.
This repo lets you flip through **many visually distinct variations of the same technique** side by
side, pick the one you like, and lift the code directly into your project.

## Pages

| Page | Technique | Styles |
|---|---|---|
| [`01-particles.html`](01-particles.html) | [tsParticles](https://particles.js.org/) (CDN) | 50 variations |
| [`02-aurora.html`](02-aurora.html) | CSS blobs + vanilla JS | 50 variations |
| [`03-gradient.html`](03-gradient.html) | CSS gradients + SVG filters | 50 variations |
| [`04-waves.html`](04-waves.html) | SVG + Canvas | 50 variations |
| [`05-fluid.html`](05-fluid.html) | CSS goo/SVG filters + Canvas | 50 variations |
| [`06-canvas.html`](06-canvas.html) | Canvas API | 50 variations |
| [`07-webgl.html`](07-webgl.html) | [Three.js](https://threejs.org/) (CDN) shaders | 50 variations |

Every page shares the same design language: fixed navbar to jump between pages, a glassmorphism
content card, a style-selector to switch variations **without reloading**, mouse-reactive parallax +
cursor spotlight + click ripple, and respect for `prefers-reduced-motion`.

## Customize and copy

Choose a variation, then select **View code** to open its live customizer. Adjust the color tint,
speed, brightness, and available renderer-specific controls such as particle density, blob size,
wave amplitude, or shader scale to preview changes immediately. **Copy full HTML** exports the
complete showcase page with the selected variation and settings, its stylesheet embedded, and
required CDN libraries left as external links. If clipboard access is blocked by the browser, the
generated HTML is selected for manual copying.

## Using a background in your own project

1. Copy the page you want (e.g. `02-aurora.html`) and its same-named CSS file (`02-aurora.css`).
2. Keep the `<div id="background">...</div>` markup and the matching `<script>` block — that's the
   whole animation. Strip out the navbar/style-selector/glass-card if you only want the background.
3. If the page uses a CDN library (tsParticles, Three.js), keep the `<script src="...">` tag.
4. Drop the markup into your template (Thymeleaf, JSP, plain HTML, etc.) and adjust colors/timings
   inside the CSS custom properties and JS config objects to fit your brand.

## Tech constraints

- Plain HTML + CSS + JavaScript only
- No React / Vue / Angular / Svelte / Next.js / Nuxt / Vite / npm / build step
- CDN libraries only where genuinely needed (tsParticles, Three.js)
- Every animation respects `prefers-reduced-motion` and pauses when the tab is hidden

## License

MIT — see [`LICENSE`](LICENSE). Free to use in personal and commercial projects.

## Contributing

Issues and PRs adding new style variations (see the catalog) are welcome.
