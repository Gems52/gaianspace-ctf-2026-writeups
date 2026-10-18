<div align="center">

# 🛰️ GaianSpace CTF 2026: Writeups

**Space-themed CTF · 22 challenges solved · 4 mission tracks**

![CTF](https://img.shields.io/badge/CTF-GaianSpace%202026-0b1020?style=for-the-badge&labelColor=0b1020&color=38bdf8)
![Team](https://img.shields.io/badge/Team-RUY-0b1020?style=for-the-badge&labelColor=0b1020&color=a78bfa)
![Solved](https://img.shields.io/badge/Solved-22-0b1020?style=for-the-badge&labelColor=0b1020&color=34d399)
![Language](https://img.shields.io/badge/Writeups-English-0b1020?style=for-the-badge&labelColor=0b1020&color=fbbf24)

Steganography · Forensics · Network · Crypto · Hardware · Reverse / Pwn · Radio · Remote sensing · Astrodynamics · Guidance

</div>

---

## About

Writeups for the challenges solved by **GEMS52** of team **RUY** (with **ftps3rver**) at **GaianSpace CTF 2026**, hosted on [ctf.gaian.space](https://ctf.gaian.space).

The challenges form connected storylines: a phishing intrusion against the *Foundation One* CanSat program, a Mars rotorcraft called *DR-ONE*, and a set of asteroid prospecting and landing missions. Every writeup documents the path that actually led to the flag: the artifact, the key insight, and the decisive command or script. Dead ends are not reproduced.

## Scoreboard at a glance

Click a challenge name to open its writeup.

### 🟦 Foundation One

*Theft, sabotage and exfiltration against the Foundation One CanSat program.* ([track overview](writeups/foundation-one/README.md))

| # | Challenge | Category | Pts | Flag |
|:-:|---|---|:-:|---|
| 203 | [signal-secrets](writeups/foundation-one/203-signal-secrets.md) | Stegano | 196 | `GAIANSPACE{tw0_l4yer_d0wnlink}` |
| 301 | [email-entrapment](writeups/foundation-one/301-email-entrapment.md) | Forensics | 99 | `GAIANSPACE{ph1sh_in_the_dump}` |
| 302 | [web-wormhole](writeups/foundation-one/302-web-wormhole.md) | Network / Forensics | 150 | `GAIANSPACE{dr1ve_by_st4ged}` |
| 303 | [gerber-grab-crack](writeups/foundation-one/303-gerber-grab-crack.md) | Forensics / Crypto | 199 | `GAIANSPACE{s1lk_scr33n_le4k}` |
| 304 | [pcb-puzzle-probe](writeups/foundation-one/304-pcb-puzzle-probe.md) | Hardware | 199 | `GAIANSPACE{pcb_qu1z_compl3te}` |
| 305 | [satellite-sabotage-sim](writeups/foundation-one/305-satellite-sabotage-sim.md) | Reverse / Pwn | 249 | `GAIANSPACE{c4nsat_t3lemetry_pwn3d}` |
| 306 | [lora-whisper](writeups/foundation-one/306-lora-whisper.md) | Radio / Forensics | 100 | `GAIANSPACE{l0ra_fr4gments_in_0rder}` |
| 307 | [descent-diary](writeups/foundation-one/307-descent-diary.md) | Forensics / Data | 141 | `GAIANSPACE{DRIFT_AND_FALL}` |
| 505 | [thermal-duty](writeups/foundation-one/505-thermal-duty.md) | Data / Hardware | 75 | `GAIANSPACE{therm4l_duty_cycle}` |
| 801 | [torn-frames](writeups/foundation-one/801-torn-frames.md) | Forensics | 99 | `GAIANSPACE{burst_r3align_sdt_r3ad}` |
| n/a | [Backdoor From Space](writeups/foundation-one/backdoor-from-space.md) | Forensics / Network | n/a | `GAIANSPACE{c2_rc4_from_the_nonce}` |

### 🟧 DR-ONE

*A small Mars rotorcraft scout performing a subsurface survey.* ([track overview](writeups/dr-one/README.md))

| # | Challenge | Category | Pts | Flag |
|:-:|---|---|:-:|---|
| 601 | [what-is-this-planet](writeups/dr-one/601-what-is-this-planet.md) | Network / Forensics | 99 | `GAIANSPACE{m4rs_r0torcraft_scout}` |
| 602 | [help-me-fly-55](writeups/dr-one/602-help-me-fly-55.md) | Network / Forensics | 198 | `GAIANSPACE{fl1ght_telemetry_str3am}` |
| 603 | [g-fold-56](writeups/dr-one/603-g-fold-56.md) | Guidance / Math | 199 | `GAIANSPACE{LZ_1370_-360}` |
| 604 | [ice-vault](writeups/dr-one/604-ice-vault.md) | Crypto / Brute-force | 125 | `GAIANSPACE{UTOPIA_PLANITIA_46.7N_97.3E}` |

### 🟪 Asteroid missions

*Prospecting, orbit recovery, landing and sampling around asteroid 2041 QM23 and its moonlet.* ([track overview](writeups/asteroid-missions/README.md))

| # | Challenge | Category | Pts | Flag |
|:-:|---|---|:-:|---|
| 110 | [m-type](writeups/asteroid-missions/110-m-type.md) | Astro / Spectroscopy | 100 | `GAIANSPACE{2041QM23_120}` |
| n/a | [Short Arc](writeups/asteroid-missions/short-arc.md) | Astro / Orbit determination | n/a | `GAIANSPACE{RT39SBA}` |
| n/a | [Touchdown](writeups/asteroid-missions/touchdown.md) | Data / Guidance | n/a | `GAIANSPACE{A-0242}` |
| n/a | [The Dark Moonlet](writeups/asteroid-missions/dark-moonlet.md) | Forensics / Data | n/a | `GAIANSPACE{aspartic_acid_L34}` |
| n/a | [Two Clocks](writeups/asteroid-missions/two-clocks.md) | Forensics / Embedded | n/a | `GAIANSPACE{0412377}` |

### 🟩 Space comms

*Satellite broadcast and LoRa link forensics.* ([track overview](writeups/space-comms/README.md))

| # | Challenge | Category | Pts | Flag |
|:-:|---|---|:-:|---|
| 121 | [channel-hop](writeups/space-comms/121-channel-hop.md) | Remote sensing / LoRa | 200 | `GAIANSPACE{AL-STAMP-Q9VRR9}` |
| 802 | [the-static](writeups/space-comms/802-the-static.md) | Forensics / DVB-MPE | 149 | `GAIANSPACE{mpe_dgram_r3a5ssembly}` |

## Techniques covered

| Domain | Highlights |
|---|---|
| **Steganography** | Spectrogram text, LSB bit-plane extraction, XOR keys |
| **Forensics** | Memory-dump carving, FAT12 recovery, MPEG-TS / MPE reassembly, padded USB captures |
| **Network** | TCP reassembly, HTTP drive-by analysis, RTSP/SDP, UDP sequence reordering, RC4 C2 decryption |
| **Crypto** | AES-128-CBC with a weak KDF, 7-Zip mask attack, XOR keystreams |
| **Hardware** | Gerber / PCB review, thermal-model inversion with a known-plaintext prefix |
| **Reverse / Pwn** | Unbounded-write firmware bug, offline LFSR de-whitening |
| **Radio** | LoRa frame reassembly, time-on-air and two-way time transfer |
| **Space & science** | Sentinel-2 channel analysis, spectroscopy, orbit determination, lander guidance, GC-MS chirality |

## Repository layout

```
.
├── README.md
├── assets/                      # figures used in the writeups
└── writeups/
    ├── foundation-one/          # phishing → exfiltration → firmware sabotage
    ├── dr-one/                  # Mars rotorcraft track
    ├── asteroid-missions/       # 2041 QM23 prospecting, landing, sampling
    └── space-comms/             # broadcast and LoRa forensics
```

## Credits

- **GEMS52**: author of the writeups, solver of the challenges listed here
- **ftps3rver**: teammate (team RUY)
- Challenges and platform: [GaianSpace CTF 2026](https://ctf.gaian.space)

## License

Released under the [MIT License](LICENSE). Challenge names, files and flags belong to their respective authors and organizers.
