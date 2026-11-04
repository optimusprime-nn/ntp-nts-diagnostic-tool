# Peer Test Responder

The responder is a small NTP server that answers with deliberately faulty replies. Point an NTP client or peer at it to see how that client handles bad time sources.

## Running it

In the **Peer test responder** card, set the **Bind address** (`0.0.0.0` for every interface, `127.0.0.1` for this PC only), the **UDP port** and the **Response mode**, then click **Start**. The settings are locked while it runs; stop it to change them. Each request is logged with its source, size, the mode used and what was sent.

UDP/123 is the standard port. On Windows, though, the Windows Time service usually holds it, and binding it may need Administrator. For lab tests where the client can use any port, use another one, such as 12300. For other machines to reach the responder, Windows Firewall must allow the port; see [Windows Firewall and Administrator](Windows-Firewall-and-Administrator).

From the command line:

```text
ntpnts respond --port 12300 --mode kod-deny
```

## Fault modes

| Mode | Reply |
| --- | --- |
| `valid` | Well-formed stratum-1 reply |
| `kod` | Kiss-o'-Death RATE (stratum 0) |
| `kod-deny` | Kiss-o'-Death DENY |
| `kod-rstr` | Kiss-o'-Death RSTR |
| `bad-originate` | Originate timestamp does not echo the request |
| `short` | 16-byte truncated packet |
| `duplicate` | Every reply carries the same transmit timestamp (T3) |
| `wrong-mode` | Client mode 3 instead of server mode 4 |
| `bad-version` | Version number 0 |
| `zero-t2` | Receive timestamp (T2) is zero |
| `zero-t3` | Transmit timestamp (T3) is zero |
| `li-alarm` | Leap indicator 3 (unsynchronised) |
| `stratum-16` | Stratum 16 (unsynchronised) |
| `drop` | Never replies |
| `delayed` | Replies after 1 second, without holding up other requests |
| `offset-plus-100ms` | Server clock 100 ms ahead |
| `offset-minus-100ms` | Server clock 100 ms behind |
| `processing-100ms` | 100 ms between T2 and T3 |
| `replay` | Resends the first reply verbatim to every request |

A correct client should reject or flag most of these. Compare the responder log with the client's own logs to see what it did.

## Self-test

**Run self-test suite** (or `ntpnts self-test`) runs every mode over loopback on a spare port. Each check confirms the bytes on the wire. It also confirms that this tool's own client accepts, rejects or flags the reply as expected:

```text
=== PEER RESPONDER SELF-TEST: 19 checks ===
[PASS] valid | ...
...
=== PEER RESPONDER SELF-TEST COMPLETE: 19 passed / 0 failed ===
```

A pass shows the responder produced the intended fault. It doesn't show that some other NTP implementation handles that fault correctly. To check that, point the other client at the responder.

## Scope

The responder injects faults into plain NTP. It doesn't inject NTS faults such as a bad UID, a bad authenticator, stale cookies or NTS-KE errors.
