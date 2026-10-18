# Backdoor From Space

> **Category:** Forensics / Networking · **Points:** n/a · **Track:** [Foundation One](README.md)
>
> **Files:** `ground_c2_traffic.pcap`

## Challenge

A span-monitor capture of a compromised array's network traffic, taken just after a downlink. Somewhere in the capture, one conversation is a live command-and-control (C2) session: the attacker sends a "shopping list" and gets told what to do next, with both sides agreeing on an encryption scheme earlier in the same exchange. Several decoy flows exist specifically to distract from the real one.

## Approach

1. **Custom parser.** No `tshark` or `scapy` was available, so a minimal pure-Python pcap parser was written from scratch: global header → per-packet records → Ethernet/IP/TCP/UDP reconstruction, with full TCP stream reassembly by sequence number.
2. **Triage of the decoys.** Dumping every reconstructed flow revealed several red herrings:
   - a "nominal" telemetry heartbeat
   - a syslog line carrying a fake `GAIANSPACE{nominal_pass}` token
   - a range-link "sample card"
   - a sync blob that publishes its own AES key in its headers
3. **The real flow.** The attacker's conversation goes to `203.0.113.77` (`cdn-sat-sync.net`, source port `49231`). Its request is a base64 blob decoding to:

   ```
   host=GS-VELA;exfil=foundation_one_gerber.zip;size=48213
   ```

   and it carries the header `X-Relay-Session: 9f3a7c2e15b84d60`, negotiated in the clear in the same request.
4. **Decrypting the tasking.** The response is another base64 blob. Plain XOR with the session id produced garbage. **RC4 keyed with the session id** (as raw ASCII bytes) decrypts it cleanly:

   ```
   TASKING v2
   target=GAIAN-13
   action=persist;rotate-keys
   note=next move queued
   flag=GAIANSPACE{c2_rc4_from_the_nonce}
   ```

5. **Sanity check.** The same RC4-with-session-id method re-applied to the decoy telemetry flow also decrypts into a coherent plaintext (`ack; no retask... window=closed...`), confirming the scheme and ruling out a coincidence.

## Flag

```
GAIANSPACE{c2_rc4_from_the_nonce}
```

---
[← Back to index](../../README.md)
