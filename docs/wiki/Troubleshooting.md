# Troubleshooting

## Key exchange

**NTS-KE timeout or connection refused.** Check that TCP 4460 (or the port you gave) is reachable, that no firewall blocks it, and that the server runs NTS-KE.

**ALPN failure.** The server didn't agree to `ntske/1`, so it isn't speaking NTS-KE on that port.

**No cookies returned.** The key exchange finished, but without the cookies authenticated NTP needs. Check the server's NTS configuration.

**NTS-KE WARNING record.** The server sent a warning. It's logged, and the exchange carries on.

## Certificates

With verification on, the **Server** you enter must match the certificate.

- **Not valid for this name or IP address.** Use the DNS name the certificate covers. To connect by IP address, the certificate needs that address as an IP Subject Alternative Name.
- **Unknown issuer.** The tool trusts the Mozilla root set and the Windows certificate store. For a lab server signed by a private CA, install that CA in Windows (Trusted Root Certification Authorities), then restart the tool.
- **Expired or not yet valid.** Check the certificate dates and this PC's clock.

**Skip TLS certificate verification** (`--insecure`) helps isolate certificate problems in a lab, but it removes the check that you're talking to the right server. Don't use it otherwise.

## Authenticated NTP

**NTS authentication failure.** The reply didn't verify with the server-to-client key. Check the server's AEAD handling and that it uses the keys from this key exchange.

**UID or originate mismatch.** The reply doesn't belong to the request it's being checked against, for example a late or replayed packet.

**NTSN Kiss-o'-Death.** The server couldn't decrypt the cookie, usually after a key rotation or restart. The tool repeats the key exchange by itself.

**Loss only at short delays.** It's probably the server's rate limit. See [Testing an NTS Server](Testing-an-NTS-Server#rate-limits).

## Plain NTP

**Kiss-o'-Death.** Stratum 0. The RefID gives the reason: RATE for rate limiting, DENY or RSTR for access refused.

**Reply from another address.** Ignored by design. The client only accepts replies from the address it queried.

**Unexpected timing.** Userspace timestamps, scheduling, path asymmetry and load all affect offset and delay. A steady offset usually means this PC's clock is off.

## Responder

**Can't bind UDP/123.** Windows Time or another NTP service holds it. Stop it, or use another port. See [Windows Firewall and Administrator](Windows-Firewall-and-Administrator#windows-time-and-udp123).

**The peer's requests never arrive.** Check that the peer uses the responder's address and port. Check that the **Windows** card shows the rule allowing that port, and that the network is on a Domain or Private profile. Check routing and VLANs between the two.

**A fault mode seems to do nothing.** Check that the log shows the request and the intended mode. Many faults are meant to be dropped silently by a correct client, so check the peer's own logs.

## Installer

**SmartScreen blocks setup.** The installer isn't code-signed yet. Choose **More info**, then **Run anyway**, or check the file's hash first; see [Installing](Installing#checking-a-download).

**"It can't be replaced."** The tool is running. Close it, then **Try again**.
