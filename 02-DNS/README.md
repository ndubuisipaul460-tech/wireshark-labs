# Lab 02: DNS

## Capture

1. Capture filter `port 53`.
2. In PowerShell:
   ```powershell
   ipconfig /flushdns
   Resolve-DnsName github.com
   Resolve-DnsName does-not-exist.invalid
   Resolve-DnsName github.com -Type MX
   ```

## Display filters

| Filter | Shows |
|--------|-------|
| `dns` | All DNS |
| `dns.flags.response == 0` | Queries only |
| `dns.flags.rcode == 3` | NXDOMAIN (name doesn't exist) |
| `dns.flags.rcode == 2` | SERVFAIL (server failed to answer) |
| `dns.time > 0.5` | Responses slower than 500 ms |
| `dns.qry.type == 15` | MX queries |
| `dns.qry.name contains "github"` | Queries for a name |
| `dns && !dns.response_in && dns.flags.response == 0` | Queries that never got an answer |

## What to look for

- Each query and response share a **transaction ID**; Wireshark links them (`dns.response_in`, `dns.time`).
- **Slow DNS** makes everything feel slow: check `dns.time`.
- **NXDOMAIN** for a name that should exist: typo, wrong DNS suffix, or the record is missing on that server.
- Long random-looking subdomains or very high query rates to one domain can indicate **DNS tunnelling** or malware using a **DGA** (domain generation algorithm).

## Questions

1. Which DNS server answered, and how long did it take?
2. What rcode did the `.invalid` name return?
3. What TTL came back for github.com's A record?

## My notes

- Date:
- Findings:
