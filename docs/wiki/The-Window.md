# The Window

![The window in the dark theme](https://optimusprime-nn.github.io/ntp-nts-diagnostic-tool/screenshots/v3-dark.png)

![The window in the light theme](https://optimusprime-nn.github.io/ntp-nts-diagnostic-tool/screenshots/v3-light.png)

The header shows the version and live status pills for the client and the responder. **Theme** switches between follow-system, light and dark. The window remembers its settings, theme and size between launches.

The sidebar holds three cards. If the window is too short for all three, the sidebar scrolls; its scrollbar is always shown.

## Client

| Control | What it does |
| --- | --- |
| **Server** | `host`, `host:port` or `[IPv6]:port`. With NTS on, the port is the NTS-KE port (default 4460). Otherwise it's the NTP port (default 123) |
| **Requests**, **Delay** | How many requests to send, and the wait between them |
| **Use NTS (RFC 8915, TCP 4460)** | Key exchange over TLS 1.3, then authenticated NTP |
| **Skip TLS certificate verification** | For lab servers only. See [Security and Technical Notes](Security-and-Technical-Notes) |
| **Log to file**, **Choose…** | Also append every log line to a file |
| **Start** / **Stop** | Run or stop. **Ctrl+Enter** starts and **Esc** stops |

## Peer test responder

A fault-injecting NTP server for testing other NTP clients and peers. Pick a **Bind address**, **UDP port** and **Response mode**, then **Start**. **Run self-test suite** checks every mode over loopback. See [Peer Test Responder](Peer-Test-Responder).

## Windows

The Windows card shows whether the tool is running as Administrator. It also shows whether a Windows Firewall rule lets other machines reach the responder port, and warns when the Windows Time service holds UDP/123. Its buttons:

| Button | What it does |
| --- | --- |
| **Restart as Administrator** | Restarts the tool through the normal UAC prompt. Shown only when not elevated |
| **Allow UDP/*port*** | Creates or refreshes the inbound rule for the responder port (Administrator only) |
| **Remove rule** | Deletes that rule (Administrator only) |
| Refresh | Re-reads the firewall and W32Time status |

See [Windows Firewall and Administrator](Windows-Firewall-and-Administrator).

## Counters

**Sent**, **Received**, **Loss**, **Failures**, **Timeouts**, **NTS failures**, **Kiss-o'-Death** and **Authenticated** update live during a run.

## Timing

Charts of offset and delay for each reply, with min and max. Hover a chart to read individual replies. The table below gives the last, mean, min, max, standard deviation and jitter for offset and delay.

## Log

A colour-coded log of every step: key exchange, packets sent and received, NTP header fields, timing and authentication. **Filter** shows only matching lines. **Copy** copies what's shown and **Clear** empties the display; neither touches the log file. Each run ends with a **SUMMARY**.

The status bar shows where the log is being written, whether you are Administrator, and the shortcuts.
