# 304 · pcb-puzzle-probe

> **Category:** Hardware · **Points:** 199 · **Track:** [Foundation One](README.md)

## Challenge

A TCP "review desk" (`server.py`) asks five questions about the recovered Gerber set, in random order, each checked against a stored SHA-256 before the next is revealed. Only once all five are correct is the flag unsealed, using a key derived from the answers themselves, so the source code alone gives no shortcut.

## Approach

Opening the four Gerber layers recovered in [303](303-gerber-grab-crack.md) and reading them as a PCB reviewer would answers every question directly from the board geometry and silkscreen text:

| Question | Answer | Evidence |
|---|---|---|
| Number of copper layers | `2` | `F_Cu` + `B_Cu` |
| Longer board edge (mm) | `50` | `Edge_Cuts`: 50 × 40 mm rectangle |
| Designator of the 4-pad sensor | `U2` | 4-pad footprint with silkscreen label |
| Exposed debug header label | `SWD` | Distinct from the decoy label `J1` |
| Pad count of `U1` | `6` | 2×3 pad array next to the `U1` label |

Hashing each normalized answer matches the five stored SHA-256 digests in `server.py`.

The flag is then unsealed **locally**, by replaying the server's own key-derivation and XOR-keystream logic against the encrypted blob, using the answers in the fixed question order the server expects:

- key = `sha256("a1|a2|a3|a4|a5")`
- keystream = repeated SHA-256 over a counter

## Flag

```
GAIANSPACE{pcb_qu1z_compl3te}
```

---
[← Back to index](../../README.md)
