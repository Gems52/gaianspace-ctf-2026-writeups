# 801 · torn-frames

> **Category:** Forensics · **Points:** 99 · **Track:** [Foundation One](README.md)

## Challenge

A derelict relay, dark for forty years, is suddenly transmitting again. A tuner-stick capture (`halcyon_dump.bin`) was taken in a hurry. The operator's notes warn that the *"USB box pads whatever it feels like padding"*, and that reading the file as one smooth take *"will lie to you"*. The task: recover the name the old operator signed into the broadcast itself.

## Approach

Byte-level inspection shows that roughly **72 %** of the dump is the single byte `0xFF`, classic USB capture-overrun padding inserted in long, meaningless runs whenever the real stream stalled. Several other leads in the notes (a PAL/625 rack plate, a weather-fax theory, a stale retune frequency) are all flagged by the operator herself as dead ends belonging to a different bird.

Stripping every `0xFF` byte and concatenating what remains, without trying to preserve any timing across the gaps (exactly as instructed: *"wait until the beat comes back by itself"*), reconstructs a clean byte stream. Scanning it for printable text immediately surfaces a **UTF-16LE** string (one NUL byte between each character), repeated several times across the capture:

```
HALCYON Deep Relay ... GAIANSPACE{burst_r3align_sdt_r3ad}
```

`HALCYON Deep Relay` is the operator's signature the bridge asked for, and the flag follows it directly in the same decoded string.

## Flag

```
GAIANSPACE{burst_r3align_sdt_r3ad}
```

---
[← Back to index](../../README.md)
