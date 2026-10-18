# Touchdown: AO-111 Descent Card

> **Category:** Data / Guidance / Astro · **Points:** n/a · **Track:** [Asteroid missions](README.md)
>
> **Files:** `hover_photometry.csv`, `descent_card.md`

## Challenge

A lander must be released from a **500 m hover station**, straight down the descent axis with no further control, to touch down within **2.5°** of a bright metal patch on a tumbling, elongated asteroid. The patch must face the descent axis **exactly at contact**, not at release.

One of two fixed-speed springs must be chosen, and one 10-second release slot out of 540 must be picked, using only a two-day hover-camera lightcurve.

## Approach

### 1. Period trap

A Lomb-Scargle periodogram of the lightcurve gives a dominant peak at **48.74 min**. For an elongated body, however, this is only **half** the true rotation period (two near-identical brightness humps per rotation).

Folding the data at that period and comparing alternating half-cycles reveals a clear, non-random asymmetry (up to 0.07 mag) caused by the metal patch sitting on only one of the two broad faces. A 2-harmonic least-squares fit over the full baseline gives the true period:

```
P = 5848.836 s  (97.48 min)
```

### 2. Spring choice

Released straight down with zero tangential velocity (the hover station station-keeps rather than orbits), the fall is purely radial under point-mass gravity. Energy conservation gives the contact speed for each spring:

| Spring | v₀ | Contact speed (50 m) | Verdict |
|---|---|---|---|
| A | 0.020 m/s | 0.0897 m/s | OK (< 0.110 m/s) |
| B | 0.080 m/s | 0.1185 m/s | Bounces, rejected |

**Spring A** is the only valid choice. Numerically integrating the radial fall from 500 m to 50 m gives `T_fall = 13 690.83 s` (3.803 h).

### 3. Phase trap

*"It's under us when it lands, not when it leaves."* The release slot must be chosen so that the patch faces the descent axis at contact, i.e. `release_time + T_fall` must land on the patch-facing phase.

The global brightness maximum of the fitted model (patch facing the camera) gives a reference epoch. Solving

```
release_time ≡ patch_epoch − T_fall   (mod true period)
```

for the unique solution inside the 2041-11-04 16:00:00–17:29:50 UTC slot window gives **16:40:20 UTC = slot 0242**. That is only 1.25 s from the ideal instant, safely inside the ±2.5° (±40.6 s) tolerance.

## Flag

```
GAIANSPACE{A-0242}
```

---
[← Back to index](../../README.md)
