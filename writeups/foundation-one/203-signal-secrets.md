# 203 · signal-secrets

> **Category:** Steganography · **Points:** 196 · **Track:** [Foundation One](README.md)

## Challenge

A mid-ocean listening array recorded an 18-second downlink that sounds like pure hiss. The challenge hints that the recording hides writing *"if you look at it instead of playing it"*, and that this visible text is not the flag itself but the instruction for unlocking a second, deeper layer carried *"under the noise floor"*.

## Approach

The WAV file is mono, 16-bit PCM at 16 kHz, 18 seconds long. Two passes are required, exactly as the prompt says ("work the recording in two passes").

### Pass 1: look, don't listen

Rendering the file as a spectrogram (`scipy.signal.spectrogram`, 1024-point STFT) reveals text drawn directly into the frequency domain around 1–3 kHz:

```
xor key = nimbus
```

### Pass 2: under the noise floor

The least-significant bit of every 16-bit sample, packed MSB-first into bytes, decodes to a 5-byte header (`GS03` plus a one-byte length field, `0x1e` = 30, matching the flag's character count) followed by the payload. XOR-ing that payload with the repeating key `nimbus` from pass 1 recovers the plaintext flag.

```python
# samples: the 16-bit PCM samples of the WAV file
u = samples.view(numpy.uint16)
lsb = u & 1                          # bit-plane 0
bytes_ = numpy.packbits(lsb)         # MSB-first packing
payload = bytes_[5:35]               # skip "GS03" + length byte
key = b"nimbus"
flag = bytes(c ^ key[i % len(key)] for i, c in enumerate(payload))
```

## Flag

```
GAIANSPACE{tw0_l4yer_d0wnlink}
```

---
[← Back to index](../../README.md)
