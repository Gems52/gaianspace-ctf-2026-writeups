# 602 · help-me-fly-55

> **Category:** Network / Forensics · **Points:** 198 · **Track:** [DR-ONE](README.md)

## Challenge

A second capture (`mars_control.pcap`) holds one flight's worth of the control channel between the pilot and DR-ONE: stick commands going up, acknowledgements coming back. The task is to isolate only the aircraft's replies, pull out the one field in them that changes while flying (ignored by the pilot, logged quietly by maintenance), and reassemble it **in the order it was actually sent**.

## Approach

The capture is a single UDP conversation between the ground station (port `54700`) and the aircraft (port `7099`). Ground-to-aircraft packets are fixed stick commands or 2-byte probes. Aircraft-to-ground packets are 5 bytes long.

- 138 of the 199 aircraft replies are an identical 5-byte acknowledgement (`53 01 00 00 00`), the "dull, parked-unit" answer the brief warns about.
- The remaining 61 replies carry a changing byte at a fixed offset. This is the live telemetry field to extract.

Those 61 packets split into two sub-types by a type byte:

| Type byte | Meaning |
|---|---|
| `0x02` | Decoy: meaningless repeated characters |
| `0x01` | Real payload: one character plus an explicit little-endian sequence number |

### Reply format

```
53 | type | char | seq_lo | seq_hi        (5 bytes, aircraft 7099 -> ground 54700)
```

Filtering on `type == 0x01` and dropping the plain `53 01 00 00 00` heartbeat leaves exactly the data-bearing packets. Sequence numbers run from 240 to 274 (35 distinct values), with duplicates from retransmission and out-of-order arrival. De-duplicating by sequence number and sorting ascending recovers the correct order.

## Solution

```python
import struct

d = open("mars_control.pcap", "rb").read()
o = 24
aircraft = []

while o < len(d):
    ts, tu, incl_len, orig_len = struct.unpack("<IIII", d[o:o + 16])
    o += 16
    p = d[o:o + incl_len]
    o += incl_len
    if p[12:14] != b"\x08\x00":              # IPv4 only
        continue
    src_port, dst_port = struct.unpack(">HH", p[34:38])
    if src_port == 7099:                     # aircraft -> ground
        aircraft.append(p[42:])              # UDP payload

# keep only type == 0x01 data packets, drop the heartbeat
t1 = [a for a in aircraft if a[1] == 1 and a != bytes.fromhex("5301000000")]

# dedupe by sequence number, then sort ascending
seq_to_char = {a[3] | (a[4] << 8): a[2] for a in t1}
flag = "".join(chr(seq_to_char[k]) for k in sorted(seq_to_char))
print(flag)   # GAIANSPACE{fl1ght_telemetry_str3am}
```

Taking the packets in raw arrival order instead produces garbled text (for example starting `AGNIASPAE{Cfl1gh...`), which is exactly the mistake the challenge is designed to catch. The sequence field, not arrival order, is authoritative.

## Flag

```
GAIANSPACE{fl1ght_telemetry_str3am}
```

---
[← Back to index](../../README.md)
