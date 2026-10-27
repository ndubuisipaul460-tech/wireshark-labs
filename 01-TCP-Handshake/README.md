# Lab 01: TCP Handshake and Connection Problems

## Capture

1. Start Wireshark on your active interface with the capture filter `tcp port 443`.
2. In PowerShell: `Test-NetConnection github.com -Port 443`, then `Test-NetConnection 127.0.0.1 -Port 3389` (a closed port).
3. Stop the capture.

## Display filters

| Filter | Shows |
|--------|-------|
| `tcp.flags.syn == 1 && tcp.flags.ack == 0` | Connection attempts (SYN) |
| `tcp.flags.syn == 1 && tcp.flags.ack == 1` | Server accepted (SYN-ACK) |
| `tcp.flags.reset == 1` | Resets: closed port or rejected connection |
| `tcp.flags.fin == 1` | Normal close |
| `tcp.analysis.retransmission` | Retransmitted segments (loss or a slow path) |
| `tcp.analysis.zero_window` | Receiver can't keep up |
| `tcp.analysis.flags` | Everything Wireshark's expert system flags for TCP |
| `tcp.stream eq 3` | One conversation only |

## What to look for

- **Healthy**: SYN → SYN-ACK → ACK, then data. The SYN to SYN-ACK delay ≈ network round-trip time (`tcp.analysis.initial_rtt`).
- **SYN, then RST**: the host is reachable, but nothing listens on that port (or a firewall rejects it).
- **SYN, SYN, SYN with no reply**: dropped silently, usually by a firewall or routing.
- **RST vs FIN**: FIN is a graceful close; RST is an abort.
- **MSS**: in the SYN options. A VPN or tunnel lowers the usable MTU, so a mismatch can make large transfers hang while small requests work.

## Statistics

- *Statistics → Conversations → TCP*: who talks to whom, bytes, duration.
- *Statistics → TCP Stream Graphs → Round Trip Time*: latency over time.
- *Analyze → Expert Information*: retransmissions, resets, zero windows in one list.

## Questions

1. What was the initial RTT to github.com?
2. Which packet answered the connection to the closed port, and what flags did it have?
3. How many retransmissions are in the capture?

## My notes

- Date:
- Findings:
