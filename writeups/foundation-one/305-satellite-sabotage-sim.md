# 305 · satellite-sabotage-sim

> **Category:** Reverse / Pwn · **Points:** 249 · **Track:** [Foundation One](README.md)

## Challenge

With the board understood, the intruder's last target was the flight computer itself: a bench emulator (`foundation_one_fw.py`) speaks the same command link as the real CanSat. The question: could an attacker on that link make the satellite reveal something it was never meant to say?

## Approach

### The bug

The firmware exposes `AUTH <hex4>`, which compares the given bytes against a random 4-byte service key stored at `mem[16:20]`.

Separately, `CFG <hexbytes>` writes its payload directly into `mem[0:n]` with **no bound check**, unlike `CAL`, which is correctly capped at 8 bytes. A 20-byte `CFG` payload therefore overwrites the service key region with an attacker-chosen value.

### The seed

`DUMP` whitens the hidden service page with a keystream seeded by `crc32(tlm()) & 0xFFFF`. Since `tlm()` always returns the same static telemetry frame, this seed is constant and can be computed completely offline.

### Exploit chain

```
CFG 0000000000000000000000000000000000000000   # zero out mem[16:20] (service key)
AUTH 00000000                                   # -> MAINT OK
DUMP                                            # -> whitened service page
# offline: seed = crc32(TLM_FRAME) & 0xFFFF = 0xA45F
# de-whiten SVC_PAGE with that seed using the firmware's own LFSR
```

The de-whitened service page reads:

```
SVC TLM imu=A3387 co2=A5190 radio=A4075 payload=GAIANSPACE{c4nsat_t3lemetry_pwn3d}
```

The full chain was validated end to end by importing the firmware's own `Sat` class and replaying `cfg → auth → dump` against a freshly randomized service key.

## Flag

```
GAIANSPACE{c4nsat_t3lemetry_pwn3d}
```

---
[← Back to index](../../README.md)
