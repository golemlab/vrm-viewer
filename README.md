# VRM Viewer with VRMA & BVH Animation

[English](README.md) | [org 日本語](README-jp.md)

A web-based VRM (Virtual Reality Model) viewer with VRMA (VRM Animation) and BVH motion capture support built using Three.js and the three-vrm library. Single-file app (`index.html`), no build system, all dependencies CDN-loaded.

## Recent Development Summary

- **Model management** — Dynamic VRM model discovery (directory listing + JSON manifest), model selector dropdown, proper cleanup/disposal.
- **VRMA animations** — Dynamic discovery + manifest, dropdown-based selection.
- **BVH animation support** — Full BVH motion capture loading with 100+ animations, playback controls, hips position grounding (cm→m), and bone filtering.
- **VRM 0.x compatibility** — `VRMUtils.rotateVRM0` for version-aware orientation, quaternion Y-axis inversion for correct BVH rotation retargeting on VRM 0.x models.

## 🎮 Live Demo

**[Try the Demo from the org creator →](https://tk256ailab.github.io/vrm-viewer/)**


## Features

- 📱 **Responsive Design**: Works on desktop and mobile devices
- 🎭 **VRM Model Support**: Load and display VRM 0.x and 1.0 models
- 🎬 **VRMA Animation**: Play custom VRMA animation files
- 🎯 **BVH Motion Capture**: Play 100+ BVH motion capture animations
- 🎮 **Interactive Controls**: Play, pause, and stop animations
- 🎨 **Modern UI**: Clean, gradient-based interface
- ⚡ **Fast Performance**: Optimized rendering and animations

## Demo

Open `index.html` in a web browser to see the demo. The viewer includes:

- A sample VRM model (sample.vrm)
- Eleven VRMA animation examples:
  - **Angry**: Angry emotion animation
  - **Blush**: Blushing emotion animation
  - **Clapping**: Clapping hands animation
  - **Goodbye**: Waving goodbye animation
  - **Jump**: Jumping action animation
  - **LookAround**: Looking around animation
  - **Relax**: Relaxed pose animation
  - **Sad**: Sad emotion animation
  - **Sleepy**: Sleepy emotion animation
  - **Surprised**: Surprised emotion animation
  - **Thinking**: Thinking pose animation

## Project Structure

```
vrm-viewer/
├── index.html                  # Main viewer application (all logic)
├── AGENTS.md                   # AI agent guidelines
├── VRM/
│   ├── models.json             # Model manifest for GitHub Pages
│   └── *.vrm                   # VRM model files
├── VRMA/
│   ├── manifest.json           # Animation manifest for GitHub Pages
│   └── *.vrma                  # VRMA animation files
├── bvh.animation/
│   ├── manifest.json           # BVH manifest for GitHub Pages
│   └── *.bvh                   # BVH motion capture files (100+)
├── README.md
└── README-jp.md
```

## Quick Start

### Method 1: GitHub Pages (Recommended)

1. **Fork or upload** this repository to GitHub
2. **Enable GitHub Pages**:
   - Go to your repository's Settings
   - Scroll down to "Pages" section
   - Under "Source", select "Deploy from a branch"
   - Choose "main" branch and "/ (root)" folder
   - Click "Save"
3. **Access your demo** at `https://YOUR-USERNAME.github.io/YOUR-REPOSITORY-NAME/`

### Method 2: Local Development

1. **Clone or download** this repository
2. **Start a local web server** (required for loading files):
   ```bash
   # Using Python
   python -m http.server 8000
   
   # Using Node.js
   npx serve .
   
   # Using PHP
   php -S localhost:8000
   ```
3. **Open your browser** and navigate to `http://localhost:8000`
4. **Load the VRM model** (automatically loads on page load)
5. **Select animations** using the VRMA buttons
6. **Control playback** with Play, Pause, and Stop buttons

## Usage

### Loading VRM Models

The viewer discovers VRM models automatically via directory listing (local servers) or `VRM/models.json` (GitHub Pages). To add a model, place a `.vrm` file in `VRM/` and add its filename to `VRM/models.json`.

### Playing Animations

1. Wait for the VRM model to load completely
2. Select a VRMA or BVH animation from the respective dropdown
3. Animation starts playing automatically
4. Use Play/Pause/Stop controls as needed

### Camera Controls

- **Rotate**: Left-click and drag to rotate the camera around the model
- **Pan**: Right-click and drag to move the camera horizontally/vertically
- **Zoom**: Scroll with mouse wheel to zoom in/out

### Controls

- **VRMA Animation Buttons**: Select and load different animations
- **Play**: Start or resume animation playback
- **Pause**: Pause/unpause the current animation
- **Stop**: Stop animation and reset to default pose

## Technical Details

### Dependencies

- [Three.js](https://threejs.org/) - 3D graphics library
- [@pixiv/three-vrm](https://github.com/pixiv/three-vrm) - VRM model support
- [@pixiv/three-vrm-animation](https://github.com/pixiv/three-vrm-animation) - VRMA animation support

### Animation Specifications

- **VRMA Format**: VRMA (VRM Animation) files in glTF binary format, compatible with VRM 1.0 humanoid specification
- **BVH Format**: Biovision Hierarchy motion capture files, auto-retargeted to VRM humanoid bones
- **Frame Rate**: 60 FPS with linear interpolation
- **Duration**: Variable (4-12 seconds for included animations)

### Browser Compatibility

- ✅ Chrome 80+
- ✅ Firefox 75+
- ✅ Safari 14+
- ✅ Edge 80+

## Customization

### Adding New Animations

**VRMA:**
1. Place `.vrma` files in `VRMA/`
2. Add filename to `VRMA/manifest.json`

**BVH:**
1. Place `.bvh` files in `bvh.animation/`
2. Add filename to `bvh.animation/manifest.json`
3. BVH files must use VRM humanoid bone names (hips, spine, chest, etc.)

### Styling

The interface uses CSS custom properties for easy theming. Key variables:

- Background colors and gradients
- Button styling and hover effects
- Control panel appearance
- Responsive breakpoints

## License

This project is for demonstration purposes. Please ensure you have appropriate rights for any VRM models and animations you use.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## Acknowledgments

- [three-vrm](https://github.com/pixiv/three-vrm) - VRM support for Three.js
- [Three.js](https://threejs.org/) - 3D graphics foundation
- VRM Consortium - VRM format specification
