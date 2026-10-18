# Two Clocks

> **Category:** Forensics / Embedded sensors · **Points:** n/a · **Track:** [Asteroid missions](README.md)
>
> **Files:** `lander_events.log`, `mothership_tt.log`, `hover_frames.csv`, `tt_icd.md`

## Challenge

The lander's own event log timestamps an anchor-firing shock event in its local `ticks_ms`, during a window when its radio was deliberately silent. The mothership's hover camera recorded 20 frames per second, timestamped in its own UTC. The two clocks are linked only through a **two-way LoRa time-transfer protocol**. The exact frame that opened right after the shock must be named, using the mothership's UTC frame index.

## Approach

### 1. Symmetry trap

The naive fix is to split each ping's round-trip time in half, assuming equal uplink and downlink delay. That is wrong here: the uplink (ping) uses **LoRa SF7**, but the downlink (reply) had been raised from **SF10 to SF11** after touchdown for link margin. A config-log line dated before every logged exchange says so.

### 2. Exact time-on-air

Time-on-air for each leg was computed from the Semtech SX1276 formula (AN1200.13):

| Leg | Settings | ToA |
|---|---|---|
| Ping | SF7, 5 B | 30.976 ms |
| Reply | SF11, 14 B, low-data-rate optimisation on | 659.456 ms |

That is a ~21× asymmetry that naive halving would have completely missed.

### 3. Clock model

Each of the 128 logged ping/reply exchanges yields two calibration points (ping-received instant and reply-sent instant, in both `ticks_ms` and UTC). A linear clock model (rate + offset) was fit across all of them.

### 4. Residual bias

The residuals showed an exact, opposite-sign **7.13 ms** bias between the two legs: a fixed radio-side delay not captured by the pure time-on-air formula (for example TX ramp-up). Adding it as a third free parameter in the regression dropped the residual RMS from ~7 ms to **0.46 ms**.

### 5. The silent window

The anchor-bite event falls inside a **75.748 s** radio-silence gap between two logged exchanges, exactly matching the logged "radio quiet" duration. It therefore had to be extrapolated from the calibrated model rather than bracketed directly:

```
2041-11-04T16:47:18.720311Z
```

### 6. Frame lookup

That instant sits almost exactly between two consecutive, gap-free camera frames (24.9 ms before one, 25.1 ms after the other). Frame **0412377** is the first whose exposure began after it.

## Flag

```
GAIANSPACE{0412377}
```

---
[← Back to index](../../README.md)
