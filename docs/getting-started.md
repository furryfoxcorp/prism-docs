# Getting Started

## Requirements

- **macOS 14** or later, Apple silicon or Intel Mac with a Metal-capable GPU.
- **Swift 6 toolchain** to build (Command Line Tools are enough for the core build).
- A full **Xcode** install (or the standalone **Metal Toolchain** component) is needed only to
  compile `Syphon.framework`. Without it Prism still builds and runs with Syphon disabled.

### Optional integrations (auto-detected, none required)

Prism probes your machine at build time and only compiles in what is present. Everything degrades
gracefully when absent.

| Backend | Provides | Where it looks |
| --- | --- | --- |
| Homebrew `ffmpeg-full` | HAP / HAP Alpha / HAP Q / NotchLC / DXV decode | `/opt/homebrew/opt/ffmpeg-full` |
| DeckLink SDK headers | SDI / HDMI capture + output | `vendor/decklink/` (+ Desktop Video runtime) |
| NDI SDK for Apple | NDI input + output | `/Library/NDI SDK for Apple` |
| Syphon.framework | Syphon input + output | built automatically by the script |

Native codecs (ProRes, DNxHR, H.264, HEVC) are decoded by VideoToolbox and never need FFmpeg.

## Build & run

```sh
scripts/build_app.sh release     # produces build/Prism.app
open build/Prism.app
```

The build script:

1. Clones and builds `Syphon.framework` (if the Metal toolchain is available).
2. Builds Syphon, then Prism via `swift build`.
3. Assembles `Prism.app`, embeds `Syphon.framework` and `libndi.dylib`, and ad-hoc signs it.

Or run from source:

```sh
swift run Prism
```

!!! note "Syphon from `swift run`"
    Syphon only loads when the framework is next to the binary, so use `scripts/build_app.sh` for
    full Syphon support. From `.build`, point `PRISM_SYPHON_PATH` at a built
    `Syphon.framework/Syphon` to enable it.

## Demo project

```sh
swift run Prism --emit-sample examples/demo.prism
```

This writes a two-projector, edge-blended demo you can open and play with.

## Self-test

```sh
PRISM_SELFTEST=1 swift run Prism
```

Runs headless and validates: Metal shader compilation and every pipeline, mesh/mask math,
composition and output passes (with pixel readback), the effect chain, audio device enumeration,
NDI runtime + DeckLink API availability, Syphon publish, FFmpeg HAP decode, the homography/DLT and
PnP solvers, structured-light Gray-code decode, mesh solve, blend-mask normalization, the LTC
biphase codec, parameter resolution, and recording (video + muxed audio). Optional assets enable
more checks:

```sh
PRISM_SYPHON_PATH=... ./Prism            # Syphon round-trip
PRISM_HAP_TEST=/path/file.mov ./Prism    # FFmpeg HAP decode
PRISM_AV_TEST=/path/audio.wav ./Prism    # recording audio mux
```

## UI drive test

`PRISM_DRIVE_TEST=1` launches the real app and drives every control through the Accessibility API
— tool modes, toolbar toggles, menus, inspector tabs/toggles/sliders/picker, the timeline, and the
per-output render pass — asserting each action against live state and reading rendered pixels back.
Results are written to `/tmp/drive_results.txt`.

```sh
PRISM_DRIVE_TEST=1 swift run Prism
```

## First run

Prism opens the editor and, if you have a second display connected, an output window on it. See
the **[User Guide](user-guide.md)** to add a surface, choose a source, and warp it.
