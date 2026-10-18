# 301 · email-entrapment

> **Category:** Forensics · **Points:** 99 · **Track:** [Foundation One](README.md)

## Challenge

An engineer's workstation was frozen after a phishing hit, and a 14 KB memory dump (`mem.dmp`) was pulled before shutdown. The incident desk is fixated on a cached credential found in the noise, but the prompt is explicit: that is not the story. The real story is the inbound mail *"dressed as a partner's contract"* and the attachment it carried.

## Approach

Carving printable strings out of the dump surfaces two distinct regions:

- **A decoy.** A cached, expired credential next to a newsletter fragment and a `~tmp` path. This is the red herring the incident desk is chasing.
- **The real artifact.** A complete, raw RFC-822 email from `"Susan Calvin, Contracts" <contracts@asimov-aerospace-partners.com>`, subject *"Foundation One - signed NDA + revised schedule (action required)"*, with a base64 MIME attachment named `Invoice_FoundationOne.pdf`. The mismatch (NDA text, invoice filename) is a classic phishing tell.

Isolating the base64 block between the MIME headers and the closing boundary (`--=_gaian_7f2a_boundary--`) and decoding it yields a small, valid one-page PDF. Its content stream renders the flag directly:

```
BT /F1 16 Tf 24 60 Td (GAIANSPACE{ph1sh_in_the_dump}) Tj ET
```

## Flag

```
GAIANSPACE{ph1sh_in_the_dump}
```

---
[← Back to index](../../README.md)
