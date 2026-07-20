# plivo-systems--CE24B077-
# Reliable Low-Latency UDP Streaming System

## Overview

This project implements a reliable real-time media streaming system over an unreliable UDP network. The goal is to transmit 160-byte audio frames generated every 20 milliseconds while minimizing playout delay and maintaining smooth playback despite packet loss, network jitter, packet duplication, and packet reordering.

The solution consists of two programs:

- **Sender** – Receives frames from the harness source and forwards them to the relay.
- **Receiver** – Receives packets from the relay, buffers them, and forwards them to the harness player.

---

## Architecture

```
Harness Source
      |
      | UDP (47010)
      |
   Sender
      |
      | UDP (47001)
      |
    Relay
      |
      | UDP (47002)
      |
   Receiver
      |
      | UDP (47020)
      |
 Harness Player
```

---

## Features

- UDP-based communication
- Sequence number handling
- Receiver-side packet buffering
- Duplicate packet filtering
- Fixed 20 ms playback scheduling
- Low-latency architecture

---

## Project Structure

```
.
├── sender.c
├── receiver.c
├── Makefile
├── SUMMARY.html
├── NOTES.md
├── RUNLOG.md
└── README.md
```

---

## Build

Compile the project using:

```bash
make
```

This generates:

```
./sender
./receiver
```

---

## Running

Example:

```bash
python3 run.py --profile profiles/A.json --delay_ms 60
```

---

## Packet Format

Each packet contains:

| Field | Size |
|------|------|
| Sequence Number | 4 Bytes |
| Payload | 160 Bytes |

---

## Design Overview

### Sender

- Receives frames from the harness.
- Packages the frame.
- Sends packets using UDP to the relay.

### Receiver

- Receives packets from the relay.
- Stores packets by sequence number.
- Removes duplicate packets.
- Delivers packets to the player at fixed intervals.
- Replaces unavailable packets with silence during playback (if implemented).

---

## Design Goals

- Low latency
- Reliable playback
- Ordered delivery
- Efficient bandwidth usage
- Continuous playout

---

## Technologies Used

- C
- POSIX UDP Sockets
- Linux / WSL
- Make
- Python Harness

---

## Files Submitted

- sender.c
- receiver.c
- Makefile
- SUMMARY.html
- RUNLOG.md
- NOTES.md
- README.md

---

## Notes

This project was developed for a systems programming assignment involving real-time communication over an unreliable UDP network. The implementation focuses on balancing playback latency and reliability while operating within the specified bandwidth constraints.
