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
| [`01-particles.html`](01-particles.html) | [tsParticles](https://particles.js.org/) (CDN) | Classic, Network, Snow, Bubble, Galaxy, Neon |
| [`02-aurora.html`](02-aurora.html) | CSS blobs + vanilla JS | Purple, Ocean, Sunset, Emerald, Northern Lights, Neon |
| [`03-gradient.html`](03-gradient.html) | CSS gradients + SVG filters | Mesh, Blob, Linear, Radial, Liquid, Multi-color |
| [`04-waves.html`](04-waves.html) | SVG + Canvas | Simple, Multi-layer, Ocean, Gradient, SVG, Flowing |
| [`05-fluid.html`](05-fluid.html) | CSS goo/SVG filters + Canvas | Fluid Blobs, Metaballs, Liquid, Organic, Mouse Interaction, Color-flow |
| [`06-canvas.html`](06-canvas.html) | Canvas API | Flow Field, Trails, Generative Lines, Noise Field, Interactive, Waves |
| [`07-webgl.html`](07-webgl.html) | [Three.js](https://threejs.org/) (CDN) shaders | Gradient, Noise, Liquid, Wave, Organic Blobs, Interactive Mouse |

Every page shares the same design language: fixed navbar to jump between pages, a glassmorphism
content card, a style-selector to switch variations **without reloading**, mouse-reactive parallax +
cursor spotlight + click ripple, and respect for `prefers-reduced-motion`.

See [`DESIGN-CATALOG.md`](DESIGN-CATALOG.md) for the full roadmap — the goal is 50 variations per
page (350 total), implemented incrementally.

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
