# The Dark Moonlet

> **Category:** Forensics / Data / Astrobiology · **Points:** n/a · **Track:** [Asteroid missions](README.md)
>
> **Files:** `moonlet_sample.csv`, `procedural_blank.csv`, `standards.csv`, `isotopes.csv`

## Challenge

A lander's onboard GC-MS accidentally sampled dust from a companion moonlet. Among the detected amino acids, exactly **one** shows a genuine, non-terrestrial enantiomeric excess. The rest are either racemic or simple lab contamination. The task is to find that one molecule and quantify how strongly it leans toward one handedness, without being fooled by contamination that looks identical to a real signal at first glance.

## Approach

### 1. Blank subtraction

Every peak in the sample and in the procedural blank was matched to the standards table by retention time (±0.03 min tolerance). The blank's quant-ion area was then subtracted from the sample's at every matching retention time. This is exactly the *"prove it didn't come off our gloves"* test the notes ask for.

### 2. Enantiomeric excess

```
ee = (L − D) / (L + D) × 100
```

computed for every chiral pair on the blank-corrected areas:

| Compound | ee (corrected) | Verdict |
|---|---|---|
| Serine | 0.0 % | Pure contamination: identical blank peak, Earth-range isotopes (δ¹³C −22.7 ‰, δ¹⁵N +3.1 ‰) |
| Alanine | 0.5 % | Mixed / contaminated: D and L isotopes wildly inconsistent |
| Isovaline | 0.5 % | Racemic here, despite its real-world meteoritic fame |
| Valine | 0.6 % | Racemic |
| Glutamic acid | 0.8 % | Racemic |
| **Aspartic acid** | **34.1 %** | **No blank peak; D and L isotopes both strongly non-terrestrial and consistent (δ¹³C +27–28 ‰, δ¹⁵N +87–88 ‰)** |

### 3. Conclusion

Aspartic acid is the only compound with **both** a real excess and a clean, self-consistent non-terrestrial isotopic signature between its enantiomers. It is the genuine extraterrestrial detection.

## Flag

```
GAIANSPACE{aspartic_acid_L34}
```

---
[← Back to index](../../README.md)
