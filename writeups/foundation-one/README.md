# Foundation One

An intruder phishes an engineer at GaianSpace, steals the Foundation One CanSat board files, pivots to the flight firmware, and finally sabotages the satellite's own descent and ground-link telemetry. Each writeup reconstructs one stage of that chain from the artifact provided, using standard stegano, forensics, radio and reverse-engineering technique.

## Suggested reading order

| # | Challenge | Stage | Category | Pts |
|:-:|---|---|---|:-:|
| 301 | [email-entrapment](301-email-entrapment.md) | Phishing mail in a memory dump | Forensics | 99 |
| 302 | [web-wormhole](302-web-wormhole.md) | Drive-by "secure portal" | Network / Forensics | 150 |
| n/a | [Backdoor From Space](backdoor-from-space.md) | C2 session to `cdn-sat-sync.net` | Forensics / Network | n/a |
| 303 | [gerber-grab-crack](303-gerber-grab-crack.md) | Board files encrypted and exfiltrated | Forensics / Crypto | 199 |
| 304 | [pcb-puzzle-probe](304-pcb-puzzle-probe.md) | Reviewing the stolen Gerber set | Hardware | 199 |
| 305 | [satellite-sabotage-sim](305-satellite-sabotage-sim.md) | Attacking the flight firmware | Reverse / Pwn | 249 |
| 306 | [lora-whisper](306-lora-whisper.md) | Descent telemetry over LoRa | Radio / Forensics | 100 |
| 307 | [descent-diary](307-descent-diary.md) | The CanSat's sensor diary | Forensics / Data | 141 |
| 203 | [signal-secrets](203-signal-secrets.md) | Hidden layer in the downlink | Stegano | 196 |
| 505 | [thermal-duty](505-thermal-duty.md) | PA case thermal log | Data / Hardware | 75 |
| 801 | [torn-frames](801-torn-frames.md) | Derelict relay capture | Forensics | 99 |

[← Back to index](../../README.md)
