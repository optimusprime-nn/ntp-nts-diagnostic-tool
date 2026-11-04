# Version 2.x

Versions 2.0.2 and 2.1.0 were written in Python with a Tkinter window. They remain available from the [releases page](https://github.com/optimusprime-nn/ntp-nts-diagnostic-tool/releases), and their source stays in [optimusprime-nn/ntp-nts-diagnostic-tool](https://github.com/optimusprime-nn/ntp-nts-diagnostic-tool). Version 3.x is a Rust rewrite, with its source in [Optimusprimeums/ntp-nts-diagnostic-tool-rs](https://github.com/Optimusprimeums/ntp-nts-diagnostic-tool-rs).

| Version | Notes |
| --- | --- |
| 2.0.2 | The original NTS interoperability baseline, validated against `time.cloudflare.com` and an ESP32-P4 NTS server |
| 2.1.0 | Added the peer test responder, the self-test suite, startup elevation and firewall prompts, **Copy Log** and **Clear Log**, and a headless Linux command line |

## Moving to 3.x

| 2.x | 3.x |
| --- | --- |
| A single PyInstaller executable | An installer or a portable zip, with `NTP-NTS-Diagnostic-Tool.exe` and `ntpnts.exe` |
| **Start Requests** | **Start**, or Ctrl+Enter |
| Linux `NTP-NTS-Diagnostic-Tool query ...` | `ntpnts query ...` |
| Linux `NTP-NTS-Diagnostic-Tool peer ...` | `ntpnts respond ...` |
| `--skip-tls-verify` | `--insecure` |
| Firewall rule always for UDP/123 | Rule for the responder's configured port |

3.x also fixes several 2.1.0 bugs. In 2.1.0 the `short`, `bad-originate`, `drop`, `delayed` and `replay` modes were wrong. The responder stopped after an ICMP port-unreachable. NTS-KE warning records were treated as fatal. See the [Changelog](Changelog).
