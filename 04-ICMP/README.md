# Lab 04: ICMP, Ping and Traceroute

## Capture

1. Capture filter `icmp`.
2. In PowerShell:
   ```powershell
   ping 8.8.8.8 -n 4
   tracert -d 8.8.8.8
   ping 8.8.8.8 -f -l 1472     # 1472 + 28 header bytes = 1500, Don't Fragment set
   ping 8.8.8.8 -f -l 1500     # too big: "Packet needs to be fragmented but DF set"
   ```

## Display filters

| Filter | Shows |
|--------|-------|
| `icmp.type == 8` | Echo request |
| `icmp.type == 0` | Echo reply |
| `icmp.type == 11` | Time exceeded (traceroute hops) |
| `icmp.type == 3` | Destination unreachable |
| `icmp.type == 3 && icmp.code == 4` | Fragmentation needed (Path MTU Discovery) |
| `icmp.resp_not_found` | Requests with no reply |

## What to look for

- **Traceroute** works by sending packets with TTL 1, 2, 3… Each router that drops one at TTL 0 sends **Time exceeded**, revealing itself. `* * *` at a hop usually means that router doesn't answer ICMP, not that the path is broken.
- **Destination unreachable** codes: 0 net, 1 host, 3 port, 4 fragmentation needed, 13 administratively prohibited (firewall).
- **PMTUD black hole**: large packets are dropped and the "fragmentation needed" message is blocked by a firewall, so small pings work while large transfers or some web pages hang. Common over VPNs.

## Questions

1. How many hops to 8.8.8.8? Which hops didn't respond?
2. What is the largest `-l` value that worked with `-f` on your connection?

## My notes

- Date:
- Findings:
