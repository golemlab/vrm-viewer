# AGENTS.md

## Project

Single-file (`index.html`) web viewer for VRM 3D models with VRMA and BVH animations. No build system, no package manager, no bundler. All dependencies are CDN-loaded via ES module importmap.

## Running Locally

A web server is required (fetch calls fail on `file://`):

```bash
npx serve .
# or
python -m http.server 8000
```

Open `http://localhost:8000`. No install step needed.

## Codebase

All application logic is in `index.html`. There are no other JS files.

- `VRM/` — VRM model files (`.vrm`). Model discovery uses directory listing or falls back to `VRM/models.json`.
- `VRMA/` — VRMA animation files (`.vrma`). Animation discovery uses directory listing or falls back to `VRMA/manifest.json`.
- `bvh.animation/` — BVH motion capture animation files (`.bvh`). Animation discovery uses directory listing or falls back to `bvh.animation/manifest.json`. BVH bone names already match VRM humanoid bone names (hips, spine, chest, etc.), so no bone name mapping is needed.
- Dependencies (via CDN importmap): Three.js 0.176, @pixiv/three-vrm 3, @pixiv/three-vrm-animation 3. BVHLoader is from Three.js addons.

## Adding a New VRM Model

1. Place `.vrm` file in `VRM/`
2. Add filename to `VRM/models.json` (needed for GitHub Pages, which has no directory listing)

## Adding a New VRMA Animation

1. Place `.vrma` file in `VRMA/`
2. Add filename to `VRMA/manifest.json` (needed for GitHub Pages)

## Adding a New BVH Animation

1. Place `.bvh` file in `bvh.animation/`
2. Add filename to `bvh.animation/manifest.json` (needed for GitHub Pages)
3. BVH files must use VRM humanoid bone names (hips, spine, chest, head, leftUpperArm, etc.)

## Gotchas

- **No tests, no lint, no typecheck.** There are no quality gates.
- **No npm/package.json.** `node_modules/` in `.gitignore` is aspirational.
- **Model filename with spaces** — filenames like `Lara Lightland.vrm` are URI-encoded at runtime; this works but keep it in mind if debugging fetch errors.
- **Manifest files must stay in sync** with actual files in their directories for GitHub Pages to find models/animations.
- **BVH Hips position scaling** — BVH uses centimeters, VRM uses meters. The loader scales Hips position by 0.01.
- **BVH bone filtering** — Bones not present in the VRM model (e.g. leftToes, rightToes) are automatically skipped with a console warning.
