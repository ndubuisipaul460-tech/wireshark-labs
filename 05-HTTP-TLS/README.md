# Lab 05: HTTP and TLS

## Capture

1. Capture filter `tcp port 80 or tcp port 443`.
2. Visit `http://neverssl.com` (plain HTTP) and `https://github.com` in a browser.

## Display filters

| Filter | Shows |
|--------|-------|
| `http.request` | HTTP requests (plain text only) |
| `http.response.code >= 400` | Client and server errors |
| `tls.handshake.type == 1` | Client Hello |
| `tls.handshake.type == 2` | Server Hello |
| `tls.handshake.extensions_server_name` | SNI: the site name the client asked for |
| `tls.record.version == 0x0301` | Records using the TLS 1.0 version field (legacy) |
| `tls.alert_message` | TLS alerts (handshake failures, bad certificate) |

## What to look for

- **HTTP** is readable: method, host, path, status code, headers. *Follow → HTTP Stream* to see the whole exchange.
- **HTTPS**: after the handshake, everything is encrypted. You still see the **SNI** in the Client Hello, which tells you which site was requested.
- **Handshake failure alerts** point to protocol or cipher mismatches, or certificate problems (expired, wrong name, untrusted CA).
- The time from Client Hello to Server Hello shows server or network latency.

## Decrypting your own browser traffic (lab only)

1. Set a user environment variable `SSLKEYLOGFILE=C:\labs\tls-keys.log` and restart the browser.
2. Wireshark: *Edit → Preferences → Protocols → TLS → (Pre)-Master-Secret log filename* → that file.
3. HTTP/2 inside TLS becomes readable. Delete the key file afterwards.

## Questions

1. What TLS version did github.com negotiate?
2. Which SNI names did your browser request in the first 10 seconds?

## My notes

- Date:
- Findings:
