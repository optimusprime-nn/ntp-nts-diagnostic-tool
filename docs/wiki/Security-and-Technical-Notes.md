# Security and Technical Notes

## Protocols

| Standard | Used for |
| --- | --- |
| RFC 5905 | NTPv4 packets, timing and Kiss-o'-Death |
| RFC 7822 | NTPv4 extension fields |
| RFC 8915 | NTS key exchange, cookies and authenticated NTP |
| RFC 5297 | AES-SIV (AEAD 15, AEAD_AES_SIV_CMAC_256) |
| RFC 8446, RFC 5705 | TLS 1.3 and the keying-material exporter |

NTS uses TLS 1.3 only, through rustls, with ALPN `ntske/1`. The client-to-server and server-to-client keys come from the TLS exporter.

## Certificates and keys

Certificate verification is on by default. The tool trusts the Mozilla root set and the Windows certificate store. **Skip TLS certificate verification** turns off server identity checks and is for controlled lab use only.

Derived NTS keys are never logged. Logs can still name hosts, internal addresses and file paths, so review them before sharing.

## Replies the client rejects

- Replies from an address other than the one queried (the UDP socket is connected)
- Replies whose originate timestamp doesn't match the request, which also catches replays
- NTS replies whose unique identifier or authentication tag doesn't verify

## Timing accuracy

T1 and T4 are taken in userspace, not by the network card. Scheduling, path asymmetry and timestamp placement all affect results. The tool is for protocol diagnostics and interoperability testing, not a sub-microsecond reference.

## Windows

The installer installs per user and needs no administrator rights. The tool asks for elevation only on request, through the standard UAC prompt. The firewall rule it can add covers only the responder's UDP port, on Domain and Private profiles, never Public.

## Interoperability

Version 3.x has completed authenticated NTS runs against:

- Cloudflare's public `time.cloudflare.com`
- two independent ESP32-P4 NTS servers, with certificate verification on. Over 40 requests at a 2 s interval, all 40 replies authenticated with no loss, 8 cookies and a single key exchange.
