# 306 · lora-whisper

> **Category:** Radio / Forensics · **Points:** 100 · **Track:** [Foundation One](README.md)

## Challenge

A ground radio log recorded every LoRa frame heard during the CanSat's descent, from two transmitters at once. Frames carry a per-sender sequence number and arrive out of order. One node is the CanSat, the other an unrelated, noisier weather beacon planted as a distraction.

## Approach

The operator's notes identify the CanSat as node `0x1F`. The loud, "obviously important" node `0x2A` is explicitly the decoy. Filtering the log to `addr=0x1F`, sorting by each frame's own sequence number and concatenating the payloads reconstructs the transmitted byte stream.

The notes also state that the modem never de-scrambles the "sync byte" `0x12` for the listener, so a single XOR of every byte with `0x12` recovers the plaintext:

```python
frames = [f for f in frames if f.addr == 0x1F]
frames.sort(key=lambda f: f.seq)
data = b"".join(f.payload for f in frames)
flag = bytes(b ^ 0x12 for b in data)
```

## Flag

```
GAIANSPACE{l0ra_fr4gments_in_0rder}
```

---
[← Back to index](../../README.md)
