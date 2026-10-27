# Wireshark Labs

Packet analysis labs for the protocols every support and operations engineer troubleshoots, plus security filters for investigations. Each lab says how to generate the traffic on a Windows PC, which display filters to use, what to look for, and questions to answer from your own capture.

## Labs

| # | Lab | Focus |
|---|-----|-------|
| 1 | [TCP Handshake](01-TCP-Handshake/) | SYN/SYN-ACK/ACK, RST vs FIN, retransmissions, zero window, MSS |
| 2 | [DNS](02-DNS/) | Query/response timing, NXDOMAIN, SERVFAIL, unanswered queries, tunnelling signs |
| 3 | [DHCP](03-DHCP/) | DORA, lease options, relay (`giaddr`), NAKs, rogue DHCP servers |
| 4 | [ICMP](04-ICMP/) | Ping, traceroute TTL, unreachable codes, Path MTU black holes |
| 5 | [HTTP & TLS](05-HTTP-TLS/) | HTTP status codes, SNI, TLS versions and alerts, SSLKEYLOGFILE decryption |
| 6 | [Security Analysis](06-Security-Analysis/) | Scans, cleartext credentials, beaconing, file export |

## Tools

- Wireshark / `tshark`
- `tcpdump` for capturing on Linux servers (see [linux-admin-labs](https://github.com/ndubuisipaul460-tech/linux-admin-labs/blob/main/Networking/network-commands.md))
- PowerShell (`Test-NetConnection`, `Resolve-DnsName`, `ipconfig`) to generate traffic

## Useful `tshark` one-liners

```
tshark -r cap.pcapng -q -z conv,tcp                       # TCP conversations
tshark -r cap.pcapng -q -z expert                         # expert info summary
tshark -r cap.pcapng -Y "dns.flags.rcode == 3" -T fields -e dns.qry.name | sort | uniq -c | sort -rn
tshark -r cap.pcapng -Y "tls.handshake.type == 1" -T fields -e ip.dst -e tls.handshake.extensions_server_name
tshark -r cap.pcapng -Y "tcp.analysis.retransmission" -T fields -e ip.src -e ip.dst | sort | uniq -c
```

## Troubleshooting with captures

1. Capture as close to the problem as possible (client side **and** server side if you can).
2. Filter down to the one conversation (`tcp.stream eq N`).
3. Read the handshake first: did the connection even start?
4. Then timing (RTT, `dns.time`), then errors (RST, retransmissions, alerts, HTTP 5xx).
5. Put the evidence in the ticket: packet numbers, filter used, screenshot.

## Skills Demonstrated

Packet capture and analysis · TCP/IP troubleshooting · DNS, DHCP, ICMP and TLS · Network security investigation

## Related Repositories

- [network-engineer-labs](https://github.com/ndubuisipaul460-tech/network-engineer-labs)
- [it-operations-labs](https://github.com/ndubuisipaul460-tech/it-operations-labs)
- [linux-admin-labs](https://github.com/ndubuisipaul460-tech/linux-admin-labs)
