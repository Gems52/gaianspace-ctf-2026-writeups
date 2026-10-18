# 802 · the-static

> **Category:** Forensics / DVB-MPE · **Points:** 149 · **Track:** [Space comms](README.md)

## Challenge

A transport-stream capture (`eumetcast_grab.ts`) of a weather-satellite broadcast (EUMETCast) contains, per the team's notes, a "mailbox" encapsulated via **MPE** (Multi-Protocol Encapsulation) inside the ordinary video broadcast. It is not to be confused with the channel explicitly flagged "VIDEO" by a former analyst (Pell), which is a deliberate decoy.

## Clues used

- *"Each section of the mailbox ends in a seal (CRC)"* → corrupted sections must be dropped.
- *"The addresses come first, the useful sentence is what follows"* → the format is `SEG<nn>|<text>` right after an addressing header.
- *"The good pages were numbered by the crew in the order they were written, a later repeat is not a later page"* → deduplicate by segment number, keeping the most complete and consistent version of each of `SEG00` … `SEG03`.

## Solution

A binary search for the pattern `SEG\d\d\|` in the stream isolates, for each segment, the useful text fragment immediately following the marker, up to the end of a valid section:

| Segment | Fragment |
|---|---|
| `SEG00` | `GAIANSPA` |
| `SEG01` | `CE{mpe_d` |
| `SEG02` | `gram_r3a5` |
| `SEG03` | `ssembly}` |

Several competing occurrences exist in the capture for each segment. Keeping only the sections whose seal is valid, and discarding the visibly corrupted or self-labelled decoy repeats (for example a variant ending in `d_decoy}`, or one containing the trap word `fool` inside `...rc_f00l3`), the ordered concatenation `SEG00 + SEG01 + SEG02 + SEG03` reconstructs a coherent flag.

**Why those variants are dropped:** the team's memo explicitly warns that any section whose seal doesn't match the page was damaged by signal fade during the pass, and that such pages *"are garbage even when the words look familiar"*.

## Flag

```
GAIANSPACE{mpe_dgram_r3a5ssembly}
```

---
[← Back to index](../../README.md)
