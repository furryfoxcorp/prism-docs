# Control

Prism is fully controllable from OSC, MIDI, and Art-Net. All three share one **parameter
vocabulary**, so anything you can MIDI-map you can also modulate with an LFO or the audio
spectrum.

## Parameter paths

A parameter is addressed by a dotted path:

```
master.opacity   master.blackout   master.freeze
surface.<i>.opacity          surface.<i>.enabled
surface.<i>.source.color.r   surface.<i>.source.volume
surface.<i>.color.gamma      surface.<i>.color.brightness
surface.<i>.mask.enabled     surface.<i>.mask.feather
surface.<i>.transform.position.x   ...scale.x   ...rotation
output.<i>.enabled
output.<i>.edgeBlend.enabled / left / right / top / bottom / gamma
output.<i>.region.x / y / w / h
scene.<i>
```

Indices are **1-based**.

## OSC

- **Receive** on a configurable port. Direct addressing works out of the box: send a float to
  `/prism/surface/1/opacity` (the leading `/prism/` is stripped and slashes become dots).
- **Mappings** let you bind a named OSC address to any target path with a value range.
- **Send** for feedback (scene recall, master state) to a configured host/port.

| OSC address | Effect |
| --- | --- |
| `/prism/master/opacity` | master opacity |
| `/prism/master/blackout` | blackout (>0.5 on) |
| `/prism/surface/1/opacity` | surface opacity |
| `/prism/surface/1/color/gamma` | color correction |
| `/prism/output/1/edgeBlend/left` | edge-blend width |
| `/prism/scene/1` | recall scene (>0.5) |

## MIDI

MIDI CCs and notes map to target paths with a one-click **Learn**: arm a mapping, wiggle a
control, done. Channel filtering supports omni or a specific channel.

## Art-Net

DMX input on port **6454** (ArtDmx). Map a universe/channel to a target path with a 0–255 range.

## Mappings

The **Control** panel lists all mappings (source, address/CC/universe, target, range). Prism
listens on all three protocols simultaneously and routes every message through the shared
resolver, so everything is consistent.

## Modulators vs. control

Control messages set a parameter value. **Modulators** (see **[Audio](audio.md#modulation)**)
continuously drive a parameter. Both use the same target paths, so a fader can set a base value
that an LFO modulates around, and the OSC/MIDI/DMX surface stays in sync.
