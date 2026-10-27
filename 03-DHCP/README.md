# Lab 03: DHCP

## Capture

1. Capture filter `port 67 or port 68`.
2. In an **elevated** PowerShell (you'll lose the network for a few seconds):
   ```powershell
   ipconfig /release
   ipconfig /renew
   ```

## Display filters

| Filter | Shows |
|--------|-------|
| `dhcp` | All DHCP (use `bootp` on Wireshark versions older than 3.0) |
| `dhcp.option.dhcp == 1` | Discover |
| `dhcp.option.dhcp == 2` | Offer |
| `dhcp.option.dhcp == 3` | Request |
| `dhcp.option.dhcp == 5` | ACK |
| `dhcp.option.dhcp == 6` | NAK (server refused the request) |
| `dhcp.ip.relay != 0.0.0.0` | Relayed through a router (`giaddr` set) |

## What to look for

- **DORA**: Discover → Offer → Request → ACK. A renew of an existing lease often shows only Request → ACK.
- In the **ACK**, check option 1 (subnet mask), 3 (router/gateway), 6 (DNS servers) and 51 (lease time).
- **Discover with no Offer**: the server isn't reachable (DHCP relay `ip helper-address` missing on that VLAN) or the pool is exhausted.
- **NAK**: the client asked for an address from another network (e.g. after moving between VLANs).
- **Two different servers offering**: a **rogue DHCP server** (e.g. a home router plugged into the office network).

## Questions

1. Which server IP gave you the lease, and for how long?
2. What gateway and DNS servers were in the ACK?

## My notes

- Date:
- Findings:
