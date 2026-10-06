# Changelog

Recent changes to Prism, newest first. Feature docs live in the pages above; this page tracks
behavioral and stability changes.

## 2026-10-05 — Output reliability, calibration crash, and a UI test harness

### Output windows
- **Fixed black/blank output.** Output views were never assigned the app state, so their draw
  path bailed every frame and the borderless window just showed its black background. Outputs now
  render correctly.
- **Fixed a crash during camera calibration.** Repeatedly setting the output window frame while
  Core Animation transactions were in flight caused an over-release of a window transform
  animation. Output windows no longer animate (`animationBehavior = .none`), `setFrame`/`orderFront`
  only run when something actually changed, and output rendering uses a **single timer driver**
  instead of mixing `MTKView`'s display link with a manual watchdog.
- **Reopen / focus.** Selecting an output shows its settings and reopens its window; the
  **"Open / Focus Output"** button reopens a closed window and auto-assigns a free display when
  none is set.

### Inspector
- **Fixed the settings pane not switching.** Surface and output selection are now mutually
  exclusive via explicit selectors, so clicking an output in the sidebar shows the Output panel
  (previously a stale surface selection kept winning).

### Menus
- **Fixed "Toggle Output Regions".** It mutated a temporary copy of the project through optional
  chaining, so it never took effect. It now routes through a proper state method.

### Stability
- Camera and screen-capture providers no longer force-unwrap their texture cache (no crash if the
  cache can't be created).
- NDI and DeckLink capture shims clamp their copies to the destination buffer, preventing
  out-of-bounds writes on sources larger than the allocated frame (e.g. 4K SDI into a 1080p buffer,
  8K NDI).
- GPU caches (warp/effect/3D/blend-mask) and empty audio device graphs are torn down when surfaces
  or outputs are deleted.
- Sliders now carry accessibility labels.

### Testing
- Added a **self-driving UI test harness** (`PRISM_DRIVE_TEST=1`). It activates every control
  through the Accessibility API and verifies each action against live state — tool modes, toolbar
  toggles, menus, inspector tabs, toggles, sliders, the source picker, timeline, and the
  per-output render pass. See **[Architecture → Verification](architecture.md#verification)**.
