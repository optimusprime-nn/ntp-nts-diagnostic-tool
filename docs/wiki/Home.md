# NTP/NTS Diagnostic Tool

A diagnostic tool for **NTPv4** and **Network Time Security (RFC 8915)** servers, with a fault-injecting NTP responder for testing NTP clients and peers.

Version 3.1.0 is written in Rust. It has a Windows installer, needs no runtime, and ships as two programs:

| Program | What it is for |
| --- | --- |
| `NTP-NTS-Diagnostic-Tool.exe` | The window: client, responder, live charts and log |
| `ntpnts.exe` | The same engine on the command line, for scripts and CI |

The source code is at [Optimusprimeums/ntp-nts-diagnostic-tool-rs](https://github.com/Optimusprimeums/ntp-nts-diagnostic-tool-rs), where you can also [report an issue](https://github.com/Optimusprimeums/ntp-nts-diagnostic-tool-rs/issues). Downloads are on the [releases page](https://github.com/optimusprime-nn/ntp-nts-diagnostic-tool/releases).

![The NTP/NTS Diagnostic Tool after an authenticated NTS run](https://optimusprime-nn.github.io/ntp-nts-diagnostic-tool/screenshots/v3-dark.png)

## Start here

- [Installing](Installing): the installer, the portable zip, upgrading and removing
- [Getting Started](Getting-Started): your first NTS test in a minute
- [The Window](The-Window): every card, button and counter

## Testing servers

- [Testing an NTP Server](Testing-an-NTP-Server)
- [Testing an NTS Server](Testing-an-NTS-Server)
- [Understanding Results](Understanding-Results)
- [Stability Testing](Stability-Testing)

## Testing clients and peers

- [Peer Test Responder](Peer-Test-Responder): 19 fault modes and the self-test suite
- [Windows Firewall and Administrator](Windows-Firewall-and-Administrator)

## Reference

- [Command Line](Command-Line)
- [Troubleshooting](Troubleshooting)
- [Security and Technical Notes](Security-and-Technical-Notes)
- [Building and Releasing](Building-and-Releasing)
- [Changelog](Changelog)
- [Version 2.x](Version-2.x): the earlier Python releases
