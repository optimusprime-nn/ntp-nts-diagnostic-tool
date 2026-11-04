# Changelog


## [3.1.0] - 2026-11-03

### Added
- A Windows installer, `NTP-NTS-Diagnostic-Tool-<version>-setup.exe`. It installs for the current user without administrator rights, adds a Start menu entry and an entry in Settings > Apps, and upgrades in place. `/S` installs silently, and `uninstall.exe /U [/S]` removes it. Each release also has a portable zip and `SHA256SUMS.txt`.
- The programs carry the app icon in Explorer, the taskbar and the Start menu.
- The 2.x launch prompts are back: restart as Administrator when not elevated, and add the firewall rule when elevated without one. Each has a **Don't ask at startup** box.
- The Windows card shows the firewall rule's live status. It warns when the rule allows a different port from the responder's, and when Windows Time holds UDP/123. It adds **Remove rule** and a refresh button.
- Declining the UAC prompt now says so, instead of reporting a ShellExecute error code.
- A website and wiki for 3.x, published automatically with every release.
- `NTPNTS_SCREENSHOT=<file.png>` runs an unattended NTS test and saves the window, for the docs.

### Fixed
- Kiss-o'-Death replies no longer also raise the "clock not synchronised" (LI=3) warning.
- The Windows card is visible again. Its button row was wider than the sidebar, which pushed the sidebar's scrollbar under the main area. The card sat below the fold at the default window size with nothing to show it was there. The row now wraps, and the scrollbar is always shown.

### Changed
- Releases are built by a new Release workflow: tests, programs, installer, an install-and-remove check, the release itself, then the website and wiki.

## [3.0.0] - 2026-11-03

Rewritten in Rust. Two standalone executables: the windowed `NTP-NTS-Diagnostic-Tool.exe` and the command-line `ntpnts.exe`.

### Added
- A redesigned window: a header with live status pills, a sidebar with Client, Responder and Windows cards, KPI tiles, live offset and delay charts with hover readout, and a card-style log console.
- Light, dark and follow-system themes, using native Segoe UI, Cascadia Mono and Segoe Fluent icons on Windows (egui's bundled fonts elsewhere).
- Settings (server, counts, NTS options, responder, theme, log file) are remembered between launches.
- Ctrl+Enter starts a run and Esc stops it. The window has an app icon and a progress bar during runs.
- `ntpnts` command line: `query`, `respond`, `self-test`, `modes`, with `--log` and exit codes.
- `host:port` and `[IPv6]:port` targets, so you can test NTS-KE on non-standard ports and query a responder on a test port.
- Statistics: loss, timeouts, authenticated count, and offset/delay last/mean/min/max/stddev/jitter.
- NTP details in the log: poll, precision, reference ID, root delay/dispersion, server processing time, and warnings for LI=3, stratum 16 and large dispersion.
- Log filter, colour coding, and a final summary for each run.
- The self-test now also checks that the client accepts, rejects or flags each fault mode as expected. Replay is detected through the originate-timestamp check.
- Trust anchors include the Windows certificate store as well as the Mozilla roots.
- Loopback integration tests and protocol unit tests (`cargo test`), including an NTS authenticate, tamper and UID-mismatch round trip.

### Fixed (bugs in 2.1.0)
- `short`, `bad-originate` and the `zero-t2`/`zero-t3` self-test checks used escaped `\x` byte strings, so the wire bytes and checks were wrong. `bad-originate` sent a 72-byte packet.
- `drop` and `delayed` modes replied immediately. `drop` now never replies and `delayed` replies after 1 s without blocking other requests.
- `replay` mode never resent its stored packet.
- On Windows the responder stopped on the first ICMP port-unreachable (`WSAECONNRESET`). That error is now ignored.
- The firewall rule always opened UDP/123. It now opens the configured responder port.
- NTS-KE warning records were fatal. They are now logged.
- An NTSN Kiss-o'-Death now discards the cookie pool and triggers a fresh NTS-KE.
- Offset and delay are calculated in 64-bit fixed point, so they are exact and safe across the 2036 NTP era rollover.
- Replies from addresses other than the target are no longer accepted (connected UDP socket).
- The responder no longer sets `SO_REUSEADDR`, which on Windows let it silently share UDP/123 with W32Time.

### Changed
- The startup elevation and firewall dialogs are now buttons in the responder panel, so launching the app no longer shows UAC prompts.
- The CI and `build_windows.bat` use Cargo instead of PyInstaller.

## [2.1.0] - 2026-09-20

- Added the NTP Peer Test Responder with fault injection and a loopback self-test suite.
- Added Windows elevation and firewall rule helpers.

## [2.0.2] - 2026-09-19

### Fixed
- Corrected TLS handshake helper regression introduced during timeout handling.
- Retained robust pyOpenSSL WantRead/WantWrite handling.

### Validated
- Successful end-to-end NTS interoperability with `time.cloudflare.com`.
- TLS 1.3 and `ntske/1` ALPN negotiation.
- AEAD AES-SIV-CMAC-256 authentication.
- NTS cookie replenishment and repeated authenticated replies.
- Successful independent ESP32-P4 NTS server interoperability.

## [2.0.1] - 2026-09-19

- Added pyOpenSSL nonblocking/timeout handling.
- Improved exception diagnostics.

## [2.0.0] - 2026-09-19

- Added real RFC 8915 NTS-KE.
- Added TLS exporter key derivation, cookies, Unique Identifier, authenticated NTP requests/replies, timing calculations, IPv4/IPv6 resolution, and stricter NTP validation.
