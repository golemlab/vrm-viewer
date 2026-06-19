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
- `bvh.animation.small/` — A smaller/alternate set of BVH files plus some VRMA files. Not wired into the app's discovery system; appears to be a development/testing directory.
- **Do not read `bvh.animation/` or `bvh.animation.small/` directory listings.** These directories contain 100+ BVH files each and are extremely token-heavy to enumerate. If you need to inspect a specific BVH file, read only that single file by name.
- Dependencies (via CDN importmap): Three.js 0.176, @pixiv/three-vrm 3, @pixiv/three-vrm-animation 3. BVHLoader is from Three.js addons.

## Adding a New VRM Model

1. Place `.vrm` file in `VRM/`
2. Add filename to `VRM/models.json` (needed for GitHub Pages, which has no directory listing)

Note: `models.json` is currently out of sync with files on disk. Several models exist in `VRM/` that are not listed in the manifest (e.g. `Freyja.vrm`, `VRM Girl.vrm`, `org_sampleV1.vrm`, `dodoo.vrm`, `Sample01.vrm`, `Galaxy Summer.vrm`). Local directory listing still finds them; GitHub Pages would not.

## Adding a New VRMA Animation

1. Place `.vrma` file in `VRMA/`
2. Add filename to `VRMA/manifest.json` (needed for GitHub Pages)

## Adding a New BVH Animation

1. Place `.bvh` file in `bvh.animation/`
2. Add filename to `bvh.animation/manifest.json` (needed for GitHub Pages)
3. BVH files must use VRM humanoid bone names (hips, spine, chest, head, leftUpperArm, etc.)

## Runtime Behavior

- On load, the app discovers models/animations via directory listing (works with local servers), falling back to the JSON manifests (works on GitHub Pages).
- Selecting a VRM model or animation from the dropdown auto-loads it; selecting an animation also auto-plays it.
- The app detects VRM version via `vrm.meta.metaVersion` (`'0'` or `'1'`).
- `VRMUtils.rotateVRM0()` rotates VRM 0.x models to face Z+ (matching VRM 1.0 convention).

## BVH Animation Retargeting

- **Hips position**: BVH uses centimeters, VRM uses meters. The loader scales by 0.01 and offsets to align with the VRM model's hips rest position.
- **VRM 0.x rotation fix**: VRM 0.x uses left-handed bone orientations while BVH uses right-handed. For VRM 0.x models, each BVH quaternion has its X and Z components negated (`-x, y, -z, w`) to correct limb direction.
- **Bone filtering**: Bones present in BVH but absent in the VRM model are silently skipped.

## Gotchas

- **No tests, no lint, no typecheck.** There are no quality gates.
- **No npm/package.json.** `node_modules/` in `.gitignore` is aspirational.
- **Model filename with spaces** — filenames like `Lara Lightland.vrm` are URI-encoded at runtime; this works but keep it in mind if debugging fetch errors.
- **Manifest files must stay in sync** with actual files in their directories for GitHub Pages to find models/animations.
