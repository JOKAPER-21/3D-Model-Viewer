# GitHub 3D Model Viewer

A free, browser-based 3D model viewer designed for GitHub Pages.

The viewer is intended to display common 3D asset formats directly in a modern web browser without requiring a backend server or paid hosting.

## Project Goal

Create a reusable web viewer for:

- FBX
- OBJ
- GLTF
- GLB

The viewer should preserve the original model's materials, textures, PBR properties, and supported animations.

Recommended deployment:

```text
Local 3D Software
        ↓
Export 3D Model
        ↓
Optimize / Verify Textures
        ↓
GitHub Repository
        ↓
GitHub Pages
        ↓
Browser 3D Viewer
```

---

## Technology

- HTML5
- CSS3
- JavaScript
- Three.js
- WebGL
- GitHub Pages
- No backend
- No database
- No paid hosting required

### Three.js Loaders

| Format | Loader | Textures | Animation |
|---|---|---:|---:|
| FBX | `FBXLoader` | Yes | Yes |
| OBJ | `OBJLoader` + `MTLLoader` | Yes | No |
| GLTF | `GLTFLoader` | Yes | Yes |
| GLB | `GLTFLoader` | Yes | Yes |

---

# Texture Support

The viewer is designed to support original model materials and textures rather than replacing them with a default material.

## Supported Material Maps

Where present in the source asset, the viewer should preserve:

- Base Color / Albedo
- Normal Map
- Roughness Map
- Metallic Map
- Ambient Occlusion Map
- Emissive Map
- Alpha / Transparency
- Double-sided materials
- PBR material properties

### Color Management

The viewer should correctly handle texture color spaces.

Color textures such as Base Color should use the appropriate sRGB color space, while data textures such as Normal, Roughness, Metallic, and AO should be treated as non-color data.

---

# Format and Texture Requirements

## GLB

GLB is the preferred format for web delivery.

A GLB file can contain:

```text
model
materials
textures
geometry
animations
```

inside one file.

Example:

```text
officeBuildingPillar_v01.glb
```

This makes GLB particularly convenient for:

- GitHub Pages
- Sharing
- Local file loading
- Drag and drop
- Reducing missing dependency problems

---

## GLTF

A typical GLTF asset may contain multiple files:

```text
officeBuildingPillar_v01.gltf
officeBuildingPillar_v01.bin
textures/
    baseColor.png
    normal.png
    roughness.png
    metallic.png
```

The relative paths inside the `.gltf` file must correctly point to the `.bin` file and textures.

Example:

```text
models/
└── officeBuildingPillar/
    ├── officeBuildingPillar_v01.gltf
    ├── officeBuildingPillar_v01.bin
    └── textures/
        ├── baseColor.png
        ├── normal.png
        ├── roughness.png
        └── metallic.png
```

---

## OBJ

OBJ normally requires additional files for materials and textures:

```text
officeBuildingPillar_v01.obj
officeBuildingPillar_v01.mtl
textures/
    baseColor.png
    normal.png
```

The `.obj` references the `.mtl`, and the `.mtl` references the texture files.

Example:

```text
models/
└── officeBuildingPillar/
    ├── officeBuildingPillar_v01.obj
    ├── officeBuildingPillar_v01.mtl
    └── textures/
        └── ...
```

If the `.mtl` or texture files are missing, the model may load without its original appearance.

---

## FBX

FBX texture handling depends on how the FBX was exported.

Textures may be:

- Embedded in the FBX
- Stored as external image files
- Referenced through paths from the original software

For reliable web deployment, verify that all required texture dependencies are available.

Example:

```text
officeBuildingPillar_v01.fbx
textures/
    baseColor.png
    normal.png
    roughness.png
```

---

# Local File Loading

The viewer should support a local file picker.

Example:

```text
Open Model
    ↓
Select .fbx / .obj / .gltf / .glb
    ↓
Detect extension
    ↓
Select appropriate Three.js loader
    ↓
Load model
    ↓
Auto-center
    ↓
Auto-fit camera
```

## Important Dependency Note

Single-file GLB loading is the simplest local workflow.

For multi-file formats such as OBJ + MTL + textures or GLTF + BIN + textures, the viewer should provide a dependency-aware loading method.

Possible implementations:

1. Folder selection using browser directory upload.
2. Multi-file selection.
3. Filename-to-Blob URL mapping.
4. Relative dependency resolution.

The viewer should display a useful error when a required dependency cannot be found.

---

# Drag and Drop

The viewer should support:

```text
Drag model here
        ↓
Detect file format
        ↓
Load model
        ↓
Resolve dependencies
        ↓
Display model
```

Supported extensions:

```text
.fbx
.obj
.gltf
.glb
```

For multi-file assets, the dependency files should be included in the selected folder/file set.

---

# URL Loading

The viewer should support loading a model from a URL.

Example:

```text
modelViewer_v01.html?model=../models/officeBuildingPillar/officeBuildingPillar_v01.glb
```

For GitHub Pages:

```text
https://jokaper-21.github.io/jokaper21-3dgs/viewers/modelViewer_v01.html?model=../models/officeBuildingPillar/officeBuildingPillar_v01.glb
```

The model URL should use HTTPS when hosted on GitHub Pages.

---

# Viewer Features

## Camera

The viewer should provide:

- Perspective camera
- Orbit controls
- Pan
- Zoom
- Camera damping
- Auto-fit
- Auto-center
- Reset camera

Suggested keyboard shortcuts:

| Key | Action |
|---|---|
| `R` | Reset camera |
| `G` | Toggle grid |
| `A` | Toggle axes |
| `W` | Toggle wireframe |
| `F` | Fullscreen |

---

# Scene Features

Recommended scene controls:

- Grid toggle
- Axes toggle
- Wireframe toggle
- Auto-rotate
- Fullscreen
- Background control
- Environment lighting
- Shadows where practical

The viewer should use a neutral lighting setup so that materials can be inspected clearly.

---

# Model Features

The viewer should preserve:

- Geometry
- Vertex positions
- Normals
- UV coordinates
- Materials
- Textures
- Transparency
- PBR properties
- Skinning
- Animations where supported

The viewer should not automatically replace the source material with a generic material unless the user explicitly enables a debug/display mode.

---

# Animation

Animations should be supported for:

- FBX
- GLTF
- GLB

Recommended UI:

```text
Animation:
[ None / Clip 1 / Clip 2 / ... ]

▶ Play
⏸ Pause
⏮ Reset
```

A future animation timeline can be added without changing the basic viewer architecture.

---

# Recommended GitHub Repository Structure

```text
jokaper21-3dgs/
│
├── index.html
│
├── viewers/
│   ├── gsViewer_v02.html
│   └── modelViewer_v01.html
│
├── models/
│   └── officeBuildingPillar/
│       ├── officeBuildingPillar_v01.glb
│       ├── officeBuildingPillar_v01.gltf
│       ├── officeBuildingPillar_v01.bin
│       ├── officeBuildingPillar_v01.fbx
│       ├── officeBuildingPillar_v01.obj
│       ├── officeBuildingPillar_v01.mtl
│       └── textures/
│           ├── baseColor.png
│           ├── normal.png
│           ├── roughness.png
│           ├── metallic.png
│           ├── ao.png
│           └── emissive.png
│
├── assets/
│   ├── css/
│   ├── js/
│   ├── icons/
│   └── thumbnails/
│
└── README.md
```

---

# Naming Convention

Use the following naming convention consistently:

```text
camelCase_words_v01
```

Recommended examples:

```text
modelViewer_v01.html
officeBuildingPillar_v01.glb
officeBuildingPillar_v01.gltf
officeBuildingPillar_v01.fbx
officeBuildingPillar_v01.obj
```

Avoid unnecessary numeric folder ordering such as:

```text
01_models
02_assets
03_viewers
```

Prefer descriptive folder names:

```text
models
assets
viewers
textures
renders
```

---

# GitHub Pages

The viewer can be hosted as a static website using GitHub Pages.

Expected repository:

```text
JOKAPER-21/jokaper21-3dgs
```

Expected Pages base URL:

```text
https://jokaper-21.github.io/jokaper21-3dgs/
```

Expected viewer URL:

```text
https://jokaper-21.github.io/jokaper21-3dgs/viewers/modelViewer_v01.html
```

---

# Example Model URL

For a GLB model:

```text
https://jokaper-21.github.io/jokaper21-3dgs/viewers/modelViewer_v01.html?model=../models/officeBuildingPillar/officeBuildingPillar_v01.glb
```

The viewer should read the `model` query parameter and automatically load the requested asset.

---

# Git Workflow

From the repository root:

```bash
git status
```

Add changes:

```bash
git add .
```

Commit:

```bash
git commit -m "Added 3D model viewer and model assets"
```

Push:

```bash
git push origin main
```

---

# Asset Workflow

Recommended workflow:

```text
3D Software
    ↓
Export FBX / OBJ / GLTF / GLB
    ↓
Verify geometry
    ↓
Verify UVs
    ↓
Verify materials
    ↓
Verify textures
    ↓
Prefer GLB for web delivery
    ↓
Copy to models/
    ↓
Git commit
    ↓
Git push
    ↓
GitHub Pages
    ↓
Open viewer URL
```

---

# Texture Verification Checklist

Before uploading an asset:

- [ ] Base Color texture exists
- [ ] Normal texture exists if required
- [ ] Roughness texture exists if required
- [ ] Metallic texture exists if required
- [ ] AO texture exists if required
- [ ] Emissive texture exists if required
- [ ] Texture paths are relative and valid
- [ ] Texture filenames contain no unexpected characters
- [ ] UV coordinates are correct
- [ ] Texture color spaces are correct
- [ ] Material assignments are correct
- [ ] Transparency is working
- [ ] Model scale is correct
- [ ] Model orientation is correct

---

# Performance Guidelines

For web viewing:

## Preferred

```text
GLB
```

because it can package the model and textures into a single file.

## Optimize When Necessary

Consider:

- Reducing unnecessary polygon density
- Removing hidden geometry
- Resizing excessively large textures
- Compressing textures where appropriate
- Removing unused materials
- Removing unused animation clips
- Using efficient GLB assets

The viewer should avoid unnecessary post-processing because the main goal is reliable model inspection.

---

# Large Files

GitHub should not be treated as unlimited production asset storage.

For large models, consider:

- Git LFS
- GitHub Releases
- External object storage
- Dedicated asset hosting

For normal-sized demonstration and portfolio assets, a GitHub repository can be sufficient.

---

# Error Handling

The viewer should clearly report:

```text
Unsupported format
Missing texture
Missing MTL
Missing BIN
Invalid GLTF
Failed to load model
Network error
File access error
WebGL error
```

Example:

```text
Model loaded successfully.

Format: GLB
Materials: 12
Textures: 18
Animations: 2
```

For missing dependencies:

```text
Model loaded, but some dependencies are missing.

Missing:
textures/normal.png
textures/roughness.png
```

---

# Recommended Default

For a production-friendly web asset:

```text
3D Software
    ↓
GLB export
    ↓
Embedded textures
    ↓
GitHub
    ↓
GitHub Pages
    ↓
Three.js GLTFLoader
    ↓
Browser
```

This minimizes dependency problems and makes sharing easier.

---

# Future Features

Possible future additions:

- Model gallery
- Thumbnail preview
- Animation timeline
- Material inspector
- Texture inspector
- Polygon count
- Vertex count
- Draw call statistics
- Bounding box information
- Scene hierarchy
- Object visibility controls
- Screenshot capture
- Download button
- QR code sharing
- Model metadata
- Multiple model comparison
- Environment/HDRI selection
- Exposure control
- Tone mapping control
- Background color control

---

# Final Architecture

```text
┌─────────────────────────────┐
│       Local 3D Software     │
│                             │
│ FBX / OBJ / GLTF / GLB      │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│     Model + Textures        │
│                             │
│ Geometry                    │
│ UVs                         │
│ Materials                   │
│ PBR Maps                    │
│ Animations                  │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│       GitHub Repository     │
│                             │
│ models/                     │
│ viewers/                    │
│ assets/                     │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│        GitHub Pages         │
│                             │
│ Static HTTPS Website        │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│       Three.js Viewer       │
│                             │
│ FBXLoader                   │
│ OBJLoader + MTLLoader       │
│ GLTFLoader                  │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│        Web Browser          │
│                             │
│ Orbit / Pan / Zoom          │
│ Materials / Textures        │
│ Animation                   │
│ Grid / Axes / Wireframe     │
└─────────────────────────────┘
```

---

# License

Choose an appropriate license for your own viewer source code and separately verify the licenses of:

- Three.js
- Third-party loaders
- HDR/environment assets
- Fonts
- Icons
- 3D models
- Textures

Do not assume that a downloaded 3D model or texture is free to redistribute merely because it is publicly available.

---

# Status

**Project:** GitHub 3D Model Viewer  
**Version:** `v01`  
**Primary formats:** FBX, OBJ, GLTF, GLB  
**Texture support:** Yes  
**PBR support:** Yes  
**Animation support:** FBX / GLTF / GLB  
**Hosting target:** GitHub Pages  
**Backend:** None  
**Cost target:** Free
