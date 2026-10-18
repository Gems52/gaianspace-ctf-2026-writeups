# 303 · gerber-grab-crack

> **Category:** Forensics / Crypto · **Points:** 199 · **Track:** [Foundation One](README.md)

## Challenge

The intruder didn't just browse the drive. They packed up the Foundation One board files, encrypted them, and shipped them off the network. Two artifacts were recovered:

- the staging disk the intruder worked from (`attacker_disk.img`)
- the corporate capture of the outbound upload (`exfil_upload.pcap`)

## Approach

### 1. The disk

A FAT12 image (label `EXFIL`) contains the intruder's own encryption tool, `ENCRYPTO.PY`:

- AES-128-CBC
- key = `sha256(passphrase)[:16]`
- IV prefixed to the ciphertext

It is a deliberately weak, home-rolled scheme. The image also holds a **deleted** earlier version of the tool with an old passphrase. A job log confirms that the current job is `foundation_one`, so the deleted tool's passphrase is a decoy. The live tool's passphrase, `g3rb3r-h31st-2026`, is the one that matters.

### 2. The capture

The upload to `cdn-sat-sync.net:8080` arrives in two chunks sent out of order, each tagged with an explicit `X-Upload-Offset` header. Reassembling them in offset order and verifying against the server's own SHA-256 status response confirms a clean 5952-byte base64 blob.

### 3. Decryption

Base64-decoding the blob and running AES-128-CBC with the key derived from the live passphrase unpacks cleanly (valid PKCS7 padding) into a ZIP archive containing four Gerber layers of the stolen `foundation_one` board.

```python
# Decryption sketch, following the scheme above
import base64, hashlib
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad

blob = base64.b64decode(upload_body)
key = hashlib.sha256(b"g3rb3r-h31st-2026").digest()[:16]
iv, ct = blob[:16], blob[16:]
zip_bytes = unpad(AES.new(key, AES.MODE_CBC, iv).decrypt(ct), 16)
```

The fabricator's note lives as a `G04` comment inside the silkscreen layer:

```
G04 FAB-NOTE foundation_one rev C : GAIANSPACE{s1lk_scr33n_le4k} do-not-pop
```

The same Gerber set is reused in [304 · pcb-puzzle-probe](304-pcb-puzzle-probe.md).

## Flag

```
GAIANSPACE{s1lk_scr33n_le4k}
```

---
[← Back to index](../../README.md)
