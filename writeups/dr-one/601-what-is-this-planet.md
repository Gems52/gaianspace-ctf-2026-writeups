# 601 · what-is-this-planet

> **Category:** Network / Forensics · **Points:** 99 · **Track:** [DR-ONE](README.md)

## Challenge

A ground relay tap of DR-ONE's downlink was captured (`mars_downlink.pcap`). The rotorcraft streams video back to a lander, and a design sketch of its onboard perception stack was also recovered (`perception_stack.drawio`). The goal is to identify the platform purely from its own downlink traffic. The stream *"introduces itself every time a client asks to watch"*.

## Approach

The pcap is a classic RTSP/RTP video capture: a client performs `DESCRIBE` / `SETUP` / `PLAY` against an RTSP server at `10.42.0.1:7070`, with two mount points advertised (`depthcam` and `scoutcam`).

An RTSP `DESCRIBE` response carries an **SDP** (Session Description Protocol) body, and SDP sessions describe themselves through the `s=` (session name) and `i=` (session information) fields. That is exactly the self-introduction the challenge hints at. RTSP carries no encryption in this capture, so the SDP body can be read in clear text.

### Key findings

- The `depthcam` stream only carries a 320×240 stereo disparity feed for obstacle avoidance. It is a decoy with no identifying information.
- The `scoutcam` `DESCRIBE` response (`Content-Type: application/sdp`) contains both `s=` and `i=`, which together name the platform.
- The `i=` line reads *"DR-ONE Mars rotorcraft scout - subsurface survey downlink"*, confirming a Mars rotorcraft rather than the hobbyist quadcopter one analyst had assumed.

## Solution

A single string search around the RTSP response headers is enough:

```console
$ strings -n 3 mars_downlink.pcap | grep -n -B8 -A6 'GAIANSPACE'
```

```
RTSP/1.0 200 OK
CSeq: 3
Content-Base: rtsp://10.42.0.1:7070/scoutcam/
Content-Type: application/sdp
Content-Length: 236

v=0
o=- 1 1 IN IP4 10.42.0.1
s=GAIANSPACE{m4rs_r0torcraft_scout}
i=DR-ONE Mars rotorcraft scout - subsurface survey downlink
a=type:broadcast
t=0 0
c=IN IP4 0.0.0.0
m=video 0 RTP/AVP 96
a=rtpmap:96 H264/90000
```

No decoding or decryption was required once the RTSP/SDP structure was recognised.

## Flag

```
GAIANSPACE{m4rs_r0torcraft_scout}
```

---
[← Back to index](../../README.md)
