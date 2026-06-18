# AGENTS.md

## Project

Single-file (`index.html`) web viewer for VRM 3D models with VRMA animations. No build system, no package manager, no bundler. All dependencies are CDN-loaded via ES module importmap.

## Running Locally

A web server is required (fetch calls fail on `file://`):

```bash
npx serve .
# or
python -m http.server 8000
```

Open `http://localhost:8000`. No install step needed.

## Codebase

All application logic is in `index.html` (~566 lines). There are no other JS files.

- `VRM/` — VRM model files (`.vrm`). Model discovery uses directory listing or falls back to `VRM/models.json`.
- `VRMA/` — VRMA animation files (`.vrma`). Paths are hardcoded in `VRMA_ANIMATION_URLS` array.
- Dependencies (via CDN importmap): Three.js 0.176, @pixiv/three-vrm 3, @pixiv/three-vrm-animation 3.

## Adding a New VRM Model

1. Place `.vrm` file in `VRM/`
2. Add filename to `VRM/models.json` (needed for GitHub Pages, which has no directory listing)

## Adding a New VRMA Animation

1. Place `.vrma` file in `VRMA/`
2. Add path to `VRMA_ANIMATION_URLS` array in `index.html`
3. Add a `<button class="vrma-btn">` in the `#vrmaButtons` div in `index.html`

## Gotchas

- **No tests, no lint, no typecheck.** There are no quality gates.
- **No npm/package.json.** `node_modules/` in `.gitignore` is aspirational.
- **Model filename with spaces** — filenames like `Lara Lightland.vrm` are URI-encoded at runtime; this works but keep it in mind if debugging fetch errors.
- **VRM/models.json must stay in sync** with actual files in `VRM/` for GitHub Pages to find models.
- **The VRMA_ANIMATION_URLS array order must match the button order** in the HTML — buttons are index-mapped to URLs.
