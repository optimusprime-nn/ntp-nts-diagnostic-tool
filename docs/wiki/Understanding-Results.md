# Understanding Results

## NTP header

| Field | Meaning |
| --- | --- |
| **LI** | Leap indicator. 3 means the server's clock is not synchronised |
| **VN**, **Mode** | Version (4) and mode (4 = server) |
| **Stratum** | 1 for a reference clock, 2–15 further down. 0 is a Kiss-o'-Death, 16 is unsynchronised |
| **Poll**, **Precision** | The server's poll interval and clock precision, as powers of two |
| **RefID** | The reference: a clock name at stratum 1, an upstream address below it, or the Kiss-o'-Death code |
| **RootDelay**, **RootDisp** | Round-trip delay and dispersion back to the reference clock |

## Timing

| Value | How it's calculated |
| --- | --- |
| **offset** | ((T2 − T1) + (T3 − T4)) / 2: how far this PC's clock is from the server's |
| **network delay** | (T4 − T1) − (T3 − T2): the round trip, without server time |
| **server processing** | T3 − T2 |
| **jitter** | The root-mean-square difference between successive offsets (or delays) |

T1 and T4 are this PC's send and receive times. T2 and T3 are the server's receive and transmit times. Values are computed in 64-bit NTP fixed point, so they're exact and safe across the 2036 era rollover.

A steady offset of a few milliseconds usually means this PC's clock is off, not the server's. That's especially likely when the Windows Time service is stopped.

## Counters

| Counter | Counts |
| --- | --- |
| **Sent**, **Received** | Requests sent and valid replies received |
| **Loss** | Requests without a valid reply, as a percentage |
| **Failures** | Every failed request, including timeouts |
| **Timeouts** | Requests that got no reply in time (3 s by default) |
| **NTS failures** | Key exchange or authentication failures |
| **Kiss-o'-Death** | Stratum-0 replies |
| **Authenticated** | NTS replies whose UID and authentication tag verified |

In a clean run, **Received** equals **Sent**, and with NTS **Authenticated** equals both. Every other counter is zero. That shows the protocol worked for every exchange in the run. It says nothing about sub-millisecond accuracy; see [Security and Technical Notes](Security-and-Technical-Notes).

## Summary

Each run ends with summary lines you can paste into a report:

```text
SUMMARY: sent 2 | received 2 | loss 0.0% | failures 0 (timeouts 0) | NTS failures 0 | KoD 0 | authenticated 2 | NTS-KE exchanges 1
SUMMARY offset: mean -4.681 ms, min -4.818, max -4.544, stddev 0.194, jitter 0.274 (n=2)
SUMMARY delay:  mean +18.722 ms, min +18.601, max +18.844, stddev 0.172, jitter 0.243 (n=2)
```
