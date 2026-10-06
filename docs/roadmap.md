# Roadmap & Limitations

Prism aims to be **on par with or better than** the projection-mapping market. Here's where it
stands and what's next, based on a survey of Resolume, MadMapper, QLab, TouchDesigner, Disguise,
Watchout, Smode, and community wish-lists.

## Done

<div class="grid cards" markdown>

- **Outputs** — multi-display full-screen, sub-region canvas, edge blending, HDR/10-bit.
- **Warp/mask/effects** — corner-pin, Catmull-Rom mesh, polygon masks, 20 GPU effects, blend modes.
- **Auto-calibration** — structured light → mesh/dome solve → multi-projector blend masks + black
  matching.
- **3D mapping** — OBJ/PLY/STL/USD import, virtual projector camera, PnP pose solver.
- **Audio** — per-device routing, FFT/beat input, modulators, muxed recording.
- **Timecode** — MTC send/receive, LTC generate/decode, follow sync.
- **I/O** — NDI, Syphon, SDI/DeckLink, HAP/NotchLC via FFmpeg.
- **Control** — OSC, MIDI learn, Art-Net, one shared parameter vocabulary.
- **Workflow** — scenes, timeline, undo, autosave, JSON projects.

</div>

## Next

### Calibration
- [ ] Camera **intrinsics / lens distortion** calibration (Brown–Conrady; Zhang from a checkerboard).
- [ ] Nonlinear **bundle refinement** (Levenberg–Marquardt) across all projectors + surface.
- [ ] Automatic **feature matching** to wire the PnP solver into 3D object alignment.
- [ ] Per-face **projective texturing** and occlusion masking for 3D objects.

### Media & codecs
- [ ] **glTF** import.
- [ ] **NDI HDR / alpha** and per-output NDI sizing.
- [ ] Validate NDI + DeckLink against **real hardware**.

### Output & sync
- [ ] Native **RTMP/SRT streaming**.
- [ ] Multi-channel / surround audio **routing** (per-channel maps).
- [ ] Close-as-possible **frame-lock** between projectors (Apple silicon has no genlock).

### Platform
- [ ] Public **GitHub Pages** hosting for these docs (currently local / MkDocs).
- [ ] Notarized builds and a signed release.

## Known limitations

- **Auto-calibration** is unit-verified but not yet hardware-verified; expect to add intrinsics
  calibration and bundle refinement for production.
- **Timecode** sync is approximate — no hardware genlock exists on Apple silicon.
- **NDI / SDI** are integrated but unvalidated against devices.
- **Browser** sources snapshot the page (fps-capped), not a zero-copy GPU path.
- **Mask** editing is point-based; polygon boolean operations are out of scope.
- **ffmpeg / DeckLink / NDI** integrations require the corresponding SDK/runtime to be installed.
