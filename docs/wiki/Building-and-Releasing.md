# Building and Releasing

## Building

Install Rust from <https://rustup.rs>, then:

```text
cargo test
cargo build --release
```

The programs are written to `target\release\`. `build_windows.bat` runs the same steps and copies them to `dist\`.

The installer embeds the release programs, so build it afterwards and point it at them:

```text
set NTPNTS_GUI_EXE=%CD%\target\release\NTP-NTS-Diagnostic-Tool.exe
set NTPNTS_CLI_EXE=%CD%\target\release\ntpnts.exe
cargo build --release -p ntpnts-setup
```

That writes `target\release\setup.exe`.

## Screenshots

The screenshots on the website and in this wiki come from the tool itself. It runs an unattended NTS test, saves the window and exits:

```text
set NTPNTS_SCREENSHOT=docs\site\screenshots\v3-dark.png
set NTPNTS_SCREENSHOT_THEME=dark
target\release\NTP-NTS-Diagnostic-Tool.exe
```

`NTPNTS_SCREENSHOT_THEME` can be `dark` or `light`, and `NTPNTS_SCREENSHOT_TARGET` picks the server (default `time.cloudflare.com`). A screenshot run never reads or changes your saved settings.

## Releasing

1. Set the version in `Cargo.toml` (`[workspace.package]`) and `VERSION`, and add a section to `CHANGELOG.md`.
2. Commit, then push a tag that matches the version:

   ```text
   git tag v3.1.0
   git push origin v3.1.0
   ```

The **Release** workflow then:

- checks that the tag matches the version, then runs the formatting check, clippy, the tests and the self-test
- builds the programs and the installer, and checks that each one carries the icon
- publishes the setup, the portable zip and `SHA256SUMS.txt` as a GitHub release. A version with a hyphen, such as `3.2.0-beta.1`, is marked as a pre-release
- copies the release, with the same files and notes, to the public repository's releases page, which is where the website's download buttons point
- updates the website and this wiki from `docs/site` and `docs/wiki`, filling in the version and date

The website, wiki and public releases live in the public repository optimusprime-nn/ntp-nts-diagnostic-tool. Publishing to it needs the `DOCS_PAT` secret: a fine-grained token with **Contents: read and write** on that repository. Without the secret, the release still goes out and the copy and docs steps are skipped with a warning.

Running the workflow by hand does everything except publish the release, and leaves the files on the run. Set **publish_docs** to update the site and wiki too.
