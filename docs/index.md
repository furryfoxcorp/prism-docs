# Prism

**Native macOS projection mapping.** GPU compositing on Metal, full-screen outputs on any
display, corner-pin and mesh warping, structured-light auto-calibration for multi-projector and
curved/dome surfaces, 3D object mapping, per-device audio, timecode, and NDI/Syphon/SDI I/O.

![Prism editor](assets/ui-01-editor.png)

<div class="grid cards" markdown>

- :material-projector:{ .lg .middle } **Multi-projector outputs**

    ---

    Unlimited full-screen output windows — one per HDMI/Thunderbolt/DisplayPort display — showing
    sub-regions of a shared virtual canvas, with per-output edge blending and HDR.

- :material-grid:{ .lg .middle } **Warp, mask, effects**

    ---

    True projective corner-pin, Catmull-Rom mesh warp, feathered polygon masks, 20 GPU effects,
    blend modes, and per-layer + master color correction.

- :material-camera:{ .lg .middle } **Auto-calibration**

    ---

    Gray-code structured light → dense camera↔projector correspondences → mesh warp. Solves
    planes, curved surfaces, and domes; generates normalized blend masks and matches black levels.

- :material-cube:{ .lg .middle } **3D mapping**

    ---

    Import OBJ/PLY/STL/USD, texture from any source, render from a virtual projector camera.
    A DLT PnP solver recovers a real projector's pose.

- :material-speaker:{ .lg .middle } **Audio**

    ---

    Route each source's audio to any CoreAudio device, capture FFT/beat input, modulate any
    parameter, and mux program audio into recordings.

- :material-network:{ .lg .middle } **I/O & control**

    ---

    NDI in/out, Syphon in/out, SDI/DeckLink, HAP/NotchLC via FFmpeg, OSC/MIDI/Art-Net, and
    MTC/LTC timecode.

</div>

## Why Prism

The macOS media-server market has real gaps: no mapper does per-device audio routing well, most
have weak or no timelines, camera-based auto-calibration is a $15k–40k feature, and free tools
have no undo. Prism closes those gaps in one native app — Apple-silicon Metal for the hot path,
no Electron, no subscription.

## At a glance

| | |
| --- | --- |
| Platform | macOS 14+, Apple silicon or Intel, Metal GPU |
| Language | Swift 6 + Metal (runtime-compiled shaders) |
| Backends | AVFoundation, ScreenCaptureKit, CoreAudio, CoreMIDI, ModelIO |
| Optional | ffmpeg-full (HAP/NotchLC), DeckLink SDK (SDI), NDI SDK, Syphon.framework |
| Rendering | Main-thread, vsync-locked, lock-free document model |

## Quick start

```sh
git clone https://github.com/furryfoxcorp/prism
cd prism
scripts/build_app.sh release     # fetches/builds Syphon, embeds libndi
open build/Prism.app
```

Generate the two-projector demo:

```sh
swift run Prism --emit-sample examples/demo.prism
```

Run the headless self-test (validates shaders, pipelines, mesh/mask math, structured-light
decode, blend normalization, PnP, LTC, and more):

```sh
PRISM_SELFTEST=1 swift run Prism
```

Start with **[Getting Started](getting-started.md)**, then the **[User Guide](user-guide.md)** and
**[Auto-Calibration](calibration.md)**.
