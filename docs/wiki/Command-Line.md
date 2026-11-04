# Command Line

`ntpnts.exe` runs the same engine as the window, for scripts, CI and remote sessions. The installer puts it next to the main program; see [Installing](Installing#using-ntpnts-from-any-prompt).

```text
ntpnts query <SERVER> [--nts] [--insecure] [-n COUNT] [-d DELAY_MS] [--timeout MS]
ntpnts respond [--bind ADDR] [--port PORT] [--mode MODE]
ntpnts self-test
ntpnts modes
```

Every command accepts `--log FILE` to append the log to a file. Exit code 2 means the log file could not be opened.

| Command | What it does | Exit code |
| --- | --- | --- |
| `query` | Queries an NTP or NTS server. Defaults: 5 requests, 1000 ms apart, 3000 ms reply timeout | 1 if any request failed |
| `respond` | Runs the fault-injecting responder until Ctrl+C. Defaults: `0.0.0.0`, port 123, mode `valid` | 2 if it can't start |
| `self-test` | Runs the loopback responder suite | 1 on any failure |
| `modes` | Lists the fault modes | 0 |

`SERVER` is `host`, `host:port` or `[IPv6]:port`. With `--nts` the port is the NTS-KE port (default 4460); otherwise it's the NTP port (default 123). `--insecure` skips TLS certificate verification. Use it for lab servers only.

## Examples

```text
ntpnts query time.cloudflare.com --nts -n 10 -d 500
ntpnts query 192.168.1.50:4460 --nts --insecure     # lab server with a self-signed certificate
ntpnts query pool.ntp.org                           # plain NTPv4
ntpnts query 127.0.0.1:12300 -n 3                   # your own responder on another port
ntpnts respond --port 12300 --mode kod-deny
ntpnts self-test
```

In a CI job, a failed query fails the step:

```yaml
- run: ntpnts query time.cloudflare.com --nts -n 5 --log nts.log
```
