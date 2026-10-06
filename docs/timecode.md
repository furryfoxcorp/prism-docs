# Timecode

Timecode keeps multiple machines and devices in sync. Prism supports **MTC** (MIDI Time Code) and
**LTC** (Linear Timecode), can send and receive both, and can drive its timeline from incoming
timecode.

Enable it in the project panel's **Timecode** section.

## MTC (MIDI)

- **Send** — Prism creates a virtual CoreMIDI source called **"Prism Timecode"** and emits
  quarter-frame messages at the configured frame rate. Other apps/machines can connect to it.
- **Receive** — Prism parses MTC quarter-frame messages from any connected MIDI source. Because
  MTC is MIDI, it can cross machines over **RTP-MIDI / network MIDI sessions**.

## LTC (audio)

- **Generate** — encode timecode as a biphase-mark audio signal; export to a WAV file or send it
  out an audio device to drive other gear.
- **Decode** — recover timecode from an LTC audio recording (biphase-mark demodulation).

The codec is unit-tested: a round-trip of `01:23:45:15` decodes exactly.

## Follow mode

Enable **Follow Incoming Timecode** to slave Prism's transport to incoming MTC/LTC. An **offset**
shifts the sync, and the timeline scrubs to the received position so multiple Prism instances stay
locked.

## Frame rates & status

Supported rates: **24, 25, 30, 60**. The panel shows the current frame rate, live **status**, and
the **current timecode**.

!!! note "No hardware genlock"
    Apple silicon has no genlock / frame-lock (there is no Quadro Sync equivalent), so
    multi-machine synchronization is approximated via timecode. Frame-accurate lockstep across
    machines would need external hardware reference.
