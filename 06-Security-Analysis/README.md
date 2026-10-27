# Lab 06: Security Analysis Filters

Use these when investigating suspicious traffic. In real incidents: preserve the capture, don't alert the suspected user, and hand over to the security team (SOC) with the evidence.

## Scans

| Filter | Indicates |
|--------|-----------|
| `tcp.flags.syn == 1 && tcp.flags.ack == 0 && tcp.window_size <= 1024` | SYN scan (nmap default often uses small windows) |
| `tcp.flags == 0x000` | NULL scan |
| `tcp.flags == 0x029` | Xmas scan (FIN, PSH, URG) |
| `icmp.type == 8 && !icmp.resp_in` | Ping sweep to hosts that don't answer |

*Statistics → Conversations → IPv4*, sorted by number of destinations: one source talking to many hosts or ports in seconds is a scan.

## Cleartext credentials

| Filter | Shows |
|--------|-------|
| `http.authorization` | HTTP Basic auth headers (base64, easily decoded) |
| `ftp.request.command == "USER" \|\| ftp.request.command == "PASS"` | FTP logins |
| `telnet` | Telnet sessions (everything in clear text) |
| `pop.request.command == "PASS"` | POP3 passwords |

## Beaconing and exfiltration

- Malware often calls home at **regular intervals**: *Statistics → Conversations*, then look for one external IP with many small, evenly spaced connections. *Statistics → I/O Graphs* with a filter for that IP shows the rhythm.
- `dns.qry.name.len > 50`: very long DNS names (possible DNS tunnelling).
- Large uploads to unknown external IPs: *Conversations*, sorted by **Bytes A→B**.

## File extraction

*File → Export Objects → HTTP / SMB*: lists files transferred in clear text, which you can save for analysis (in an isolated environment only).

## Questions

1. In your own capture, which external IPs had the most connections? Do you recognise them?
2. Is any traffic in your capture sent in clear text that shouldn't be?

## My notes

- Date:
- Findings:
