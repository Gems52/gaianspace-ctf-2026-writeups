# Short Arc: Recovering 2041 QM23

> **Category:** Astro / Orbit determination · **Points:** n/a · **Track:** [Asteroid missions](README.md)
>
> **Files:** `2041QM23.obs` (MPC 80-column astrometry), `recovery_tracklets.csv`, `station_notes.md`

## Challenge

Fifteen astrometric positions of a fast, close near-Earth object from four stations over three nights must be turned into an orbit, then propagated 25 days forward to a recovery epoch, in order to pick the correct moving-object tracklet out of **44 look-alike candidates** imaged by Cerro Tololo (807).

- One station's clock is known to have drifted.
- The desk's own naive fit ("put it at the centre of the Earth") is explicitly flagged as wrong: the object passed closer to Earth than some satellites, so **topocentric parallax cannot be ignored**.

## Approach

### 1. A from-scratch astrodynamics pipeline

No `astropy`, `skyfield` or `spiceypy`, and no network access, so the entire pipeline was implemented from scratch in NumPy/SciPy:

- an exact byte-offset parser for the MPC 80-column format
- observer position vectors for each station (F51 Pan-STARRS1/Haleakala, G96 Mt. Lemmon, 703 Catalina, T08 ATLAS/Mauna Loa, 807 CTIO): geodetic → ECEF → GMST rotation → precession → J2000
- a truncated **VSOP87D** heliocentric Earth ephemeris, cross-validated against an independent JPL/Standish mean-elements model
- a classic angles-only **Gauss** preliminary orbit determination from three well-spaced, clock-trustworthy observations
- a universal-variable (Stumpff-function) two-body propagator with iterative light-time correction
- a 6-parameter nonlinear least-squares refinement (`scipy.optimize.least_squares`) of the full state vector against every reliable observation

### 2. The drifting clock

Station **703**'s reported timestamps were excluded from the fit (its field note about a timing-server outage was a direct hint). A post-hoc check found an optimal clock offset of only ≈ 10 s for that station, confirming the suspicion quantitatively.

### 3. The fit

The refined fit converged to a **0.18–0.24″ RMS** residual and a physically sensible Amor-type NEO orbit:

| Element | Value |
|---|---|
| Semi-major axis `a` | ≈ 1.06 AU |
| Eccentricity `e` | ≈ 0.107 |
| Inclination `i` | ≈ 24.2° |

This is consistent with a transient close Earth flyby during the discovery arc, matching the "closer than some of our satellites" remark.

### 4. The subtle bug

Propagating 25 days to the recovery epoch (2041-10-15, 04:10 and 04:40 UTC) and projecting topocentrically from Cerro Tololo first pointed at a plausible but **wrong** tracklet.

Debugging traced this to a subtle bug: the precession rotation matrix was applied in the **wrong direction** for the Sun/Earth ephemeris only. The error is invisible over the 3-day discovery arc, but it grows with the 41-year gap since the J2000 reference epoch and becomes significant over a 25-day extrapolation.

### 5. Identification

After fixing the rotation direction and swapping in the full VSOP87D series, the predicted position collapsed onto a single tracklet:

| Metric | Before fix | After fix |
|---|---|---|
| Position offset | 363″ | **14″** |
| Angular rate offset | n/a | **0.6″ per 30 min** |

An unambiguous identification. The same body is characterised spectrally in [110 · m-type](110-m-type.md).

## Flag

```
GAIANSPACE{RT39SBA}
```

---
[← Back to index](../../README.md)
